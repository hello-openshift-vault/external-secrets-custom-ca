# External Secrets Operator with Custom CA

Use External Secrets Operator against a secret store using a TLS certificate
signed by a custom certificate authority (CA). This does require that the
custom CA is trusted by OpenShift (see the OpenShift [Custom PKI Docs]).

This was specifically tested with Vault but should work for any secret provider
signed by a custom CA that OpenShift is configured to trust.

## Installing

1. Create the external-secrets project:
```bash
oc new-project external-secrets
```

2. Create the ca-bundle config map:
```bash
oc create -f - << EOF
kind: ConfigMap
apiVersion: v1
metadata:
  name: ca-bundle
  namespace: external-secrets
  labels:
    config.openshift.io/inject-trusted-cabundle: 'true'
EOF
```

3. Deploy External Secrets Operator from Helm
```bash
helm repo add external-secrets https://charts.external-secrets.io
helm install \
    external-secrets \
    external-secrets/external-secrets \
    -f values.yaml \
    -n external-secrets \
    --create-namespace \
    --set installCRDs=true
```

[Custom PKI Docs]: https://docs.openshift.com/container-platform/latest/networking/configuring-a-custom-pki.html
