# Student Management System: Automated CI/CD

Spring Boot REST API (CRUD for students) with a full CI/CD pipeline:
GitHub Actions -> build + test -> Docker image -> Docker Hub -> Kubernetes.

## Run locally
    mvn spring-boot:run
    curl -X POST localhost:8080/api/students -H "Content-Type: application/json" \
      -d '{"name":"Harsh","email":"harsh@example.com","course":"CSE"}'
    curl localhost:8080/api/students

## Run with Docker
    docker build -t student-management .
    docker run -p 8080:8080 student-management

## Deploy to Kubernetes (manual)
    kubectl apply -f k8s/
    # minikube: minikube service student-management

## GitHub secrets needed
- DOCKERHUB_USERNAME
- DOCKERHUB_TOKEN
- KUBE_CONFIG (base64 of your kubeconfig: `base64 -w0 ~/.kube/config`)

## Pipeline
1. build-test: `mvn verify` (runs unit tests)
2. docker: builds image, pushes `:sha` and `:latest` (main branch only)
3. deploy: updates image tag, `kubectl apply`, waits for rollout
CI demo
