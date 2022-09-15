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
$ kubectl get po 

```

1. Clean up

	```
	$ kubectl delete -f deploy2.yml
	```