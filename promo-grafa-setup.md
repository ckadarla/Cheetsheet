Run the following commands to add the Prometheus helm chart
```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```
Install Prometheus Helm Chart on Kubernetes Cluster
To install the Prometheus helm chart we need to run the “helm install” command as below shown
```
helm install prometheus prometheus-community/prometheus
```
```
minikube service monitoring-prometheus-server &
```

To get the Grafana Helm chart, run this command
```
helm repo add grafana https://grafana.github.io/helm-charts 
helm repo update
```

Install the Grafana Helm Chart on a Kubernetes Cluster
For installing Grafana on Kubernetes, use “helm install” command
```
helm install grafana grafana/grafana
```
```
minikube service grafana &
```
To get the admin user password 
```
 kubectl get secret --namespace default grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```
To get the dashbard library 
```
https://grafana.com/grafana/dashboards/
```
Popular Dashboards 
---
#### 15661 -  K8S Dashboard EN 20250125

#### 6417 - Kubernetes Cluster (Prometheus)

#### 11074 - Node Exporter Dashboard EN 20201010-Sta

#### 315 - Kubernetes cluster monitoring (via Prometheus)

#### 3831 - Kubernetes Cluster Autoscaler (via Prometheus)

#### 14900 - Nginx 

#### 21548

```
kubectl exec -n grafana grafana-7968bbd4b7-tdklb -- bin/grafana-cli admin reset-admin-password MyAdminPassword
```
