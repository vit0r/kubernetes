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
helm repo add jetstack https://charts.jetstack.io
helm repo add metallb https://metallb.github.io/metallb
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo add metrics-server https://kubernetes-sigs.github.io/metrics-server/
helm repo add nfs-subdir-external-provisioner https://kubernetes-sigs.github.io/nfs-subdir-external-provisioner/
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add hashicorp https://helm.releases.hashicorp.com
```

## cert-manager

```console
helm upgrade -i itnk-cert-manager jetstack/cert-manager -n cert-manager --create-namespace --set installCRDs=true --max-history 2
```

## metallb

```console
helm upgrade -i itnk-metallb metallb/metallb --set installCRDs=true -n metallb-system --create-namespace --max-history 2
```

## nginx-ingress

```console
helm upgrade -i itnk-ingress-nginx -n ingress-nginx ingress-nginx/ingress-nginx --create-namespace --max-history 2
```

## nfs-server

```console
:todo
```

## nfs-provisioner

```console
helm install itnk-nfs-provisioner nfs-subdir-external-provisioner/nfs-subdir-external-provisioner --set nfs.server=x.x.x.x --set nfs.path=/data
```

## hashicorp

### vault

```console
helm upgrade -i itnk-vault hashicorp/vault --version 0.25.0 --max-history 2
helm upgrade -i itnk-vault-secrets-operator hashicorp/vault-secrets-operator --version 0.1.0 --max-history 2
```

### consul

```console
helm upgrade -i itnk-consul hashicorp/consul --version 1.2.0 --max-history 2
```

## Prometheus stack

```console
helm install itnk-prometheus-stack prometheus-community/kube-prometheus-stack --version 48.3.1 --max-history 2
```
