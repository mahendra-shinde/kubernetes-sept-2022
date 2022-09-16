# Helm : Kubernetes Package Manager

> Helm is "third party" package manager for Kubernetes.

What can helm do ?
1. Install packages or extensions from helm repositories.
1. Convert your application as package and share it with other team members.
1. Manage individual deployed package.

# Installing Helm

1. Download helm from https://github.com/helm/helm/releases/tag/v3.9.4
1. Extract the ZIP file into C:\helm
1. Make sure C:\helm contains "helm.exe"
1. Add "C:\helm" to PATH env variable
1. Launch new CMD or PWSH and try following command:

	```
	helm version		# Print version info
	helm list -A		# List pre-installed packages
	```
## Adding new Repository

```
  helm repo add nginx-stable https://helm.nginx.com/stable
  helm repo update
```

## Installing RabbitMQ

```
 helm repo add bitnami https://charts.bitnami.com/bitnami
 helm install my-release bitnami/rabbitmq
 kubectl get all -l  app.kubernetes.io/name=rabbitmq
```

## Access RabbitMQ Dashboard

```
 echo "URL : http://localhost:15672/"
 kubectl port-forward svc/my-release-rabbitmq 15672:15672
```

Now access the URL using any web browser
Get the password using following command:

```
 echo "Password      : $(kubectl get secret --namespace mahendra my-release-rabbitmq -o jsonpath="{.data.rabbitmq-password}" )"  
```
Copy the password (Which is base64 encoded value) and visit https://base64decode.org
and Decode it !

## Uninstall RabbitMQ

```
 helm uninstall my-release
```

