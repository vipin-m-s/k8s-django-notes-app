Architecture:- 
<img width="1025" height="330" alt="Screenshot 2026-06-21 at 11 11 20 AM" src="https://github.com/user-attachments/assets/146b2155-cbb7-4227-9311-2a38c6e04c43" />

- Create a docker image from the django notes app Dockerfile
```
docker build -t [docker.io/vipinachar/django-notes-app:v1 ](https://docker.io/vipinachar/django-notes-app:v1) . 
```
- Push the Docker image to Docker hub 
```
docker push [docker.io/vipinachar/django-notes-app:v1](https://docker.io/vipinachar/django-notes-app:v1) 
```
- Create a namespace notes-app-ns where all the resources are created
```
apiVersion: v1
kind: Namespace
metadata:
  name: notes-app-ns
```
- Create a deployment with docker image from the docker hub 
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: notes-app-deployment
  namespace: notes-app-ns
spec:
   replicas: 1
   selector:
     matchLabels:
       app: django-notes-app
   template:
      metadata:
        name: django-notes-app-pod
        labels: 
          app: django-notes-app
      spec:
       containers: 
       - name: django-notes-app 
         image: docker.io/vipinachar/django-notes-app:v1
```
- Create a service to expose the docker deployment to outside world
```
kind: Service
apiVersion: v1
metadata:
   name: django-notes-app-service
   namespace: notes-app-ns
spec:
  selector:
    app: django-notes-app
  ports:
  - protocol: TCP
    port: 8000
    targetPort: 8000
```
- port forward the service to access it via Host IP address
```
k port-forward service/django-notes-app-service 8000:8000 -n notes-app-ns
```


