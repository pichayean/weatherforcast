kubectl apply -f pod.yaml --namespace=test

0) docker build -t pichayean/weatherforcast:v1 .
0.1) docker push pichayean/weatherforcast:v1 .

1) kubectl apply -f ./deployment.yaml --namespace=americano
#--namespace=test
    kubectl get deployments
    kubectl get pods

2) kubectl apply -f ./service.yaml 
#--namespace=test




kubectl delete deployment weatherforcast-deployment
kubectl delete service weatherforcast-service





////888888888888888888888888888888888888888888888
install mimikube
https://www.atlantic.net/vps-hosting/how-to-install-kubernetes-with-minikube-on-centos-8/

kubectl proxy --address='0.0.0.0' --disable-filter=true

Jenkins
http://194.233.73.42:31870/job/weather/

https://ai1love6.medium.com/deploy-jenkins-on-k8s-c2df6c109915




