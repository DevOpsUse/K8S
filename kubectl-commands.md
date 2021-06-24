### kubectl-commands: https://kubernetes.io/docs/reference/kubectl/overview/


https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands

https://kubernetes.io/docs/reference/kubectl/cheatsheet/

    kubectl [command] [TYPE] [NAME] [flags]
    
    kubectl help

* kubectl cluster-info

* kubectl config view

* kubectl get pods --all-namespaces -o wide

* create the deployments/pods/containers

  kubectl create deployment [delployment-name] --image=[docker-image-name]
  
* create the service to expose the app

  kubectl create service [port-type]  [delployment-name] --tcp=8080:8080
  
* run the specific container

  kubectl run [deployment-name] --image=jenkins --replicas=2 --port=8080 --hostport=8888

* list the pods/nodes/deployments/services/[replicasets/rs]

  kubectl get [pods/pod/po]/[nodes/node/no]/[deploy/deployments]/[services/service/svc]/[replicasets/rs]
  
  kubectl get all >>> list all the deployments/rs/pods/svc

* list the pods/nodes/deployments/services/namespaces by name

  kubectl get [pods/pod/po]/[nodes/node/no]/[deploy/deployments]/[services/service/svc]/[replicasets/rs][namespaces/ns][hpa] [name]
  
  kubectl api-resources >> run this command what all resources we can get

* kubectl get pod [pod-1] [pod-2]

* kubectl get pod/[pod-1] rs/[ra-name-1] deploy/[deployment-name]

* kubectl get pod [pod-name] -o=[yaml/json/name/wide]

* To see the labels automatically generated for each pod, run the command "kubectl get pods --show-labels".

* description(information) of the pods/nodes/deployments/services

  kubectl describe [pods/pod/po]/[nodes/node/no]/[deploy/deployments]/[services/service/svc]/[replicasets/rs] [name]
  
  describe all the deployments: kubectl describe deploy
  
  decribe all the nodes: kubectl describe nodes
  
  describe all the pods: kubectl describe pods
  
* edit the deployments:

    kubectl edit deployment/nginx-deployment >> Once you run this command, deployment file will be opened, you can edit the image version here and save the file.
    
    kubectl rollout status deployment/nginx-deployment >> see the rollout status
    
    kubectl describe deployment/nginx-deployment

* Scaling a Deployment:

    kubectl scale deployment [deploymentName] --replicas=number
    ex: kubectl scale deployment devopsweb --replicas=10

    kubectl get deployment

    kubectl describe deployment devopsweb
    
    kubectl autoscale deployment [deploymentName] --min=10 --max=15 --cpu-percent=60
    ex: kubectl autoscale deployment devopsweb --min=10 --max=15 --cpu-percent=60

    kubectl get rs

* delete pods/nodes/deployments/services

  kubectl delete [pods/pod/po]/[nodes/node/no]/[deploy/deployments]/[services/service/svc]/[replicasets/rs] [name] 
  
  or
  
  kubectl delete [pods/pod/po]/[nodes/node/no]/[deploy/deployments]/[services/service/svc]/[replicasets/rs] [name] --force
  
* kubectl delete [pods|nodes|deploy|svc] --all

* kubectl logs [pod name]

    kubectl logs [pod name]
    
    kubectl logs [pod name] --all-containers=true
    
    kubectl logs --tail=20 [pod name] # Display only the most recent 20 lines of output in pod nginx
    
    kubectl logs -f -c ruby web-1 # Begin streaming the logs of the ruby container in pod web-1
    
    kubectl logs -p -c ruby web-1 
    
    kubectl logs --since=1h nginx # Show all logs from pod nginx written in the last hour
    
* Updating a Deployment

    Suppose that you now want to update the nginx Pods to use the nginx:1.9.1 image instead of the nginx:1.7.9 image.

    Command: kubectl set image deployment/nginx-deployment nginx=nginx:1.9.1


* kubectl apply:

  ex: kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"
  
* kubectl taint: By default, your cluster will not schedule pods on the master for security reasons. If you want to be able to schedule pods on the master, e.g. for a single-machine Kubernetes cluster for development, run below command. This will remove the node-role.kubernetes.io/master taint from any nodes that have it, including the master node, meaning that the scheduler will then be able to schedule pods everywhere.

  ex: kubectl taint nodes --all node-role.kubernetes.io/master-
  
* kubectl api-versions

* kubectl apply -f FILENAME [flags]

* kubectl [command] [TYPE] [NAME] --sort-by=<jsonpath_exp>

    ex: kubectl get pods --sort-by=.metadata.name
    
* kubectl get replicationcontroller <rc-name>

* kubectl exec POD [-c CONTAINER] [-i] [-t] [flags] [-- COMMAND [args...]]

    ex: kubectl exec [pod-name] date
    
    kubectl exec -ti <pod-name> /bin/bash --> it will interact with first created container
    
    kubectl exec -ti <pod-name> -c <container-name> /bin/bash --> it will interact with specified container, if you are not sure about the container name, describe pod.  kubectl describe pod [pod-name].
    
    kubectl exec -it [pod-name] -c [container-name] -- [command]
    
* kubectl plugin list

* kubectl cp [source] [destination]

     source : local-path
     destination: pod-name:container-path
     
     ex: kubectl cp index.html [nginx-pod-name]:[container-nginx-path]
     
* kubectl set resources

* kubectl rollout

    kubectl rollout status [deployment-name]
    kubectl rollout pause [deployment-name]
    kubectl rollout history [deployment-name]
    kubectl rollout resume [deployment-name]

#### All commands `kubectl --help`


        root@k8s-master:~# kubectl --help
        kubectl controls the Kubernetes cluster manager.

         Find more information at: https://kubernetes.io/docs/reference/kubectl/overview/

        Basic Commands (Beginner):
          create        Create a resource from a file or from stdin.
          expose        Take a replication controller, service, deployment or pod and expose it as a new Kubernetes Service
          run           Run a particular image on the cluster
          set           Set specific features on objects

        Basic Commands (Intermediate):
          explain       Documentation of resources
          get           Display one or many resources
          edit          Edit a resource on the server
          delete        Delete resources by filenames, stdin, resources and names, or by resources and label selector

        Deploy Commands:
          rollout       Manage the rollout of a resource
          scale         Set a new size for a Deployment, ReplicaSet or Replication Controller
          autoscale     Auto-scale a Deployment, ReplicaSet, or ReplicationController

        Cluster Management Commands:
          certificate   Modify certificate resources.
          cluster-info  Display cluster info
          top           Display Resource (CPU/Memory/Storage) usage.
          cordon        Mark node as unschedulable
          uncordon      Mark node as schedulable
          drain         Drain node in preparation for maintenance
          taint         Update the taints on one or more nodes

        Troubleshooting and Debugging Commands:
          describe      Show details of a specific resource or group of resources
          logs          Print the logs for a container in a pod
          attach        Attach to a running container
          exec          Execute a command in a container
          port-forward  Forward one or more local ports to a pod
          proxy         Run a proxy to the Kubernetes API server
          cp            Copy files and directories to and from containers.
          auth          Inspect authorization
          debug         Create debugging sessions for troubleshooting workloads and nodes

        Advanced Commands:
          diff          Diff live version against would-be applied version
          apply         Apply a configuration to a resource by filename or stdin
          patch         Update field(s) of a resource
          replace       Replace a resource by filename or stdin
          wait          Experimental: Wait for a specific condition on one or many resources.
          kustomize     Build a kustomization target from a directory or a remote url.

        Settings Commands:
          label         Update the labels on a resource
          annotate      Update the annotations on a resource
          completion    Output shell completion code for the specified shell (bash or zsh)

        Other Commands:
          api-resources Print the supported API resources on the server
          api-versions  Print the supported API versions on the server, in the form of "group/version"
          config        Modify kubeconfig files
          plugin        Provides utilities for interacting with plugins.
          version       Print the client and server version information

        Usage:
          kubectl [flags] [options]

        Use "kubectl <command> --help" for more information about a given command.
        Use "kubectl options" for a list of global command-line options (applies to all commands).
