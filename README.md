# infra-toda-no-kubernetes

## kind Kubernetes in docker

```console
# https://kind.sigs.k8s.io/docs/user/quick-start/#installation
kind create cluster --config kind-cluster.yaml --name itnk
```

## install helm

[helm install](https://helm.sh/docs/intro/install/)

## add helm repos

```console
helm repo add hashicorp https://helm.releases.hashicorp.com
```

## hashicorp

### vault

```console
helm install itnk-vault hashicorp/vault --version 0.25.0
helm install itnk-vault-secrets-operator hashicorp/vault-secrets-operator --version 0.1.0
```

### consul

```console
helm install my-consul hashicorp/consul --version 1.2.0
```
