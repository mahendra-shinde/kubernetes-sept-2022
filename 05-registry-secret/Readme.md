# Private Registry

1. Create registry secret using registry login server url, username and password.

	```
	kubectl create secret docker-registry my-reg `
		--docker-server mahendrashinde.azurecr.io `
		--docker-username mahendrashinde `
		--docker-password pKCQJFgu/kh8tnf4PhVB3bMO7o8V+lu4 `
		
	```

2. Create Pod/ReplicaSet/Deployment with "ImagePullSecret" attribute

	```yml
	imagePullSecrets:
    - name: my-reg
	```

3.	Deploy the pod

	```
	$ kubectl apply -f pod1.yml
	$ kubectl describe -f pod1.yml
	```