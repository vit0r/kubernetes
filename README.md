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
helm repo add grafana https://grafana.github.io/helm-charts
```

## cert-manager

```console
helm upgrade -i itnk-cert-manager jetstack/cert-manager -n cert-manager --create-namespace --set installCRDs=true
```

## metallb

```console
helm upgrade -i itnk-metallb metallb/metallb --set installCRDs=true -n metallb-system --create-namespace --wait
# https://kind.sigs.k8s.io/docs/user/loadbalancer/
cat <<EOF |
apiVersion: metallb.io/v1beta1
kind: IPAddressPool
metadata:
  name: default
  namespace: metallb-system
spec:
  addresses:
  - 192.168.100.200-192.168.100.200
  autoAssign: true
---
apiVersion: metallb.io/v1beta1
kind: L2Advertisement
metadata:
  name: default
  namespace: metallb-system
spec:
  ipAddressPools:
  - default
<<EOF
```

## nginx-ingress

```console
helm upgrade -i itnk-ingress-nginx -n ingress-nginx ingress-nginx/ingress-nginx --set controller.config.ssl-redirect="false" --set controller.service.externalTrafficPolicy="Local" --set controller.service.loadBalancerIP="192.168.100.200" --set controller.service.nodePorts.http="30010" --set controller.service.nodePorts.https="30011" --create-namespace
```

## nfs-server on docker

```console
# https://sysadmins.co.za/setup-a-nfs-server-with-docker/
mkdir -p $HOME/kinddata
docker run --name itnk-nfs --privileged -h itnk-nfs --network kind -v $HOME/kinddata:/data -p 2049:2049 -e SHARED_DIRECTORY=/data itsthenetwork/nfs-server-alpine:12
```

## nfs-provisioner

```console
helm upgrade -i itnk-nfs-provisioner nfs-subdir-external-provisioner/ --create-namespace -n storage nfs-subdir-external-provisioner --set nfs.server=x.x.x.x --set nfs.path=/data
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

## Prometheus stack

```console
helm install itnk-prometheus-stack prometheus-community/kube-prometheus-stack --version 48.3.1 --create-namespace -n monitoring
```

## Grafana Loki

```console
helm install itnk-loki grafana/loki --version 5.10.0 --create-namespace -n monitoring
```
