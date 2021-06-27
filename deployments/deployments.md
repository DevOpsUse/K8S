
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

#### create deployment using command

    kubectl create deployment devopsweb --image=venkatasykam/devopswebapp:1.0.13 --replicas=2
    
    kubectl get all
    
    kubectl describe deployment devopsweb (observe the image version)
    
    kubectl create service nodeport devopsweb --tcp=8080:8080
    
    kubectl get svc devopsweb
    
    curl http://[publicIP]:[nodePort]//appname
    
    ex: http://3.83.178.35:30628/DevOpsWebApp-1.0.13/
    
#### Update the deployment using command

     kubectl set image deployment/devopsweb devopswebapp=venkatasykam/devopswebapp:1.0.14
     
     kubectl describe deployment devopsweb (observe the image version whether its updated or not)
     
#### Scaling a Deployment using command

     kubectl scale deployment devopsweb --replicas=5
     
     or
     
     kubectl autoscale deployment devopsweb --min=5 --max=10 --cpu-percent=60
     
     kubectl get hpa (11th question in - https://github.com/DevOpsOnlineTraining-2021/K8S/blob/main/Interview-questions.md)

     Horizontal Pod Autoscaler ( HPA ): Horizontal Pod Autoscaler scales the number of Pods in a Deployment.
     Vertical Pod Autoscaler ( VPA )  : Unlike Horizontal Pod Autoscaler ( HPA ), Vertical Pod Autoscaler ( VPA ) automatically adjusts the CPU and Memory attributes for your Pods.
     
     
     
     
     
     
     
    

