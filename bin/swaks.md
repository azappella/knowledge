# swaks

small cli to send emails via smtp

```
swaks --auth \
    --server smtp.eu.mailgun.org \
    --port 587 \
    -au username@example.com \
    -ap password \
    -tls
    --from from@example.com \
    --to to@example.com \
    --h-Subject: "Hello" \
    --body 'Testing some smtp mailing via swaks'
```
