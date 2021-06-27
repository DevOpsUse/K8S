
## Example-1

#### create deployment using command

    kubectl create deployment nginx-deployment --image=nginx --replicas=2
    
    kubectl get all
    
    kubectl delete all --all
    
#### create deployment using yml file

    kubectl create -f https://raw.githubusercontent.com/DevOpsOnlineTraining-2021/K8S/main/yml/1.nginx-deployment.yml
    
    or
    
    kubectl apply -f https://raw.githubusercontent.com/DevOpsOnlineTraining-2021/K8S/main/yml/1.nginx-deployment.yml
    
    kubectl get all
    
    kubectl get pods -l app=nginx

#### update deployment using yml file

    kubectl apply -f https://raw.githubusercontent.com/DevOpsOnlineTraining-2021/K8S/main/yml/2.nginx-deployment-update.yml
    
    kubectl get pods -l app=nginx
    
#### scaleup deployment using yml file

    kubectl apply -f https://raw.githubusercontent.com/DevOpsOnlineTraining-2021/K8S/main/yml/3.nginx-deployment-scale.yml
    
    kubectl get pods -l app=nginx


## Example-2

