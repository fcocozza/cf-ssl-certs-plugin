# Codefresh SSL Plugin (cf-ssl-certs-plugin)
Codefresh plugin for creation of self signed SSL certificates

## Usage

Set required and optional environment variable and add the following step to your Codefresh pipeline:

```yaml
---
version: '1.0'

steps:

  ...
 
  GenerateSSLCertificate:
    title: Generates SSL Self-signed certificate
    image: paulczar/omgwtfssl
    working_directory: ${{CF_VOLUME_PATH}}
    commands:
      - echo "SSL_SUBJECT = " $SSL_SUBJECT
      - mkdir -p ${{CF_VOLUME_PATH}}/certs
      - cd ${{CF_VOLUME_PATH}}/certs
      - /usr/local/bin/generate-certs
    environment:
      - SSL_SUBJECT=${{SSL_SUBJECT}}    

  ...

```

## Environment Variables

- **required** `KUBE_CONTEXT` - Kubernetes context to use
- `FILE` - Docker Compose file to deploy (default `docker-compose.yaml` file)
- `NAMESPACE` - target Kubernetes namespace (default `default` namespace)
- `REPLICAS` - specify the number of replicas generated (default `1`)
- `VOLUMES` - volumes to be generated (`persistentVolumeClaim`|`emptyDir`) (default `persistentVolumeClaim`)
- `DRY_RUN` - do a "dry run" (print out) deployment (do not install anything, useful for Debug)
- `DEBUG` - print verbose install output

* **required** `SSL_SUBJECT` SSL Subject
* `CA_KEY` CA Key file, default `ca-key.pem` __[1]__
* `CA_CERT` CA Certificate file, default `ca.pem` __[1]__
* `CA_SUBJECT` CA Subject, default `test-ca`
* `CA_EXPIRE` CA Expiry, default `60` days
* `SSL_CONFIG` SSL Config, default `openssl.cnf` __[1]__
* `SSL_KEY` SSL Key file, default `key.pem`
* `SSL_CSR` SSL Cert Request file, default `key.csr`
* `SSL_CERT` SSL Cert file, default `cert.pem`
* `SSL_SIZE` SSL Cert size, default `2048` bits
* `SSL_EXPIRE` SSL Cert expiry, default `60` days
* `SSL_DNS` comma seperate list of alternative hostnames, no default [2]
* `SSL_IP` comma seperate list of alternative IPs, no default [2]

__[1] If file already exists will re-use.__
__[2] If `SSL_DNS` or `SSL_IP` is set will add `SSL_SUBJECT` to alternative hostname list__

## (example) How to use the generated certificate in another step of the build
```yaml
---
version: '1.0'

steps:

  ...

  UseSSLCertificate:
    title: Uses the certificate created in previous step
    image: alpine:latest
    working_directory: ${{CF_VOLUME_PATH}}/certs
    commands:
      - ls
      - echo "This demonstrates how to use the generated certificate in another step:"
      - echo "cert.pem file:"
      - cat cert.pem
      - echo "key.pem file:"
      - cat key.pem
      - echo "key.csr file:"
      - cat key.csr

  ...
 
```
 
