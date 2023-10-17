This repository is for using cert manager to sign certificates from Godaddy.

To use it, a values.yaml of the following form is required:

```yaml
# Default values for godaddy-certs.
# This is a YAML-formatted file.
# Declare variables to be passed into your templates.

certificates:
  secretName: wildcard-foundationmodels-io-tls
  hosts:
    - 'put.com'
    - 'your.io'
    - 'multiple.dev'
    - 'domains.ai'
godaddy:
  apiKey: "godaddy_api_key:godaddy_secret"
acmeProps:
  environment: staging # can change to production
  email: email@email.com
  # prod : https://acme-v02.api.letsencrypt.org/directory
  # staging : https://acme-staging-v02.api.letsencrypt.org/directory
  server: https://acme-staging-v02.api.letsencrypt.org/directory
```

Note that there is a staging server for acme, so you need to update the link to point to the prod, and then update from staging to production. Also you need godaddy api key. This will then create the TXT records for the domains, verify with ACME and issue the secret with the TLS cert and key under the secret with secret-name above.

Also note that if you do staging, then it will hang in requested, but you can verify the TXT records have been created in Godaddy console or with dig, or nslookup.

```shell
dig [url] +short txt
```
