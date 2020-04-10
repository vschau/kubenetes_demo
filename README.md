## Background
This web application allows users to log in and upload images to AWS S3 bucket.  User data is stored in Postgress RDS.  The application is deployed to Kubernetes on AWS.

## Install
- Download or clone repository
- Build docker images:
  - `docker-compose -f udacity-c3-deployment/docker/docker-compose-build.yaml build --parallel`
- Run app with `docker-compose up`
- Edit file `udacity-c3-deployment\k8s\aws-secret.yaml` to insert AWS credentials in base64
- Edit file `udacity-c3-deployment\k8s\env-configmap.yaml` to insert AWS environment variables
- Edit file `udacity-c3-deployment\k8s\env-secret.yaml` to insert Postgress username and password in base64
- Create a kubernetes cluster on AWS with KOPS/Minikube/KubeOne.  For KOPS, follow this instruction: https://github.com/kubernetes/kops/blob/master/docs/getting_started/aws.md
- Deploy the application to the cluster
  - Apply configmap
    `kubectl apply -f udacity-c3-deployment/k8s/env-configmap.yaml`
  - Apply secrets
    `kubectl apply -f udacity-c3-deployment/k8s/aws-secret.yaml`
    `kubectl apply -f udacity-c3-deployment/k8s/env-secret.yaml`
  - Apply services
    `kubectl apply -f udacity-c3-deployment/k8s/backend-feed-service.yaml`
    `kubectl apply -f udacity-c3-deployment/k8s/backend-user-service.yaml`
    `kubectl apply -f udacity-c3-deployment/k8s/frontend-service.yaml`
    `kubectl apply -f udacity-c3-deployment/k8s/reverseproxy-service.yaml`
  - Deploy
    `kubectl apply -f udacity-c3-deployment/k8s/backend-feed-deployment.yaml`
    `kubectl apply -f udacity-c3-deployment/k8s/backend-user-deployment.yaml`
    `kubectl apply -f udacity-c3-deployment/k8s/frontend-deployment.yaml`
    `kubectl apply -f udacity-c3-deployment/k8s/reverseproxy-deployment.yaml`
  - Expose the application locally
		`kubectl port-forward service/reverseproxy 8080:8080`
		`kubectl port-forward service/frontend 8100:8100`
  - In browser, open http://localhost:8100