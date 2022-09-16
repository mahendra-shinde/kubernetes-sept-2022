# Ingress

## USe Case

HostName:  				mahendra.com
Original Request:  		mahendra.com/webapp1/index.html

Forwared to 			http://webapp1-svc/index.html


ReWrite Path : /$1

Request Path Pattern: /webapp1(.*)

For every request path, it would store path segment after "webapp1" in 
Variable "$1" then while forwarding request it will drop webapp1 but retain the value of $1

# Demo

1. Install nginx ingress in cluster (Shared by all namespaces)

	```
	kubectl create ns nginx
	helm install ingress -n nginx bitnami/nginx-ingress-controller
	kubectl get all -n nginx
	```

2. Install the sample application and it's services

	```
	 kubectl apply -f https://raw.githubusercontent.com/mahendra-shinde/kubernetes-demos/master/14-ingress/deployment.yml
	 kubectl get pod,svc
	 
	```

3.	Create `routes.yml`

	```yml
	apiVersion: networking.k8s.io/v1
	kind: Ingress
	metadata:
		name: my-app
		labels:
			name: my-app
		annotations:
			nginx.ingress.kubernetes.io/rewrite-target: /$1
	spec:
		ingressClassName: nginx
		rules:
		#- host: <Host>
		- http:
			paths:
			- pathType: Prefix
			  path: "/YORUNAME/webapp1(.*)"
			  backend:
			    service:
			      name: webapp1-svc
				  port: 
				    number: 80
			- pathType: Prefix
			  path: "/YOURNAME/webapp2(.*)"
			  backend:
			    service:
			      name: webapp2-svc
		   		  port: 
				    number: 80
			- pathType: Prefix
			  path: "/YOURNAME/(.*)"
			  backend:
			    service:
				  name: webapp3-svc
			      port: 
				    number: 80
	```

4.	Deploy the ingress

	```
	kubectl apply -f routes.yml
	```

5. Test the ingress using following URLs

	http://20.85.250.252/YOURNAME/webapp1

	http://20.85.250.252/YOURNAME/webapp2

	http://20.85.250.252/YOURNAME/