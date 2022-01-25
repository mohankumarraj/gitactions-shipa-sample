# Shipa GitHub Actions Example

This is a light-weight example in managing Shipa Objects with GitHub Actions. 
You will need to fork one of the examples into a private repo and wire in the
required cluster defination examples. 

## Running This Example

The best way is to Fork into your own repository. You will need to wire in your
Shipa Token and Shipa Host as Repository Secrets. The Cluster Connection / Defination
Items are designed to be files for operaitons teams. These items are in ./infra

### Shipa Token
Can install the Shipa CLI to get the token. 
https://learn.shipa.io/docs/downloading-the-shipa-client

```
shipa token show
```

Wire a Repository Secret as "SHIPA_TOKEN".

### Shipa Host
The Shipa Host is Your Shipa Target. Can view in the UI under Settings -> Target Address
or in the CLI.

```
shipa target list
```

Wire a Repository Secret as "SHIPA_HOST".

### Infra and cluster-def.yaml Items
You will need to have a Kubernetes cluster as a destination and will need the Kubernetes Endpoint
API Address, Shipa Role Token, and CA Cert.

#### API Address
```
kubectl cluster-info | grep 'Kubernetes' | awk '/http/ {print $NF}'  
```

#### Shipa Role Binding and Token
Can Grab the Role Binding Here
https://learn.shipa.io/docs/connecting-clusters
```
kubectl apply -f shipa-admin-service-account.yaml
#Token
kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep shipa-admin | awk '{print $1}')
```

#### CA Cert
```
kubectl get secret $(kubectl get secret | grep default-token | awk '{print $1}') -o jsonpath='{.data.ca\.crt}' | base64 --decode
```