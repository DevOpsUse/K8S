# Running Kubernetes Locally via Minikube

### Install docker.

Launahc an EC2 ubuntu instance. And connect to it, then install docker.

    sudo -i

    apt-get update -y &&  apt-get install -y docker.io apt-transport-https
    
    curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add
    
    echo "deb http://apt.kubernetes.io/ kubernetes-xenial main" > /etc/apt/sources.list.d/kubernetes.list
    
    apt-get update
    
    apt-get install -y kubectl conntrack
    
    kubectl version
    
    docker version
      

### Install minikube

    curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/

    minikube version
        

### Start minukube - single node cluster setup

    minikube start --vm-driver=none

    minikube status
    
    kubectl version

### Deploy docker images

ex-1:
    
    kubectl create deployment nginx-deployment --image=nginx

    kubectl expose deployment nginx-deployment --port=80 --type=NodePort

    kubectl get svc nginx-deployment
    
    kubectl get all
    
    try remove pod: kubectl delete pod <pod-name> (observation is, immidiately a new pod will be created by replica set)
    
    try delete deployment and then access nginx from browser. (observation is, service will not work after we deleted the deployment)

ex-2:

    kubectl run devops-deployment --image=venkatasykam/devopswebapp:1.0.13 --port=8080
    
    kubectl get all

    kubectl expose pod devops-deployment --port=8181 --target-port=8080 --type=NodePort

    kubectl get svc devops-deployment
    
    kubectl get all
    
    ex: http://34.234.91.119:30764/DevOpsWebApp-1.0.13/
    
    try remove pod: kubectl delete pod <pod-name> (observation is, pod will not be created automatically as this pod not controlled by any replica set or deployment)

ex-3:

    kubectl create deployment nginx --image=nginx
    
    kubectl create service nodeport nginx --tcp=80:80
    
    kubectl get svc nginx
    
        try clusterip
    
    kubectl create service clusterip nginx --tcp=80:80
    
    kubectl get all or kubectl get svc nginx
    
    curl <ClusterIp>
    
    ex: curl 10.97.117.148
    
    <!DOCTYPE html>
    <html>
    <head>
    <title>Welcome to nginx!</title>
    <style>
        body {
            width: 35em;
            margin: 0 auto;
            font-family: Tahoma, Verdana, Arial, sans-serif;
        }
    </style>
    </head>
    <body>
    <h1>Welcome to nginx!</h1>
    <p>If you see this page, the nginx web server is successfully installed and
    working. Further configuration is required.</p>

    <p>For online documentation and support please refer to
    <a href="http://nginx.org/">nginx.org</a>.<br/>
    Commercial support is available at
    <a href="http://nginx.com/">nginx.com</a>.</p>

    <p><em>Thank you for using nginx.</em></p>
    </body>
    </html>

    

ex-4: 

    kubectl create deployment tomcat --image=tomcat
    
    kubectl create service nodeport tomcat --tcp=8080:8080
    
    kubectl get svc tomcat

ex-5: Single command to run & expose: 

    kubectl run nginx-deploy --image=nginx --port=80 --hostport=8888

    curl http://172.17.0.94:8888

    docker ps | grep nginx-deploy

ex-6: Single command to run & expose: 

    kubectl run jenkins-cicd --image=jenkins/jenkins:latest --port=8080 --hostport=8888

    curl http://172.17.0.94:8888

ex-7: Single command to run & expose: 

    kubectl run sonarqube --image=sonarqube --port=9000 --hostport=9999

    curl http://172.17.0.94:9999
    

ex-8: Single command to run & expose: 

    kubectl run jenkins-cicd --image=jenkins --port=8080 --hostport=8888

    kunectl describe pod <pod-name>
    
---------------

    kubectl get all

    kubectl --namespace=kube-system get all

    kubectl get nodes

    kubectl describe node <node-name>

    minikube stop


