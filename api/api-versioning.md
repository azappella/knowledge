# Pragmatic API Versioning
15 May 2017 on API, HTTP Header Routing

Source: https://web.archive.org/web/20170610175619/https://fly.io/articles/pragmatic-api-versioning/

API design is a spicy topic! Many a wise word 1, 2, 3, 4 has been written about the best way to structure and version an API. Within this article we'll take a dip into the conflicting schools of API-design, establish a pragmatic middle-ground, then demonstrate how we can use Fly to bring our middle-ground to life.
API Versioning

While there is no agreed upon right way to design an API, it will be helpful to identify a few key ideas that many developers agree upon. A well-structured web API should be...

1. An ongoing contract with the client. The contract guarantees consistency and stability; the client should be able to use the API without concern that it will suddenly break or vanish.

2. Backwards compatible after a change or upgrade. Old queries to new endpoints should still generate the expected return.

3. RESTful. It should speak the language of HTTP verbs: GET, PUT, POST, PATCH, DELETE, etc.

To best check these desirable boxes, there are divergent philosophies on how to implement differing versions of the API. Let's look at three of them...
URI Versioning

```
curl https://example.com/api/v2/lists/3  
```

By plunking down the version number in the URI, the client can access /v1/ or /v2/ of the API. It's readable, adaptable, and can plugin directly in to a users' browser.
Header Versioning
```
curl https://example.com/api/lists/3 \  
-H 'Accept: application/vnd.example.v2+json'
```

The API URI remains the same. Header versioning in the wild is primarily done via a customized Accept HTTP header. The core URI remains the same to best represent the API's resources and version changes are passed through within the header along with the response type.
No Versioning!
```
curl https://example.com/api/lists/3  
```

What do you need to version for!? Let's just extend our API to fit new or adjusted cases! Leave old scaffolding in place and opt to build and extend.
The Middle Ground

Each method is justifiable and - when designed well - can render an excellent API. Ultimately, we want to be practical and ship things as quickly as we can. An ever-green API with no versioning whatsoever has the potential to get messy, so we'll stay away from that. Instead, we'll accept versioning in both our URI and our HTTP header. As a twist, we'll use a customized HTTP header.

Before we panic about how muddled and verbose our requests might look, let's get a little deeper into why we would use this approach. Our method is based on the adage that says: As sure as the sun will rise, your application will change.

Let's consider an example.
MightyList!

We're building an API for our fictional application MightyList. In its humble and early days, Mighty List allows users to create lists. It has an API presented from a URI as mightyapp.com/api/v1/ . You can use the API to request list information, like so:

```
curl https://mightyapp.com/api/v1/lists/3

...

{
  "listId": "3",
  "shopping": "Shoes, tie, umbrella, snorkel",
  "leisure": "Skiing, surfing, snorkeling ",
  "food": "bananas, peanut butter, spinach",
  "cost": "One hundred dollars"
}

```
When developing our applications, there are two types of changes we can expect: minor changes and major revisions. Let's look at a minor change using our example response above, where a GET request to list 3 returned string values for shopping, leisure, food, and cost. Our development team wants to adjust the data model and cost will now be an integer instead of a string.
```
curl https://mightyapp.com/api/v1/lists/3

...

{
  "listId": "3",
  "shopping": "Shoes, tie, umbrella, snorkel",
  "leisure": "Skiing, surfing, snorkeling ",
  "food": "bananas, peanut butter, spinach",
  "cost": 100
}
```
This change disrupts the backwards compatibility of our API! If someone has incorporated the MightyList API into their application, then receiving an integer for cost instead of a string may cause their application to break. To prevent this frustration, any minor change that disrupts our backwards compatibility should receive a version bump.

Another case to ponder is the major revision: MightyList Version 2 is on the horizon. lists will become superlists and we have many new resources being added: bots, ai -- that sort of fancy stuff. This major upgrade will certainly break backwards compatibility; it's fundamentally a new application!

Both of these examples expose how challenging it would be to keep our URL up to date while improving our API. Should a minor revision bump our URL version up? We'd wind up with many versions! We want to remain backwards compatible, but it can be challenging with an evolving application.

Before we address this problem, let's define what a safe and backwards compatible API change might look like:

```
    Adding a new resource.
    Adding a new event type.
    Adding new properties to responses.
    Changing the order of properties.
    Adding optional parameters to existing methods.
```

When adding new things via options or entirely new resource or event parameters, you can rest assured that the stability contract you have formed with your client will remain unbroken. They can continue accessing API resources and they will respond as expected. If we change the existing resources or alter expected response content, we need to represent that change within our API version.

```
    To make the state of our application clear, we want to have a version in our URI that represents the base product version; the URI version will change when your product fundamentally changes. MightyList V1 uses /api/v1/. MightyList V2 uses /api/v2/.

    To make the current version of our API clear, we'll use a customized HTTP Header to represent minor revisions. This would be akin to having your users request application version V2.X, where X is the API version.
```

Versioning in two places is a pragmatic approach; your application will iterate and there's always potential for metamorphosis into something greater. Let's look at how we can do this easily with Fly.

### Superfly API Versioning

Within Fly, each site is made up of multiple backends. You may have a static-page on GitHub for your marketing page, a Kubernetes cluster or Heroku deployment for your application, and a database management system or two for your data.

When you construct an API, you receive scalability and load balancing benefits by decoupling your API and hosting it as its own backend. API decoupling also gives us the opportunity to version with relative ease.

To begin, we'll add a new backend for our original API, /api/v1/. We'll specify the Path to match as /api/v1/; if we have multiple redundant instances to load balance, we can add them as separate backends, set the same path, then adjust the priority. If we wanted to get fancy, we could use Fly Middleware to route users to our various API backends based on device or geographic information.

Above we've created a baseline API endpoint. We'll also want to create a backend for the latest version of API V1. We'll date the endpoint 04-05-2017. After that, we can setup our API routing rules.

Now, we want to configure Fly Routing Rules for our second backend so that users who attach the custom header will receive the latest version of the API. We'll use the header name API-Version as it's explicit, memorable, and clear. The API-Version header will expect values in the form of day-month-year, or 04-05-2017. Doing this will allow us to create a public changelog for the API; your user's can pass in change dates to reach the endpoint they need.

Radical! We've set the API-Version to 04-05-2017 and we've chosen to route to the API V1 - 04-05-2017 backend when those headers are present. Our users now have the option to request a specific point-in-time of our API, while retaining the ability to reach the original API V1 from the example.com/api/v1 URI. You can see this by looking at the original routing rule for our first API backend:

### Summary

Let's recap: We have endpoints that represent different versions of our API. Our users can query the original API or add a customized header to receive a specific version. If our application receives a major revision, we'll ceremonially bump our URI to V2.

As we make iterative improvements, we'll create endpoints coinciding with the date of our changes and allow our users to attach dated headers to reach them. We can then choose how long to keep older versions alive and gracefully sunset them as our users adapt, without worrying about backwards compatibility.

There are many different philosophies that you can apply when designing and versioning your API. Using this example case, you can see how the flexible power of Fly header-based or URI backend routing can enable you to create the API architecture and versioning scheme of your wildest dreams*.


* Please note, if you're actually dreaming of APIs, you probably need a vacation. If you aren't able to take one, perhaps breath deeply and stare at this GIF for a few hours. That way you might dream about Krabby Pattys, instead.