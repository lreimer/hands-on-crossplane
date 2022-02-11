# Hands-on Crossplane

Demo repository for Crossplane and GitOps style cloud infrastructure management.

## Local Installation

For local installation simply follow the instructions found on the official [Crossplane documentation](https://crossplane.io/docs/v1.6/getting-started/install-configure.html).

```bash
# install latest Crossplan release using Helm in a dedicated namespace
kubectl create namespace crossplane-system

helm repo add crossplane-stable https://charts.crossplane.io/stable
helm repo update

helm install crossplane --namespace crossplane-system crossplane-stable/crossplane --set provider.packages={crossplane/provider-aws:v0.23.0}

## check everything came up OK
helm list -n crossplane-system
kubectl get all -n crossplane-system
```

Next we need to configure the actual cloud provider, e.g. AWS and GCP. For the AWS configuration the credentials are basically the `aws_access_key_id` and `aws_secret_access_key` from the default profile found in the `${HOME}/.aws/credentials` file. With this information we can create a secret and reference it from a provider config resource.

```bash
kubectl create secret generic aws-credentials -n crossplane-system --from-file=credentials=${HOME}/.aws/credentials

# this assumes that the AWS provider has been installed as part of the Helm installation
# otherwise use YAML or kubectl

# kubectl crossplane install provider crossplane/provider-aws:v0.23.0
# kubectl apply -n crossplane-system -f aws/provider.yaml

kubectl apply -n crossplane-system -f aws/providerconfig.yaml

kubectl get events
kubectl get crds
```


## Maintainer

M.-Leander Reimer (@lreimer), <mario-leander.reimer@qaware.de>

## License

This software is provided under the MIT open source license, read the `LICENSE`
file for details.
