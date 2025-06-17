# Exercises

To be successful in the "Self-host Orchestration & Tooling Engineer" role, there are a few necessary skills. This role will be taking on
responsibility of a few key areas in an fairly autonomous way. The following exercises will demonstrate those needed
skills. Each one of these skills will be used on a weekly basis, if not a daily one.

**Note:** Feel free to use documentation, articles, and/or videos on the topics here; just like you would do in such a
position in daily work. This is **NOT** a knowledge test.

## 1. Exercise 1: Dockerfiles

Container images are the building blocks of our self-host stack. Understanding how to create them from scratch is
important for the success of this role. Use the provided application to build a Dockerfile

### Exercise 1: Acceptance Criteria

- Dockerfile in the root of the `roles/SHOT/` directory
- Dockerfile users python:3.12.5-slim as the base image
- The exposed port on the container is port 8080

### Validation

```bash
docker run -e BW_MESSAGE='hey' -p 8080:8080 $IMAGE_NAME

curl http://localhost:8080/         ## Expected result: {"message": "Hi from Bitwarden!"}
curl http://localhost:8080/custom   ## Expected result: {"message": "hey"}
```

## 2. Exercise 2: Docker Compose Files

Our self-host stack uses a docker compose file to orchestrate the Bitwarden containers on the end user's VMs. Build a
docker-compose file that has both the image created with the above Dockerfile and an nginx container in front of it.

### Exercise 2: Acceptance Criteria

- docker-compose file in the root of the repo
- service named `app` that uses an image named `bw-shot-exercise` built from the Dockerfile in exercise 1.
- service running an nginx container in front of `app` available at port `8081`
- `app` is ONLY accessible through the nginx container and not from localhost

## 3. Exercise 3: Kubernetes

This role maintains Helm charts for the self-host stack and is responsible for creating and testing new features for our charts. Create a Kubernetes cluster that you can use in our next exercise. Choose your tool of choice, if you use a script or IaC, place it into the `roles/SHOT/kubernetes` directory.

Some examples of tools you can use:

- Terraform/Pulumi for a cloud hosted Kubernetes cluster
- KIND/Minikube for a local Kubernetes cluster

### Exercise 3: Acceptance Criteria

- Kuberentes cluster is running and deployment is reproducible
- Any IaC files exist in the `roles/SHOT/kubernetes` directory

## 4. Exercise 4: Helm Chart

This role is responsible for maintaining the official Bitwarden Helm Charts. Create a Helm chart that deploys the Docker image you built in exercise 1. Place the chart into the `roles/SHOT/kubernetes` directory.

### Exercise 4: Acceptance Criteria

- A Helm Chart that has
  + One deployment of the python app; configurable image versions
  + Ingress pointing to the python app; configurable for TLS and custom hostname
  + A values file that can be used to configure the chart
  + A test file that tests the chart
  + Passed `helm lint`

### Exercise 4: Validation

```bash
helm lint roles/SHOT/kubernetes/helm-chart ## Expected result: Linting passed
helm install bw-shot-exercise roles/SHOT/kubernetes/helm-chart ## Expected result: Deployment created
curl -k https://bw-shot-exercise.example.com ## Expected result: {"message": "Hi from Bitwarden!"}
```
