# istio-example
Istio example with my basic microservice


For using follow next steps:


Use Mac book air (Catalina OS) with minikube minikube version: v0.35.0 and kubernetes 0.13.2

1) minikube start
2) Install Istio from /<istio folder>/bien/istiocl manifest apply --set profile=demo 
3) eval $(minikube docker-env)  
4) docker build -t webservice .
5) docker build -t database -f DockerfileMysql .
  //Deploy istio and database in kubernetes
6) kubectl apply -f database-deployment.yaml,database-webservice.yaml,istio-gateway.yaml,istio-virtualservice.yaml
  
  //Inject istio sidecar in our service
7) kubectl apply -f <(istioctl kube-inject -f kubernetes-openshift/webservice-service.yaml)
8) kubectl apply -f <(istioctl kube-inject -f kubernetes-openshift/webservice-deployment.yaml)

After a few minutes, when everything is runnning and working you can access to your service via Istio ingress:

http://<minikube ip>:32680/docker-php/index.php
  
If you want to see Grafana working, you can launch a port forwaring to localhost and play:

kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=grafana -o jsonpath='{.items[0].metadata.name}') 3000:3000 &

accesing via localhost:3000


ENJOY IT!
