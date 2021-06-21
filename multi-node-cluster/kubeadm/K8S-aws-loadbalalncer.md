# Step-1: Create an IAM role with below permissions

![image](https://user-images.githubusercontent.com/24622526/112564463-990c3380-8ddb-11eb-8a1d-6a8ce516067a.png)

# Step-2: Create EC2 instances with IAM role created in step-1

##### In this example, I am choosing t2.medium for master, t2.micro for nodes.

![image](https://user-images.githubusercontent.com/24622526/112569344-4c792600-8de4-11eb-8981-5b0d01e6aaea.png)

![image](https://user-images.githubusercontent.com/24622526/112569271-2eabc100-8de4-11eb-8ef5-c5c0c19307f4.png)


# Step-3: Connect to all EC2 instances and update hostname

![image](https://user-images.githubusercontent.com/24622526/112565650-b8a45b80-8ddd-11eb-8028-90fff1f820eb.png)


```
hostnamectl set-hostname <hostname>
```

##### here hostname = Private IPv4 DNS ex: ip-172-31-59-217.ec2.internal

##### run the below curl command to get "Private IPv4 DNS"

```
curl http://169.254.169.254/latest/meta-data/local-hostname
```

![image](https://user-images.githubusercontent.com/24622526/112565760-f86b4300-8ddd-11eb-9070-cb950e43a8f5.png)

##### Or you can copy the “Private IPv4 DNS” from EC2 console also.

# Step-4: Create a loadbalancer (classic) for master (control-plane)

![image](https://user-images.githubusercontent.com/24622526/112566023-821b1080-8dde-11eb-9cf4-2d3308634b04.png)

![image](https://user-images.githubusercontent.com/24622526/112566046-8fd09600-8dde-11eb-9470-6569479bfd67.png)


##### As of now, Status is OutOfService. Once k8s cluster setup ready, status will be changed.

![image](https://user-images.githubusercontent.com/24622526/112566077-9a8b2b00-8dde-11eb-87ab-736a66c06335.png)


![image](https://user-images.githubusercontent.com/24622526/112566113-aaa30a80-8dde-11eb-9c88-051351985884.png)


# Step-5: add a new tag to EC2, SG, Subnect, Role, VPC, ELB

```
name: kubernetes.io/cluster/<CLUSTERNAME>  ex: kubernetes.io/cluster/v2k8sdevops3
value: owned
```

![image](https://user-images.githubusercontent.com/24622526/112566269-ee960f80-8dde-11eb-9c62-30ce6a89ff31.png)

![image](https://user-images.githubusercontent.com/24622526/112566282-f35ac380-8dde-11eb-8c82-22c7561a2992.png)

![image](https://user-images.githubusercontent.com/24622526/112566305-f9e93b00-8dde-11eb-9eab-c0b32153d9cf.png)

![image](https://user-images.githubusercontent.com/24622526/112566314-feadef00-8dde-11eb-89fc-f87d9dd5a22a.png)


![image](https://user-images.githubusercontent.com/24622526/112566326-040b3980-8ddf-11eb-9206-b2e2fb318f3b.png)


# Step-6: Install the required tools: docker, kubelet, kubeadm, kubectl

##### Copy the commands from the section “Installation” from the GitHub file https://github.com/DevOpsPlatform/Phase-2/blob/master/kubernetes/multi-node-cluster/kubeadm/1.Kubernetes-installation-examples.md

##### Create sh file and run it. (do the same this on all the EC2 instances which will be part of the k8s cluster

```
vi k8s.sh
```

##### paste the copied commands from the section “Installation” from the GitHub file https://github.com/DevOpsPlatform/Phase-2/blob/master/kubernetes/multi-node-cluster/kubeadm/1.Kubernetes-installation-examples.md

![image](https://user-images.githubusercontent.com/24622526/112566368-1eddae00-8ddf-11eb-9032-933ff5467cb0.png)

##### Check all the tools are working or not

```
docker version

kubeadm version

kubectl version
```

# Step-8: change the cgroup driver to system after docker installed (perform this on all EC2 instances)

```
cat > /etc/docker/daemon.json <<EOF
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2"
}
EOF


sudo systemctl restart docker

sudo systemctl enable docker

```

# Step-9: Update the configuration of the kubelet service so that it knows about the AWS environment as well. Edit the file `/etc/systemd/system/kubelet.service.d/10-kubeadm.conf`

```
vi /etc/systemd/system/kubelet.service.d/10-kubeadm.conf
```

##### add  `--cloud-provider=aws`  as new line 

```
systemctl daemon-reload
```

![image](https://user-images.githubusercontent.com/24622526/112566775-e68a9f80-8ddf-11eb-958d-d3de1063e926.png)

# Step-10: Prepare kubeadm.yml configuration file. (you can copy the below file and update the cluster name and ELB name with your resource details.

##### Updae the `clusterName: "v2k8sdevops3"` in the below file with your details given in Step-5.

```
apiVersion: kubeadm.k8s.io/v1beta1
kind: InitConfiguration
nodeRegistration:
  kubeletExtraArgs:
    cloud-provider: "aws"
---
apiVersion: kubeadm.k8s.io/v1beta1
kind: ClusterConfiguration
kubernetesVersion: v1.20.5
apiServer:
  extraArgs:
    cloud-provider: "aws"
clusterName: "v2k8sdevops3"
controllerManager:
  extraArgs:
    cloud-provider: "aws"
    allocate-node-cidrs: "true"
    cluster-cidr: 10.244.0.0/16

```


# Step-10: Cluster setup

```
kubeadm config images pull
kubeadm init --config kubeadm.yml
```

##### if anything goes wrong, try: `kubeadm reset -f` and repeat this step-10.

![image](https://user-images.githubusercontent.com/24622526/112567182-ae379100-8de0-11eb-8819-429fb2edb450.png)


```
To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

Alternatively, if you are the root user, you can run:

  export KUBECONFIG=/etc/kubernetes/admin.conf

```

![image](https://user-images.githubusercontent.com/24622526/112567295-e0e18980-8de0-11eb-96bd-6e26c218bbcd.png)

##### Install k8s network

```

kubectl get nodes

kubectl get pods --all-namespaces

sysctl net.bridge.bridge-nf-call-iptables=1

export kubever=$(kubectl version | base64 | tr -d '\n')

kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$kubever"

kubectl get nodes

kubectl get pods --all-namespaces

```

![image](https://user-images.githubusercontent.com/24622526/112567504-39188b80-8de1-11eb-854c-bf04e4bd32be.png)


```
kubectl get pods --all-namespaces
```


##### Join the nodes into the cluster using kubeadm join command. Use this command to create and print the join command : kubeadm token create --print-join-command

![image](https://user-images.githubusercontent.com/24622526/112567608-682efd00-8de1-11eb-95fb-7b8792fc2512.png)


##### so far, you ran all the commands on k8s-master to setup cluster, now run the join command on k8s-worker to join the other servers as worker in the k8s cluster.

![image](https://user-images.githubusercontent.com/24622526/112567696-97de0500-8de1-11eb-9b23-3b3cf747d0d0.png)

##### on master:

```
kubectl get nodes or kubectl get nodes -o wide
```

![image](https://user-images.githubusercontent.com/24622526/112567811-be9c3b80-8de1-11eb-8b17-dd1425c6e770.png)



# Step-11: Final step, Create the deployment and service

```
kubectl create deployment nginx --image=nginx

kubectl get deploy

kubectl get all or kubectl get all -A
```


```
vi svc.yml
```

```
apiVersion: v1
kind: Service
metadata:
  name: nginx
spec:
  selector:
    app: nginx
  ports:
    - port: 80
      targetPort: 80
  type: LoadBalancer

```

```
kubectl apply -f svc.yml
```

```
kubectl get svc
```


![image](https://user-images.githubusercontent.com/24622526/112568397-b1338100-8de2-11eb-8ef6-3187a357c902.png)


##### Add the instance (master) to the newly created loadbalancer and wait for the status changed to "InService"

![image](https://user-images.githubusercontent.com/24622526/112568336-982ad000-8de2-11eb-80e1-dc9740058126.png)


##### Once the status is updated to "InService", copy the load balancer DNS and hit in any brwoser.

![image](https://user-images.githubusercontent.com/24622526/112568738-3c147b80-8de3-11eb-80e1-dc0001d7fc54.png)


![image](https://user-images.githubusercontent.com/24622526/112568692-28691500-8de3-11eb-95a6-4366d2168dbc.png)

##### Remove the service and deployment loadbalancer also will be removed automatically from AWS.

![image](https://user-images.githubusercontent.com/24622526/112569086-d4126500-8de3-11eb-9bc0-208aaa417ede.png)


