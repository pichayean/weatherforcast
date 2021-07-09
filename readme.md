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
