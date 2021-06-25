
1. k8s archetecture or k8s components

2. which tool you are using to setup cluster

	self managed tools: kubeadm, kops, kubeaws, julu etc
	cloud: EKS(AWS), AKS(Azure), IKS(IBM) etc
	others: openshift, rancher, helm etc
  
3. diff b/w dockerswam and k8s

	k8s: self healing, autoscaling, loadbalancer, high availability with k8s, k8s is complex but robust, durable, scalable, etc
	dockerswam: dockerswam is suitable for small scale cluster (less number of containers)
	
	by default loadbalancer available in docker swarm but manaul setup is required in k8s.
	by deault autoscaling is not available in docker swarm but available in k8s.

4. how to communicate one pod to another pod

   using service types (NodePort, Loadbalance, clusterIp) pods can communicate.
   
5. how to communicate one service to another service

    	use same service types (NodePort, Loadbalance, clusterIP)
	    use same lable to communicate one service to another
      
6. 	NodePort Vs Loadbalance Vs clusterIp

        clusterIP: to access the deployed service with in the cluster (ex: database services/containers)
        NodePort: to access from outside of the cluster (public internet) (with in the range 30000-32767) - if we have thusands of application/services these ports are not enough, that why use Loadbalance type.
        Loadbalance: to access from internet (as same NodePort)
          application load balalcner: to call any service without port
            loadbanaceURL1 for single service1
            loadbanaceURL2 for single  service2
            loadbanaceURL3 for single service3
 
7. how to troubleshoot issues in k8s cluster at application level

        imagePullBackoff (unable to pull the image, may be image not vailable, not accessable)
        crashloopbackoff (may be apoplication issues, application unable to run or db conection issues, application may not up due to any other reason)

8. how to fix the failed pods (image pull back off, cross loop back off, pending)

        step-1: first describe the pod

          kubectl describe pod coredns-74ff55c5b-hxhb5 -n kube-system

        step-2: 	see the 'reason' under 'Events'

            Events:
          Type     Reason            Age    From               Message
          ----     ------            ----   ----               -------
          Warning  FailedScheduling  2m41s  default-scheduler  0/1 nodes are available: 1 node(s) had taint {node.kubernetes.io/not-ready: }, that the pod didn't tolerate.
          Warning  FailedScheduling  2m41s  default-scheduler  0/1 nodes are available: 1 node(s) had taint {node.kubernetes.io/not-ready: }, that the pod didn't tolerate.

        step-3: in this case, node is not available and not ready yet. Need to setup the network layers, so that master node will be available and ready (in the cluster). i.e, node is ready to communicate with other nodes.

        Other reasons for status 'pending'/'failed' 

        1. resources may be fulled like CPU, RAM, Harddisk etc
        2. n/w issue or connectivity

9. how to up the k8s cluster when its down. (steps to fix, how/where to check logs etc)

        kubectl cluster-info
        https://codefresh.io/kubernetes-tutorial/recover-broken-kubernetes-cluster/#:~:text=Update%20your%20Kubernetes%20API%20load,of%20the%20new%20master%20node.&text=HA%20Recovery%20Steps-,If%20Kubernetes%20was%20not%20running%20in%20HA%20mode%20and%20the,the%20cluster%20will%20be%20down

        n/w issues
        kubelet may stopped
        docker may stopped

        if k8s completely down, connect to master, and check the API server logs (ex: /var/log/kube-apiserver.log, /var/log/kube-scheduler.log, /var/log/kube-controller-manager.log)

        if it is something worng with kubelet, connect to worker and check thge logs - /var/log/kubelet.log

10. 3 types of controllers (refer k8s archeture)

        1. Node controller
        2. Replicas controller
        3. Cloud controller

11. 
