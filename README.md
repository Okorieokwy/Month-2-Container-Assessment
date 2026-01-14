# Month 2 Container Assessment

## Project Overview
This project is a containerized application designed to demonstrate the deployment and management of a microservices-based architecture. It includes a backend service and a MongoDB database, orchestrated using Kubernetes. The project also provides scripts for building, running, and deploying the application.

## Project Structure
```
docker-compose.yml
Dockerfile
application-code/
    go.mod
    main.go
Evidence/
kubernetes/
    ingress.yaml
    namespace.yaml
    backend/
        backend-configmap.yaml
        backend-deployment.yaml
        backend-secret.yaml
        backend-service.yaml
        deployment.yaml
    mongodb/
        mongodb-configmap.yaml
        mongodb-deployment.yaml
        mongodb-pvc.yaml
        mongodb-secret.yaml
        mongodb-service.yaml
scripts/
    docker-build.sh
    docker-run.sh
    k8s-cleanup.sh
    k8s-deploy.sh
```

### Key Directories and Files
- **application-code/**: Contains the Go application source code.
- **kubernetes/**: Contains Kubernetes manifests for deploying the backend and MongoDB services.
  - `backend/`: Backend service configuration files.
  - `mongodb/`: MongoDB service configuration files.
- **scripts/**: Shell scripts for building Docker images, running containers, and managing Kubernetes deployments.
- **docker-compose.yml**: Defines services for local development using Docker Compose.
- **Dockerfile**: Docker image definition for the application.

## Prerequisites
- Docker
- Kubernetes (kubectl)
- Minikube or a Kubernetes cluster
- Helm (optional, for advanced deployments)
- Bash shell (for running scripts)

## Setup Instructions

### 1. Clone the Repository
```bash
git clone <repository-url>
cd "Month 2 Container Assessment"
```

### 2. Build the Docker Image
Use the provided script to build the Docker image:
```bash
./scripts/docker-build.sh
```

Alternatively, you can build the image manually:
```bash
docker build -t backend-app .
```

### 3. Run the Application Locally
Use Docker Compose to run the application locally:
```bash
docker-compose up
```

### 4. Deploy to Kubernetes

#### Step 1: Create the Namespace
Ensure the namespace is created before deploying resources:
```bash
kubectl apply -f kubernetes/namespace.yaml
```

#### Step 2: Deploy MongoDB
Apply the MongoDB manifests:
```bash
kubectl apply -f kubernetes/mongodb/
```

#### Step 3: Deploy the Backend Service
Apply the backend manifests:
```bash
kubectl apply -f kubernetes/backend/
```

#### Step 4: Configure Ingress
Apply the ingress configuration:
```bash
kubectl apply -f kubernetes/ingress.yaml
```

#### Step 5: Verify the Deployment
Check the status of the pods:
```bash
kubectl get pods -n muchtodo
```

Check the ingress:
```bash
kubectl get ingress -n muchtodo
```

### 5. Clean Up Resources
To clean up the Kubernetes resources, use the provided script:
```bash
./scripts/k8s-cleanup.sh
```

## Environment Details

### Kubernetes Namespace
The application is deployed in the `muchtodo` namespace. Ensure all commands are executed within this namespace.

### Backend Service
- **Deployment File**: `kubernetes/backend/backend-deployment.yaml`
- **Service File**: `kubernetes/backend/backend-service.yaml`
- **ConfigMap**: `kubernetes/backend/backend-configmap.yaml`
- **Secrets**: `kubernetes/backend/backend-secret.yaml`

### MongoDB Service
- **Deployment File**: `kubernetes/mongodb/mongodb-deployment.yaml`
- **Service File**: `kubernetes/mongodb/mongodb-service.yaml`
- **ConfigMap**: `kubernetes/mongodb/mongodb-configmap.yaml`
- **Secrets**: `kubernetes/mongodb/mongodb-secret.yaml`
- **Persistent Volume Claim**: `kubernetes/mongodb/mongodb-pvc.yaml`

### Ingress
- **Ingress File**: `kubernetes/ingress.yaml`
- **Host Configuration**: Ensure the ingress host matches your DNS or `/etc/hosts` file.

## Scripts
- **docker-build.sh**: Builds the Docker image.
- **docker-run.sh**: Runs the Docker container locally.
- **k8s-deploy.sh**: Deploys the application to Kubernetes.
- **k8s-cleanup.sh**: Cleans up Kubernetes resources.

## Troubleshooting

### Common Issues
1. **Pods Not Starting**:
   - Check pod logs:
     ```bash
     kubectl logs <pod-name> -n muchtodo
     ```
   - Verify ConfigMaps and Secrets are correctly applied.

2. **Ingress Not Working**:
   - Check ingress status:
     ```bash
     kubectl describe ingress -n muchtodo
     ```
   - Ensure DNS or `/etc/hosts` is configured correctly.

3. **Database Connection Issues**:
   - Verify MongoDB service is running:
     ```bash
     kubectl get svc -n muchtodo
     ```
   - Check MongoDB logs for errors.
   

## Acknowledgments
- Kubernetes documentation
- Docker documentation
- Community tutorials and resources