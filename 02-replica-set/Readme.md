# ReplicaSet

- Scale the pods (Supports manual scaling using `kubectl scale` command and autoscaling using `kubectl autoscale` command)

## Demo

```sh
$ kubectl apply -f replicaset1.yml
$ kubectl get rs 
$ kubectl scale rs/rs1 --replicas=2
$ kubectl get rs
$ kubectl scale rs/rs1 --replicas=5
$ kubectl scale rs/rs1 --replicas=0
```

- Provides Self healing to the pods

## Demo

```
$ kubectl apply -f replicaset1.yml
$ kubectl get po -w 
# Using new CMD/PWSH
$ kubectl delete po -l app=web1
$ kubectl get po -l app=web1
$ kubectl delete -f replicaset1.yml
```

## Autoscaling

1. HorizontalPodAutoscaler v1 (HPA-v1)

	```
	$ kubectl apply -f replicaset1.yml
	$ kubectl autoscale rs/rs1 --min=1 --max=10 --cpu-percent=50
	$ kubectl describe hpa rs1
	$ kubectl get po -l app=web1
	$ kubectl delete hpa rs1
	```

2. HPA v2 (Recommanded)

	```
	$ kubectl apply -f replicaset1.yml
	$ kubectl apply -f hpa-v2.yml
	$ kubectl get po -w
	$ kubectl delete -f hpa-v2.yml
	$ kubectl delete -f replicaset1.yml
	```
