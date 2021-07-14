# certbot

## simple wildcard certificate (nginx)

Create a cloudfare.ini file, e.g. ~/.secrets/certbot/cloudflare.ini
```
# Cloudflare API credentials used by Certbot
dns_cloudflare_email = cloudflare@example.com
dns_cloudflare_api_key = 0123456789abcdef0123456789abcdef01234567

```
Install the cloudflare plugin (ubuntu 18.04)
```
sudo apt install python3-certbot-dns-cloudflare
```
Run the certbot command
```
sudo certbot \
  --dns-cloudflare \
  --dns-cloudflare-credentials ~/.secrets/certbot/cloudflare.ini \
  -i nginx  \
  -d "*.example.com" \
  --server https://acme-v02.api.letsencrypt.org/directory
```
```
certbot \
  --dns-cloudflare \
  --dns-cloudflare-credentials /home/magento/.secrets/certbot/cloudflare.ini \
  -i nginx  \
  -d "example.com" \
  -d "*.example.com"
```