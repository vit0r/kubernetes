# kubernetes

## kind Kubernetes in docker

```console
# https://kind.sigs.k8s.io/docs/user/quick-start/#installation
kind create cluster --config kind-cluster.yaml --name itnk
```

## install helm

[helm install](https://helm.sh/docs/intro/install/)

## add helm repos

```console
helm repo add metrics-server https://kubernetes-sigs.github.io/metrics-server/
helm repo add hashicorp https://helm.releases.hashicorp.com
```

## hashicorp

### vault

```console
helm upgrade -i itnk-vault hashicorp/vault --version 0.25.0 --create-namespace -n vault
helm upgrade -i itnk-vault-secrets-operator hashicorp/vault-secrets-operator --version 0.1.0 --create-namespace -n vault
```

### consul

```console
helm upgrade -i itnk-consul hashicorp/consul --version 1.2.0 --create-namespace -n consul
```
