# Install cert-manager using Let's Encrypt as Root CA and Cloudflare as DNS registrar

## Install cert-meneger
```helm repo add jetstack https://charts.jetstack.io --force-update```
```
helm install \
  cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --create-namespace \
  --version v1.16.2 \
  --set crds.enabled=true
```

## Create the API secret of Cloudflare in the secret.yaml file
```
---
apiVersion: v1
kind: Secret
metadata:
  name: cloudflare-api-key-secret
  namespace: cert-manager
type: Opaque
stringData:
  api-key: <your-global-api-comes-here>
```
## Apply the secret
kubectl apply -f secret.yaml -n cert-manager

## Apply the clusterissuer to your namespace
```
---
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: cloudflare-clusterissuer
spec:
  acme:

    server: https://acme-v02.api.letsencrypt.org/directory
    privateKeySecretRef:
      name: cloudflare-clusterissuer-account-key
    solvers:
      - dns01:
          cloudflare:
            email: <your-email-address-comes-here>
            apiKeySecretRef:
              name: cloudflare-api-key-secret
              key: api-key
```

``` kubectl apply -f clusterissuer.yaml ```

## To test the certificate issuing progress, use the below commands

## To change the namespace to cert-manager
``` kubectl config set-context --current --namespace=cert-manager ```
## To see the status of the issued certificate
``` kubectl describe certificate ```

## To create a test certificate, let's say for ArgoCD

## Create the namespace for ArgoCD
``` kubectl create ns argocd ```

## Apply the wildcard certificate for this namespace
``` kubectl apply -f ./wc_certificate.yaml -n argocd ```


kubectl apply -f ./argocd_cert.yaml -n argocd
