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


## Kubernetes Configuration

