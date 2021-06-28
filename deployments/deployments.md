
## Example-1

#### create deployment using command

    kubectl create deployment nginx-deployment --image=nginx --replicas=2
    
    kubectl get all
    
    kubectl delete all --all
    
#### create deployment using yml file

> Refer: https://kubernetes.io/docs/concepts/workloads/controllers/deployment/

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
    
#### create service using yml

    kubectl apply -f https://raw.githubusercontent.com/DevOpsOnlineTraining-2021/K8S/main/yml/3.1.nginx-service.yml


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
     Vertical Pod Autoscaler ( VPA )  : Vertical Pod Autoscaler automatically adjusts the CPU and Memory attributes for your Pods.
     
     
#### create deployment using yml

     kubectl apply -f https://raw.githubusercontent.com/DevOpsOnlineTraining-2021/K8S/main/yml/4.devopswebapp-deploy.yml
     
     kubectl get all
     
     kubectl get pods -l app=devopswebapp
     
#### update deployment using yml file

    kubectl apply -f https://raw.githubusercontent.com/DevOpsOnlineTraining-2021/K8S/main/yml/5.devopswebapp-deploy-update.yml
    
    kubectl get pods -l app=devopswebapp
    
#### scaleup deployment using yml file

    kubectl apply -f https://raw.githubusercontent.com/DevOpsOnlineTraining-2021/K8S/main/yml/6.devopswebapp-deploy-scale.yml
    
    kubectl get pods -l app=devopswebapp

#### Autoscale using yml file

    kubectl apply -f https://raw.githubusercontent.com/DevOpsOnlineTraining-2021/K8S/main/yml/7.devopswebapp-deploy-hpa.yml

    kubectl get pods -l app=devopswebapp
    
    kubectl get all
     
#### create service using yml

    kubectl apply -f https://raw.githubusercontent.com/DevOpsOnlineTraining-2021/K8S/main/yml/8.devopswebapp-service.yml

## Example-3

#### Create deployment, service, hpa in a single yml file

    kubectl apply -f https://raw.githubusercontent.com/DevOpsOnlineTraining-2021/K8S/main/yml/9.jenkins-deploy.yml

## Example-4: update the deployments using commands

    kubectl create deployment nginx-deploy --image=nginx --replicas=2
    
    kubectl describe deployment nginx-deploy
    
    kubectl edit deployment nginx-deploy
    
    update nginx image tags as 1.14.2 and save
    
    kubectl describe deployment nginx-deploy
    
    kubectl edit deployment nginx-deploy
    
    update nginx image tags as 1.16.1 and save
    
    kubectl describe deployment nginx-deploy

## Example-5: Volumes

#### Jenkins volumes hostpath

    chown -R 1000:1000 /var/lib/jenkins

    kubectl apply -f https://raw.githubusercontent.com/DevOpsOnlineTraining-2021/K8S/main/yml/10.jenkins-vol-deploy.yml
    
    ll /var/lib/jenkins
    
    kubectl delete deployment jenkins-vol-deploy
    
    ll /var/lib/jenkins
    
#### Jenkins volumes emptyDir

    kubectl apply -f https://raw.githubusercontent.com/DevOpsOnlineTraining-2021/K8S/main/yml/11.jenkins-empty-vol-deploy.yml
    
    kubectl get all
    
    ll /var/lib/kubelet/pods/8e44e612-0248-486c-9bb1-2a5c5734a4d8/volumes/kubernetes.io~empty-dir/jenkins-vol-empty
    
    kubectl delete deployment jenkins-vol-deploy
    
    find / -name "jenkins-vol-empty"
    
#### Persistance Volume and Persistance Volume Claim

    mkdir /mnt/nginx_vol
    
    echo "<center><h1>I am from $hostname</h1></center>" > /mnt/nginx_vol/index.html

    kubectl apply https://raw.githubusercontent.com/DevOpsOnlineTraining-2021/K8S/main/yml/12.nginx-pv.yml
    
    

    
    
     
     
    

