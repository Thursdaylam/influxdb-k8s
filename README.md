# influxdb-k8s
Simple example on how to set up influxDB on Kubernetes and a simple python server that read/writes to it.


## Getting Started

```
#start a cluster, a simple one would be minikube
minikube start

#use minikube docker env
eval $(minikube docker-env)

#build image
docker build -t test-app/test-app:0.1.3 . 

#deploy server that periodically read/writes an influx db
kubectl create -f ./deployment.yaml

#deploy influxdb via Helm
helm install --name influxdb \
  --set persistence.enabled=true,persistence.size=2Gi \
  --set resources.limits.memory=2Gi,resources.limits.memory=2Gi \
    stable/influxdb

#Check if influxDB is receiving updates
kubectl port-forward PODNAME 8086:8086 

#should be visible at http://influxdb.default:8086

#try a example query via http (select * from * pod_shutdown_data) http://localhost:8086/query?db=pods&q=select+%2A+from+pod_shutdown_data%3B
```

## Notes

```
#If queries are not showing:

#make sure pods are running and get their name
kubectl get pod --all-namespaces

#describe pods if they are not starting
kubectl describe POD-NAME

#check logs of influxdb and the test-app pods if started but not receiving updates
kubectl logs POD-NAME


```