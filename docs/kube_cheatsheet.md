# KUBERNETES CHEAT SHEET 


## Help

```
kubectl --help
```

NB: Everything you need to know about `kubectl` usage is found here [kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

## Display cluster nodes
```
kubectl get nodes 
kubectl get nodes -o=wide # show IP address, etc.
```

## View kubectl config (allows non-root management of cluster)

```
ls -la /home/amia/.kube
```

## Display pods

```
kubectl get pods # display default namespace only
kubectl get pods --all-namespaces # display all namespaces
kubectl get pods --namespace=kube-system # display kube-system namespace
kubectl get pods dnstools #display dnstools pod only from default namespace
```

## Display services and endpoints

```
kubectl get service # display default namespace only -> svc is shorthand for service
kubectl get service --all-namespaces # display all namespaces
kubectl get service --namespace=kube-system # display kube-system namespace

kubectl get ep # display default namespace only 
kubectl get ep --all-namespaces # display all namespaces
kubectl get ep --namespace=kube-system # display kube-system namespace

```

## Create dnstools pod from spec

```
kubectl create -f /home/amia/tutorial/specs/dnstools.yaml
```

## View pod details and logs

```
kubectl get pods dnstools
kubectl describe pods dnstools
kubectl logs dnstools
```

## Lookup (dig) for local cluster resources and get time to response for service/ep in default namespace by servicename.default, using cluster name server for lookup (10.96.0.1)

```
kubectl get services # get service name; NB: can specify as services and svc
kubectl get endpointp # show service endpoint; NB: can specify as ep
kubectl exec -ti dnstools -- time dig @10.96.0.10 kubernetes.default #redirects DNS queries to local cluster DNS
```

## Dig for local cluster resources (kube dns) and get time to response for service/ep in kube-system namespace by servicename.kube-system

```
kubectl get service --namespace=kube-system # get service name
kubectl get endpoints --namespace=kube-system # show service endpoint
kubectl exec -ti dnstools -- time dig @10.96.0.10 kube-dns.kube-system
```

## Dig for external resources and get time to response

```
kubectl exec -ti dnstools -- time dig @10.96.0.10 google.com
```






