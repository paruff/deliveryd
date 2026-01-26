# Kubernetes Manifests for deliveryd

This directory contains Kubernetes manifests for deploying deliveryd to a Kubernetes cluster.

## Files

- `jenkins-pvc.yaml` - PersistentVolumeClaim for Jenkins data
- `jenkins-rbac.yaml` - ServiceAccount and RBAC for Jenkins
- `jenkins-deployment.yaml` - StatefulSet for Jenkins
- `jenkins-service.yaml` - Service for Jenkins
- `jenkins-ingress.yaml` - Ingress for Jenkins (requires ingress controller)

## Deployment

1. Create namespace:
   ```bash
   kubectl create namespace deliveryd
   ```

2. Create secrets:
   ```bash
   kubectl create secret generic dockerhub-credentials \
     --from-literal=username=your-username \
     --from-literal=token=your-token \
     -n deliveryd
   
   kubectl create secret generic jenkins-admin \
     --from-literal=password=your-secure-password \
     -n deliveryd
   ```

3. Apply manifests:
   ```bash
   kubectl apply -f k8s/
   ```

4. Check status:
   ```bash
   kubectl get pods -n deliveryd
   kubectl get svc -n deliveryd
   kubectl get ingress -n deliveryd
   ```

## Notes

- Update `jenkins-ingress.yaml` with your domain
- Adjust storage class in `jenkins-pvc.yaml` for your cluster
- The Jenkins image (`deliveryd/jenkins:latest`) needs to be built and pushed to a registry
- For production, enable TLS in ingress and use cert-manager

## Building and Pushing Custom Image

```bash
cd deliveryd
docker build -t your-registry/deliveryd-jenkins:latest -f jenkins/Dockerfile jenkins/
docker push your-registry/deliveryd-jenkins:latest

# Update jenkins-deployment.yaml with your image
```
