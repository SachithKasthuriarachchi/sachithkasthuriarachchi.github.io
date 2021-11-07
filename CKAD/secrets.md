# Secrets

- `kubectl create secret generic <secret-name> --from-literal=<key>=<value>`
- `kubectl create secret generic <secret-name> --from-file=<path to kv secrets>`

```
apiVersion: v1
kind: Secret
metedata:
  name: app-secret
data:
  DB_HOST: hshjcj
```

When creating secrets we need to provide the values in an encoded format (base64).

- `echo -n 'value' | base64`: encoding
- `echo -n 'value' | base64 --decode`: decoding

If we mount the secret as a volume in the pod, each property will create a file whose content is the secret value.

---
[Kubernetes Security](security.md)

[Back](index.md)
