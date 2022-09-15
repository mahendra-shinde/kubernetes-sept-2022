# Deployment

- Providing RollingUpdate and Recreate Strategy for application
- Maintain revision history and allow rollback (Undo) 

## Demo : Recreate

```
$ kubectl apply -f deploy1.yml
$ kubectl get rs -o wide
$ kubectl set image deploy/app1 myapp=mahendrshinde/myweb:2 --record
$ kubectl get rs -o wide
$ kubectl rollout history deploy/app1
$ kubectl set image deploy/app1 myapp=mahendrshinde/myweb:3 --record
$ kubectl rollout history deploy/app1
$ kubectl get rs -o wide
$ kubectl delete -f deploy1.yml
```