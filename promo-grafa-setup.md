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

Install Grafana Helm Chart on Kubernetes Cluster
For installing the Grafana on Kubernetes, Use “helm install” command
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
15661
6417
11074
