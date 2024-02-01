
### Step 1: Write a Simple Web Application (Python/Flask)

Create a file named `app.py` with the following content:

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello_tamatem():
    return 'Hello Tamatem!'

if __name__ == '__main__':
    app.run(debug=True, host='0.0.0.0', port=5000)
```

### Step 2: Create a Dockerfile

Create a `Dockerfile` :

```Dockerfile
FROM python:3.8-slim

WORKDIR /app

COPY requirements.txt /app/
RUN pip install --no-cache-dir -r requirements.txt

COPY . /app

CMD ["python", "app.py"]
```

### Step 3: Create a requirements.txt file

Create  `requirements.txt`:

```plaintext
Flask==3.0.1
```

### Step 4: Build and Test Locally

Build the Docker image and run the container locally to test the application:

```bash
docker build -t hello-tamatem-app .
docker run -p 5000:5000 hello-tamatem-app
```

visit `http://localhost:5000/` in your web browser to check if "Hello Tamatem!" is shown.

### Step 5: Create a Kubernetes Deployment

Create `k8s/deployment.yaml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello-tamatem-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: hello-tamatem-app
  template:
    metadata:
      labels:
        app: hello-tamatem-app
    spec:
      containers:
      - name: web
        image: hello-tamatem-app:latest
        imagePullPolicy: Never
        resources:
          requests:
            memory: "200Mi"
            cpu: "0.2"
          limits:
            memory: "200Mi"
            cpu: "0.2"
        ports:
        - containerPort: 5000
```

### Step 6: Create a Kubernetes Service

Create `k8s/service.yaml`:

```yaml
# service.yaml
apiVersion: v1
kind: Service
metadata:
  name: hello-tamatem-service
spec:
  selector:
    app: hello-tamatem-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 5000
  type: NodePort
```

### Step 7: Deploy to Minikube

Start a minikube cluster and apply the deployment and service configurations:

```bash
minikube -p tamatem-task start
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
```

### Step 8: Access the Web Application

Get the external IP of your minikube cluster:

```bash
minikube -p tamatem-task service hello-tamatem-service --url
```

Visit the provided URL in your web browser to see "Hello Tamatem!" displayed.

### Step 9: Cleanup

After testing, you can stop and delete the minikube cluster:

```bash
minikube -p tamatem-task stop
minikube -p tamatem-task delete
```

### Step 10: Upload to GitHub

Create a new GitHub repository and upload the `app.py`, `Dockerfile`, `requirements.txt`, `deployment.yaml`, and `service.yaml` files. Share the public repository link.

Note: Make sure to follow best practices for security, secrets management, and Kubernetes resource configurations in a production environment.