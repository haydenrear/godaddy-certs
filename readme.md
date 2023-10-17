This repository is for using cert manager to sign certificates from Godaddy. It simply uses Godaddy Webhook and cert manager.

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

Note that there is a staging server for acme, so you need to update the link acmeProps.server to point to the prod uri, and then update acmeProps.environment from staging to production als. Also you need godaddy api key and secret, created in Godaddy console. This will then create the TXT records for the domains, verify with ACME and issue the secret with the TLS cert and key under the secret with secret-name above if in Prod, or if in staging, it will hang (check with kubectl describe Challenge) after updating the TXT records.

Also note that if you do staging, then it will hang in requested, but you can verify the TXT records have been created in Godaddy console or with dig, or nslookup.

```shell
dig [url] +short txt
```

to check to make sure the TXT records are there.
