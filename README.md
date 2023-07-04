# K8s cluster

This repository defines helm charts to initialise a cluster with the following components:
* Certificate manager and automatic certificate issuer using Let's Encrypt
* Nginx ingress controller
* Prometheus
* Grafana
* Registry for docker images
* Application namespace

## Certificate manager and automatic certificate issuer using Let's Encrypt

```shell
helm repo add jetstack https://charts.jetstack.io
helm repo update

helm upgrade --install -f cert-manager/values.yaml cert-manager jetstack/cert-manager --create-namespace --namespace cert-manager --wait
helm upgrade --install cert-issuer cert-issuer --create-namespace --namespace cert-manager --wait
```

## Nginx ingress controller

```shell
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
helm upgrade --install -f ingress-nginx/values.yaml ingress-nginx ingress-nginx/ingress-nginx --create-namespace --namespace ingress-nginx --wait
```

## Prometheus

```shell
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update

helm upgrade --install -f prometheus/values.yaml prometheus prometheus-community/prometheus --create-namespace --namespace monitoring --wait
```

## Grafana

```shell
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update

helm upgrade --install -f grafana/values.yaml grafana grafana/grafana --create-namespace --namespace monitoring --wait
helm upgrade --install -f grafana-ingress/values.yaml grafana-ingress grafana-ingress --create-namespace --namespace monitoring --wait
```

## Registry for docker images

```shell
helm repo add twuni https://helm.twun.io
helm repo update

helm upgrade --install -f registry/values.yaml registry twuni/docker-registry --create-namespace --namespace registry --wait
helm upgrade --install -f registry-ingress/values.yaml registry-ingress registry-ingress --create-namespace --namespace registry --wait
```

## Application namespace

```shell
helm upgrade --install -f apps/values.yaml apps apps --create-namespace --namespace apps --wait
```

## PostgreSQL

```shell
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update

helm upgrade --install -f postgresql/values.yaml postgresql bitnami/postgresql --create-namespace --namespace apps --wait
```
