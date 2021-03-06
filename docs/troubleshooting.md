# Troubleshooting

Generally if the following information doesn't provide a solution to your problem please take a look at existing issues (search for keywords) before creating a new one.

## "No registration exists matching provided key"

You probably changed from staging-CA to production-CA (or the other way).

Currently dehydrated doesn't detect a missing registration on the selected CA,
the current workaround is to move `private_key.pem` (and, if you care, `private_key.json`) out of the way so the scripts generates and registers a new one.

This will hopefully be fixed in the future.

## "Provided agreement URL [LICENSE1] does not match current agreement URL [LICENSE2]"

Set LICENSE in your config to the value in place of "LICENSE2".

LICENSE1 and LICENSE2 are just placeholders for the real values in this troubleshooting document!

## "Error creating new cert :: Too many certificates already issued for: [...]"

This is not an issue with dehydrated but an API limit with boulder (the ACME server).

At the time of writing this you can only create 5 certificates per domain in a sliding window of 7 days.

## "Certificate request has 123 names, maximum is 100."

This also is an API limit from boulder, you are requesting to sign a certificate with way too many domains.

## Invalid challenges

There are a few factors that could result in invalid challenges.

If you are using HTTP validation make sure that the path you have configured with WELLKNOWN is readable under your domain.

To test this create a file (e.g. `test.txt`) in that directory and try opening it with your browser: `http://example.org/.well-known/acme-challenge/test.txt`. Note that if you have an IPv6 address, the challenge connection will be on IPv6. Be sure that you test HTTP connections on both IPv4 and IPv6. Checking the test file in your browser is often not sufficient because the browser just fails over to IPv4.

If you get any error you'll have to fix your web server configuration.
