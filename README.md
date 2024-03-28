# helm-charts

```bash
helm repo add breuerfelix https://breuerfelix.github.io/helm-charts
helm repo update
```

## paperless-ngx

You need to create a secret manually (create namespace first):
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: paperless-ngx
  namespace: paperless-ngx
type: Opaque
stringData:
  PAPERLESS_SECRET_KEY: "some-secret-key"
  PAPERLESS_URL: "https://paperless.example.com"
  PAPERLESS_ALLOWED_HOSTS: "https://paperless.example.com"
  PAPERLESS_ADMIN_USER: "admin"
  PAPERLESS_ADMIN_PASSWORD: "some-secret-password"
```

Use the following `values.yaml`:

```yaml
paperless:
  image:
    name: ghcr.io/paperless-ngx/paperless-ngx
    tag: 2.6.3 # search for the newest version online

  ingress:
    enabled: true
    host: paperless.example.com

  env:
    - name: PAPERLESS_REDIS
      value: redis://redis:80
    - name: PAPERLESS_TIME_ZONE
      value: Europe/Berlin # your timezone
    - name: PAPERLESS_OCR_LANGUAGE
      value: deu # your language

  secretRefs:
    - key: PAPERLESS_SECRET_KEY
      name: paperless-ngx
    - key: PAPERLESS_URL
      name: paperless-ngx
    - key: PAPERLESS_ALLOWED_HOSTS
      name: paperless-ngx
    - key: PAPERLESS_ADMIN_USER
      name: paperless-ngx
    - key: PAPERLESS_ADMIN_PASSWORD
      name: paperless-ngx
```

Install:
```bash
kubectl create namespace paperless-ngx
helm install -f values.yaml -n paperless-ngx paperless-ngx breuerfelix/paperless-ngx
```
