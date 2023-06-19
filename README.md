# FastAPI GKE Deployment

This repository contains the necessary files and instructions to deploy a FastAPI application on Google Kubernetes Engine (GKE). Before proceeding with the deployment, make sure you have set up an Artifact Registry repository to store your container image.

## Prerequisites

Before proceeding with the deployment, ensure that you have the following prerequisites:

- [Google Cloud SDK](https://cloud.google.com/sdk) installed and configured.
- Docker installed on your local machine.

## Deployment Steps

1. Authenticate with Google Cloud:
   ```
   gcloud auth login
   ```

2. Set the project ID:
   ```
   gcloud config set project test-390300
   ```

3. Build the Docker image:
   ```
   docker build -t fastapi-gke-image .
   ```

4. Submit the Docker image to the Artifact Registry repository:
   ```
   gcloud builds submit --tag us-west1-docker.pkg.dev/test-390300/testapp/fastapi-gke-image
   ```

5. Create a GKE cluster:
   ```
   gcloud container clusters create fastapi-gke-cluster --zone us-central1-a --num-nodes 3
   ```

6. Apply the deployment configuration:
   ```
   kubectl apply -f deployment.yaml
   ```

7. Expose the deployment as a service:
   ```
   kubectl expose deployment testapp-deployment --type LoadBalancer --port 80 --target-port 8080
   ```

8. Retrieve the service details:
   ```
   kubectl get services
   ```

## Cleanup

To delete the deployment and the service, use the following commands:
```
kubectl delete deployment testapp-deployment
kubectl delete service testapp-deployment
```

Make sure to run these commands when you no longer need the deployed application.

For additional information and troubleshooting, refer to the official documentation provided by Google Cloud and Kubernetes.

**Note:** Before starting the deployment process, make sure you have set up an Artifact Registry repository to store your container image. This repository will serve as the source for your Docker image during the deployment process.

Enjoy your FastAPI application on GKE!