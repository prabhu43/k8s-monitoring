## Install prometheus, grafana and prometheus operator using kube-prometheus

git clone git@github.com:coreos/kube-prometheus.git
cd kube-prometheus

#### Create the namespace and CRDs, and then wait for them to be available before creating the remaining resources
kubectl create -f manifests/setup
until kubectl get servicemonitors --all-namespaces ; do date; sleep 1; echo ""; done
kubectl create -f manifests/

#### To allow prometheus to access pods, services, endpoints in any namespace in cluster
- Add this in prometheus clusterrole `manifests/prometheus-clusterRole.yaml`
```
- apiGroups:
  - ""
  resources:
  - services
  - pods
  - endpoints
  verbs:
  - get
  - list
  - watch
```
- kubectl apply -f manifests/prometheus-clusterRole.yaml

> Note: this is configured correctly in `prometheus-operator` helm chart 

## Access Dashboards:
* Prometheus: 
```
kubectl --namespace monitoring port-forward svc/prometheus-k8s 9090
```

* Grafana:
```
kubectl --namespace monitoring port-forward svc/grafana 3000
```

* Alertmanager:
```
kubectl --namespace monitoring port-forward svc/alertmanager-main 9093
```