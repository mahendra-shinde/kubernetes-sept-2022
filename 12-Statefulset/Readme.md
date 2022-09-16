## Challenges in "Deployment" and "ReplicaSet"

1. Unordered or unpredictable pod creation and deletion.

2. Volume binding
	
	When pod is "self healed" then the "new" pod should have "claim" on old-pod's
	volume.


# StatefulSet

1. Similar to "deployment" objects. 
2. Manually scale or Scale using HPA
3. Ordered Creation and deletion
		Every new pod is named as "StateFulSetName-Index"
			where index = lastIndex+1

	Example "StatefulSet" with name "web" and replicas count 3 would have pods:
	* web-0
	* web-1
	* web-2

4. Scaling always affects the LAST pods

	- Last one is First one to DELETE
	- Every new POD is Last pod

5.  When "POD" is deleted, it doesn't delete PVC assigned to that pod.

## Demo

```
kubectl apply -f web.yml
kubectl get po -l app=app10
kubectl scale sts/web --replicas=5
kubectl get po -l app=app10 -w
kubectl scale sts/web --replicas=0
kubectl scale sts/web --replicas=0
kubectl delete -f web.yml
```