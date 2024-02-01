### Build and Test Locally

Build the Docker image and run the container locally to test the application:

```bash
docker build -t hello-tamatem-app .
docker run -p 5000:5000 hello-tamatem-app
```

visit `http://localhost:5000/` in your web browser to check if "Hello Tamatem!" is shown.

### Deploy to Minikube

Start a minikube cluster and apply the deployment and service configurations:

```bash
minikube -p tamatem-task start
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
```

#### Access the Web Application

Get the external IP of your minikube cluster:

```bash
minikube -p tamatem-task service hello-tamatem-service --url
```

Visit the provided URL in your web browser to see "Hello Tamatem!" displayed.

### Cleanup

After testing, you can stop and delete the minikube cluster:

```bash
minikube -p tamatem-task stop
minikube -p tamatem-task delete
docker image rm hello-tamatem-app
```
