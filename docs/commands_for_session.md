# W22: Large Scale Ensembled NLP Systems with Docker and Kubernetes
## AMIA 2019, Washington, DC, 11/17/2019


## 1. Access the Virtual Machine (VM)
     ssh amia@<your_ip_address>

The password is (case sensitive): `Tutorial2019`

## 2. Explore your cloud image

Common Linux commands: 

     ls     # list the files in a directory
     pwd    # display the current working directory
     cd     # change directories
     less   # scroll the contents of the file
     cat    # display all contents of file at once
     nano   # a way to edit text
     vim    # another, more complicated way to edit text

The use of `-la` to list current directory with full attributes, and `~/` is equivalent to `/home/amia`. 

To look into the OS release, home directory and `/bin`: 

     cat /etc/os-release
     ls /home/amia
     ls -la /bin

## 3. Basic Docker Commands (Docker build!)
 1. Now list available docker images on VM: 

        docker images  
        docker --help

 1. Pull a docker image (`whalesay`) from Docker hub:

        docker pull docker/whalesay 

1. Run docker image's version of "Hello World": 

        docker run docker/whalesay cowsay "Hello W22" 
     
## 4. Let's use Docker!
 1. Pull our ML image from the `nlpieumn` repository from Docker hub:

        docker pull nlpieumn/ml 

 2. Start with `bash` in the ML image:

        docker run -it nlpieumn/ml /bin/bash 
     
 3. Explore your docker image:

        cat /etc/os-release
        ls /home/tutorial
        ls /bin 
        exit   # when you're done
     
## 5. Build your own docker image
  
Build the `vote` image from Dockerfile at target "vote":

    cd ~/tutorial
    ls -la
    less Dockerfile
    
Build image from Dockerfile at target = "vote"; (need to set env variable BUILDKIT=1)

    docker build -t nlpieumn/vote --target vote . 

## 6. Let's use Kubernetes! 

 1. List all nodes in "cluster":
     
        kubectl get nodes 

 2. List the config file that allows unprivileged user to run commands:

        ls /home/amia/.kube 

 3. List all pods in the default namespace, all namespaces and all services:

        kubectl get pods 
        kubectl get pods --all-namespaces
        kubectl get services

## Kubernetes network troubleshooting resources 
Advanced network diagnostics (using newly created pod) 

Usage: Determine if built-in kubernetes DNS is functioning properly
Lookup (aka dig) a local cluster resources and get time to response for service/ep in `kube-system` namespace by `servicename.kube-system` using cluster name server for lookup (`10.96.0.1`)

Check for internal kubernetes network communications functionality. 

1. Get service name from kube-system namespace
```
    kubectl get service --namespace=kube-system
```
2. Query by endpoint name
```
    kubectl exec -ti dnstools -- time dig @10.96.0.10 kube-dns.kube-system 
```

2. Show service endpoint name
```
    kubectl get endpoints --namespace=kube-system  
```

2. Lookup for external resources and get time to response. Check for external network communications functionality 
```  
    kubectl exec -ti dnstools -- time dig @10.96.0.10 google.com
```

## 7. Ensemble models for Word Sense Disambiguation (WSD) 

The standalone script can also run as `python ~/tutorial/scripts/ml.py`. To run svm classifier from within the docker image:
```
    docker run -it -e DOCKER='True' -v /home/amia/tutorial:/data nlpieumn/ml python /home/tutorial/ml.py -c svm
```
     
## 8. Let's use Argo!

 1. List all argo workflows:

        argo list

 2. Validate yaml file:

        nano specs/evaluation.yaml 
        argo lint specs/evaluation.yaml 

 3. Submit argo workflow spec and watch status in real time:

        argo submit --watch specs/evaluation.yaml 

 4. List workflow pods in workflow and view the logs:

        argo get <workflow_name> 
        argo logs <pod_name> 
