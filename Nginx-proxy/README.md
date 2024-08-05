# NGINX Server Deployment and Proxy Configuration

## Section 1: Deploying an NGINX Server

### Step 1: Create a Deployment for the NGINX Server
Create a file named `nginx-deployment.yaml` and add the configuration for the NGINX deployment.

### Step 2: Create a Service to Expose the NGINX Deployment
Create a file named `nginx-service.yaml` and add the configuration for the NGINX service.

### Step 3: Create an Ingress Resource
Create a file named `nginx-ingress.yaml` and add the configuration for the Ingress resource.

### Step 4: Apply the YAML Files
Run the following commands to apply the YAML files to your Kubernetes cluster:

```sh
kubectl apply -f nginx-deployment.yaml
kubectl apply -f nginx-service.yaml
kubectl apply -f nginx-ingress.yaml
```

### Step 5: Verify the Setup
Check the status of your resources:

```sh
kubectl get deployments
kubectl get services
kubectl get ingress
kubectl get pods
```

## Section 2: Adding a Proxy Server for the Main Backend NGINX Server

### Step 1: Create a Deployment for the NGINX Proxy Server
Create a file named `nginx-proxy-deployment.yaml` and add the configuration for the NGINX proxy deployment.

### Step 2: Create a ConfigMap for the NGINX Proxy Configuration
Create a file named `nginx-proxy-configmap.yaml` and add the configuration for the NGINX proxy.

### Step 3: Create a Service for the NGINX Proxy
Create a file named `nginx-proxy-service.yaml` and add the configuration for the NGINX proxy service.

### Step 4: Create an Ingress Resource for the NGINX Proxy
Create a file named `nginx-proxy-ingress.yaml` and add the configuration for the Ingress resource.

### Step 5: Apply the YAML Files
Run the following commands to apply the YAML files to your Kubernetes cluster:

```sh
kubectl apply -f nginx-proxy-configmap.yaml
kubectl apply -f nginx-proxy-deployment.yaml
kubectl apply -f nginx-proxy-service.yaml
kubectl apply -f nginx-proxy-ingress.yaml
```

### Step 6: Verify the Setup
Check the status of your resources:

```sh
kubectl get deployments
kubectl get services
kubectl get ingress
kubectl get pods
```

### Step 7: Testing the Setup
Port-forward to the NGINX proxy service:

```sh
kubectl port-forward svc/nginx-proxy-service 8080:80
```

Access `http://localhost:8080` in your browser to verify the proxy is working.

### Step 8: Access the Ingress Controller
If you have an Ingress Controller installed, you can access the proxy server via the defined host:

```sh
http://proxy.example.nginx.bheru
```

or use the NodePort if configured:

```sh
http://<node-ip>:<node-port>
```

This documentation provides a step-by-step guide to deploying an NGINX server and adding an NGINX proxy server with ingress configuration.