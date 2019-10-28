# 7. cert-manager

## Install cert-manager

* Install cert manager with helm
```
kubectl apply --validate=false -f https://raw.githubusercontent.com/jetstack/cert-manager/release-0.11/deploy/manifests/00-crds.yaml
helm repo add jetstack https://charts.jetstack.io
helm repo update
helm upgrade --install --namespace cert-manager --version v0.11.0 cert-manager jetstack/cert-manager
```

* Continue with your DNS provider below
 
### With AWS Route 53

* Add AWS Secret Key to `route53/credentials-secret.yaml`
* Add AWS Access Key to `route53/clusterissuer.yaml`
* Create ClusterIssuer
```
kubectl apply -f route53/credentials-secret.yaml
kubectl apply -f route53/clusterissuer.yaml
```
* Change hostnames in `certificate.yaml`
* Create wildcard certificate
```
kubectl apply -f route53/certificate.yaml
```

### With designate

* Clone the designate certmanger repository and follow the install instructions
```
git clone git@github.com:syseleven/designate-certmanager-webhook.git
```

* Create ClusterIssuer
```
kubectl apply -f designate/clusterissuer.yaml
```

* Change hostnames in `certificate.yaml`
* Create wildcard certificate
```
kubectl apply -f designate/certificate.yaml
```

### With DigitalOcean

* Change hostnames in `certificate.yaml`
* Encode your API token in base64 and swap it in `credentials-secret.yaml`
```
base64 <<< 'TOKEN'
```

* Apply the configuration
```
kubectl apply -f digitalocean/
```