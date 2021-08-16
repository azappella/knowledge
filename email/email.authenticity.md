# Email Authenticity 101: DKIM, DMARC, and SPF

source: https://www.alexblackie.com/articles/email-authenticity-dkim-spf-dmarc/

Published August 15 2021

Password resets, two factor codes, business secrets, private  conversations… Email is at the centre of most of life and   business, and so we must ensure it is trustworthy and authentic.

If you use email with your own domain, a lot of the burden of authenticity has suddenly shifted from your service provider to you. This guide will hopefully give you the information and practices you need to keep your domain's email authentic and less vulnerable to spoofing.

We'll cover the three major components of modern email domain security: **DKIM** for signing, **SPF** for sender verification, and **DMARC** for stricter enforcement of the other two. It is assumed the reader has a basic understanding of DNS and has experience using email with their own domain.

## 1. SPF

SPF, or Sender Policy Framework, is one of the most basic email verification technologies, and is the easiest and more common protection. Often service providers will give you the DNS record contents you need to simply copy-paste during setup.

It takes the form of a DNS `TXT` record on whatever domain you are sending email from. It looks something like this:

```
"v=spf1 include:spf.messagingengine.com -all"
```

  **Note**:   Keep in mind that SPF only supports 10 recursive lookups  (`a`, `mx`, `ptr`,  `exists`, or `include` are recursive). However,   if you need more than this, it's probably a symptom of much worse IT  bloat.

At its core, SPF is just a list of IP addresses that are authorized to send email from your domain. This can be of a few different forms:

- Most commonly, `include:` which does a recursive lookup     to include all the IPs from a different hostname.   
- Also common is `ip4:` and `ip6:` for    referencing IP addresses directly.  
- There are a few others (`a`, `mx`,    `ptr`, `exists`), but they are not generally    used in normal circumstances so I'm ignoring them for now.  
- Finally we have the special `all` mechanism, which is a     wildcard catch-all that matches, well, all IP addresses. This is    primarily used to blocklist everything by default, which we'll    explain more below. 



For our example above, `spf.messagingengine.com` has a couple dozen `A` records on it which are included for this SPF policy via the `include:` mechanism.

Other than the IPs and included hostnames, we have a qualifier, which is [one of a few symbols](https://datatracker.ietf.org/doc/html/rfc7208#section-4.6.2) that prefix a mechanism.

Each symbol recommends a different policy to a mail server that tells it what to do if it receives a message from your domain from that IP. By default, with no symbol, it is considered equivalent to `+`, which is a "pass."

In our example, we have two mechanisms:

1. `include:spf.messagingengine.com`, which includes all    the IPs for Fastmail, and has no qualifier, making it an implicit     "pass all messages from these IPs."   
2. `-all`, which is a fallback with the "reject" qualifier,     instructing the receiver to "fail all messages from any other IP." 



And that is all there is to it. SPF is a very simple tool, but provides the very base level of email verification ("what IPs are allowed to send my email") necessary to do basic spam filtering. Even just setting up SPF alone should help significantly with your delivery success.

## 2. DKIM

DKIM, or Domain Keys Identified Mail, is another security mechanism that uses asymmetric keys to cryptographically verify the server sending email for your domain is authorized to do so. With DKIM configured, the server receiving your mail can look up the public key in DNS and validate the email was legitimately sent from your domain.

DKIM protects against IP's changing hands, or large service providers sharing IP space between customers. If you say, "Google IPs can send my email", what's stopping someone else from spoofing email from your domain and sending it from their own Google account? Since those IPs are shared, it will still pass SPF checks, but it *will not* pass DKIM.

There are two main pieces to DKIM: a DNS record with the public key, and a header added to every sent email with cryptographic signatures and details on how to find the aforementioned DNS record.

The DNS record is just a normal `TXT` record, but under a special name. The generic format is:

```
<selector>._domainkey.<domain>
```

The "selector" is generally set by your email service provider and will be provided to you when you enable DKIM with them. Some providers, like Fastmail and Microsoft 365, even provide multiple selectors for you to set. For example, for Google it's just one, "`google`":

```
google._domainkey.example.com
```

Lately these domain key records have been moving to `CNAME`'s to a service provider domain to make configuration easier, and allow the service provider to rotate keys without making you update your DNS. For example, both Fastmail and Microsoft 365 have switched to `CNAME`, though Google is still providing keys directly.

These selectors and DNS records don't mean much unless the receiving server knows how to find them, though. Which brings us to the DKIM headers.

DKIM headers are added to every email you send, and contain [a few important details](https://datatracker.ietf.org/doc/html/rfc6376#section-3.5). The two major ones we're interested in here are:

1. `**d=<domain>**` lists the sender's domain name for verification.
2. `**s=<selector>**` is the "selector", matching the DNS subdomain.



With these two pieces of metadata, the receiving server can rebuild the subdomain containing the DKIM key and resolve it. Taking this key, they can then cryptographically verify the DKIM signature and validate if the message is authentic or not.

DKIM is a much stronger signal than SPF for detecting spam, as there is actual maths involved and not just an IP list. If all you do is configure SPF and DKIM, you're in a pretty good place, however we can take it a step further.

## 3. DMARC

You may have noticed that DKIM only applies if there is a header in the message. This means that illegitimate messages will not have the header, and thus no DKIM validation will happen. This results in the DKIM validation being "neutral" instead of a "failure", as it was simply omitted.

Adding a DMARC policy allows us to

- enforce SPF and DKIM checks on *all* emails claiming to be from our domain;
- give hints to the receiving server on how to handle failed checks;
- provide a reporting address so we can receive reports about these checks in the wild from email providers.



A DMARC record is the same format as the other two, and is also pretty simple. Here's an example of a very basic, permissive policy:

```
v=DMARC1; p=none; adkim=r; aspf=r;
```

Please don't actually use this policy, it won't do anything for you. But it gives us a place to start explaining all the bits.

- **`p=none`** sets our "global policy" for handling emails that fail authentication checks. This can be `none`, `quarantine`, or `reject`. 
- **`adkim=r`** sets our policy for enforcing DKIM checks. This can be `r` ("relaxed") or `s` ("strict"). 
- **`aspf=r`** sets our policy for enforcing SPF checks. This takes the same values as `adkim`.

Before we get to locking things down, we need to talk about reporting. One of the most important things to do is figure out how you're going to monitor your DMARC reports. There are many services that do this for you; I use [Postmark's free DMARC reports](https://dmarc.postmarkapp.com) which sends you weekly digests, but I have used other paid tools in the past and would recommend going for one of those if you are a business and need more advanced insights.

Then you can set a permissive policy with one extra value: `rua`, which provides an email address for the providers receiving your email to send reports back to you. For example:

```
v=DMARC1; p=none; adkim=r; aspf=r; rua=mailto:re+someidentifier@dmarc.postmarkapp.com;
```

This policy does not enforce any checks, but does set a report address. This will give you insight into where your mail is currently coming from. You don't want to start out of the gate with a locked-down policy and accidentally start blocking legitimate email from a source you forgot about.

Once you are confident you know all the legitimate email sources, you can start tightening the policy. A good next step would be to set the policy to quarantine, to hint that failures should be considered spammy.

```
v=DMARC1; p=quarantine; adkim=r; aspf=r; rua=mailto:re+someidentifier@dmarc.postmarkapp.com;
```

Let that sink in, and keep and eye on your reports for a bit. If you are confident it all looks good, you can move to marking SPF and DKIM validation as "strict" to spam-ify failing emails with more prejudice:

```
v=DMARC1; p=quarantine; adkim=s; aspf=s; rua=mailto:re+someidentifier@dmarc.postmarkapp.com;
```

At this point, you have a very strong policy, and this is a perfectly fine place to leave it. If you are particularly paranoid, you can move up to `p=reject` which will instruct the receiving servers to throw out all email that fails authenticity checks, keeping it out of the recipient's mailbox entirely. This can be a bit dangerous, as there is no recourse if legitimate email accidentally fails a check, it's just gone forever, but is technically the most secure policy.

DMARC is the most powerful piece of modern email security, and its reporting can be incredibly insightful into what spam is out there pretending to be you. If you take anything from this guide, I hope it is that you should take the time and care to set a strict DMARC policy.

## Bonus: domains without email

SPF and DMARC don't just apply to domains with email configured. A good practice is to configure highly-restrictive blocking policies for any domains you have that do not send email. This will ensure anyone trying to spoof email as that domain gets blocked immediately.

For all of my non-email domains I set this "block all" SPF record:

```
v=spf1 -all
```

And I set a DMARC policy that hard-rejects all email that fails SPF or DKIM (which will be everything):

```
v=DMARC1; p=reject; adkim=s; aspf=s;
```

Just these two empty-looking records are enough to prevent any spam being sent in your name from a domain you didn't even expect to send email.