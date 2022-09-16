# Services 

Kubernetes has THREE types of services

1. ClusterIP (Default)

	Internal service, not accessible from outside cluster. Has Virtual IP Address and DNS
	DNS depends on CoreDNS or KubeDNS extension installed in cluster.

	Endpoint Controller (Part of Controller Manager deployed on Master) will keep sync with the pods in backend.
	Service Controller would take care of actual routing and load-balancing.

2. NodePort

	Extension of "ClusterIP" service. Expose your services (Port Forward) to a node-port in range 30000-39999.
	Registers service with "kube-proxy"
	Example:
		Any request on port 30001, please send it to product-service (cluster-ip)
		Any request on port 30002, please send it to order-service (cluster-ip)

3. LoadBalancer (Implementation depends on Vendor like Azure or AWS)

	Allows clients to access SINGLE IP for service.  Load Balancer service is Extension of NodePort Services.

## Traffic Routing example for node-port
External Client made a request
   http://192.168.1.100:30001/.../.../..

1. Request is RECEIVED by node-1 (192.168.1.100)
2. Node-1 will check "Which process is bound to "30001"
		Found "kube-proxy"
3. Kube-proxy will verify "NodePort" service and find "ClusterIP"
	and then forward request to http://product-service or http://10.0.1.15
4. Request is RECEIVED by cluster-ip service, 
		Cluster IP service will perform "Load Distribution" and pick a POD
5. Request is RECEIVED by the POD


Limitations
1. Client need to send request to any ONE node.
2. Sharing node-ip with external client is NOT safe !
3. Also not-reliable ?


Valid Range : 30000 ==> 39999
product-service >>> node1:30001
order-service   >>> node1:30002


Kubernetes provides ClusterIP and NodePORT, but for also has "Support" for load-balancer (Public) service

on AKS  will get Azure LB
on EKS  will get Elastic LB
Docker-Desktop & Minikube will get "localhost"

Internal Service Discovery and DNS Names

Service deployed in "mahendra" namespace, can be accessed by ANY POD inside same namespace with URL "http://internal-service" or http://10.0.1.15
But, if a pod from ANOTHER namespace need to call this service then it must use
		"http://internal-service.mahendra"  or http://10.0.1.15


## Demo 1 : Cluster IP Service

```pwsh
 kubectl apply -f deploy1.yml
 kubectl apply -f service.yml
 kubectl get svc
 kubectl get po -l app=app7
 kubectl get ep internal-service
  $PODNAME=$(kubectl get po -l app=app7 -o=jsonpath="{ .items[0].metadata.name  }")
 kubectl exec -it $PODNAME -- sh
 curl http://internal-service
 exit
```

# Demo 2 :  NodePort Service

```bash
 kubectl apply -f deploy1.yml
 kubectl apply -f service2.yml
 kubectl get svc
 kubectl get po -l app=app7
 kubectl get ep np-service
  $PODNAME=$(kubectl get po -l app=app7 -o=jsonpath="{ .items[0].metadata.name  }")
 kubectl exec -it $PODNAME -- sh
 curl http://np-service
 curl http://10.240.1.81:31428
 exit

```

