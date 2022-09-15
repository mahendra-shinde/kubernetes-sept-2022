Health Probe:
	
	Check if application is "RESPONDING" (Running)

	http probe:		Send "HTTP GET" request to certain path (User defined) with user defined frequency
						Does expect either 200 Or 100 Http Status Code

	TCP probe:		Send "few packates" of data to pod and expect them to return (Ping or Telnet)
	
	CMD Probe:		Run a command and expect it return EXIT CODE "Zero"

-------------------------------------------------------------------------------------
StartupProbe:
	
	Block all incoming requests untill application is UP and RUNNING.
	Also block other "probes" like "LivenessProbe" and "ReadinessProbe"
	
	Once, startup-probe returns a "SUCCESS", it will stop repeating itself.


LivenessProbe:
	Check if application is RUNNING or else RESTART it !
	

ReadinessProbe:
	Block all incoming requests untill application is UP and RUNNING.
	It will NEVER restart application.

	Readiness Probe will KEEPING repeating event after application is Ready.

# Liveness Probe Demo

```
$ kubectl apply deploy2.yml
$ kubectl get po -l app=myapp5
## Use Windows Powershell ##
$PODNAME=$(kubectl get po -l app=myapp5 -o=jsonpath="{ .items[0].metadata.name  }")
$ kubectl exec -it $PODNAME -- sh
$ rm /usr/share/nginx/html/index.html
$ exit
## Wait for 10 seconds
$ kubectl logs $PODNAME
# Wait for 20 Seconds more
$ kubectl logs $PODNAME
$ kubectl get po -l app=myapp5

```

Clean up

	```
	$ kubectl delete -f deploy2.yml
	```

## Readiness probe demo

```
$ kubectl apply -f deploy1.yml
$ kubectl apply -f service.yml
$ kubectl get po -l app=myapp4
$ kubectl get ep probe-app
$ kubectl describe ep probe-app
$PODNAME=$(kubectl get po -l app=myapp4 -o=jsonpath="{ .items[0].metadata.name  }")
$ kubectl exec -it $PODNAME -- sh
$ cd /usr/share/nginx/html
$ mv index.html index.txt
$ exit
# Wait for 30 seconds
## Expected: One pod in "NotAvailable" group
$ kubectl describe ep probe-app
$PODNAME=$(kubectl get po -l app=myapp4 -o=jsonpath="{ .items[0].metadata.name  }")
$ kubectl exec -it $PODNAME -- sh
$ cd /usr/share/nginx/html
$ mv index.txt index.html
$ exit
# Wait for 30 seconds
## Expected: Zero pod in "NotAvailable" group
$ kubectl describe ep probe-app
```

Clean up

	```
	$ kubectl delete -f deploy2.yml
	```
