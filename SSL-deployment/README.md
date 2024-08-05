# Applying a Self-Signed SSL Certificate in Kubernetes

## Prerequisites

1. A Kubernetes cluster set up.
2. `kubectl` installed and configured to interact with your cluster.
3. OpenSSL installed on your local machine.

## Steps to Create and Apply a Self-Signed SSL Certificate

### 1. Generate a Private Key

```sh
openssl genrsa -out tls.key 2048
```

### 2. Generate a Certificate Signing Request (CSR)

```sh
openssl req -new -key tls.key -out tls.csr
```

### 3. Generate a Self-Signed Certificate

```sh
openssl x509 -req -in tls.csr -signkey tls.key -out tls.crt -days 365
```

### 4. Create a Kubernetes Secret to Store the TLS Certificate

```sh
kubectl create secret tls tls-secret --cert=tls.crt --key=tls.key
```

### 5. Apply the Secret to Your Kubernetes Cluster

```sh
kubectl apply -f - <<EOF
apiVersion: v1
kind: Secret
metadata:
  name: tls-secret
type: kubernetes.io/tls
data:
  tls.crt: $(cat tls.crt | base64 | tr -d '\n')
  tls.key: $(cat tls.key | base64 | tr -d '\n')
EOF
```

### 6. Update Your Deployment to Use the Secret

Modify your deployment YAML to mount the secret as a volume and reference it in your container. Below is an example snippet of a deployment file.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: your-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: your-app
  template:
    metadata:
      labels:
        app: your-app
    spec:
      containers:
      - name: your-container
        image: your-image
        ports:
        - containerPort: 443
        volumeMounts:
        - name: tls-volume
          mountPath: /etc/tls
          readOnly: true
      volumes:
      - name: tls-volume
        secret:
          secretName: tls-secret
```

### 7. Expose Your Application Using an Ingress

Create or update an Ingress resource to use the TLS certificate.

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: your-ingress
  annotations:
    nginx.ingress.kubernetes.io/ssl-redirect: "true"
spec:
  tls:
  - hosts:
    - your-domain.com
    secretName: tls-secret
  rules:
  - host: your-domain.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: your-service
            port:
              number: 80
```

### 8. Apply the Ingress Resource

```sh
kubectl apply -f your-ingress.yaml
```

## Conclusion

You have now successfully applied a self-signed SSL certificate to secure your website served from a Kubernetes cluster. Your application should now be accessible via HTTPS.