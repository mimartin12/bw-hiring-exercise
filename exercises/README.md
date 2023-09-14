# Exercises 

To be successful in the "DevOps Engineer, Self-host" role, there are a few necessary skills. This role will be taking on
responsibility of a few key areas in an fairly autonomous way. The following exercises will demonstrate those needed
skills. Each one of these skills will be used on a weekly basis, if not a daily one.


**Note:** Feel free to use documentation, articles, and/or videos on the topics here; just like you would do in such a
position in daily work. This is **NOT** a knowledge test. 


## 1. Dockerfiles

Container images are the building blocks of our self-host stack. Understanding how to create them from scratch is
important for the success of this role. Use the provided application to build a Dockerfile


### Acceptance Criteria

- Dockerfile in the root of the repo 
- Dockerfile users python:3.11.3-slim as the base image
- The exposed port on the container is port 8080


### Validation
```
docker run -e BW_MESSAGE='hey' -p 8080:8080 $IMAGE_NAME

curl http://localhost:8080/         ## Expected result: {"message": "Hi from Bitwarden DevOps!"}
curl http://localhost:8080/custom   ## Expected result: {"message": "hey"}
curl http://localhost:8080/version  ## Expected result: {"version": "0.0.0"}
```


## 2. Docker Compose Files

Our self-host stack uses a docker compose file to orchestrate the Bitwarden containers on the end user's VMs. Build a
docker-compose file that has both the image created with the above Dockerfile and an nginx container in front of it.


### Acceptance Criteria

- docker-compose file in the root of the repo
- service named `app` that uses an image named `bw-devops-exercise` built from the Dockerfile in exercise 1.
- service running an nginx container in front of `app` available at port `8081`
- `app` is ONLY accessible through the nginx container and not from localhost



## 3. CI/CD

This position is going to be owning the entire CI/CD pipeline from front to back, including any/all automation of the
SDLC for self-host including managing versioning. While there are a lot of CI/CD tools out there, we had to choose a
single one for this exercise so we chose the one that we use at Bitwarden. This is *not* an exercise to examine skills
in GitHub Actions specifically. We will be looking for: 1) general understanding of how CI/CD flows work, and 2) ability
to translate needs from the SDLC to automation.

Cases:

- Run tests on each push to an PR, but not all pushes to all branches (`pytest tests/unit`)
- On every merge to `main`
  + A new versioned docker image is built and pushed 
  + GitHub tag is created with the version
- When a merge to `main` includes an update to the major or minor version in the `baseVersion` in the `/version.json`
  file
  + `patch` version is reset to `0`
  + A new versioned docker image is built and pushed (we suggest GitHub's built in container registry, but any container
    registry that read only access can be granted to the hiring manager)
  + The latest docker image tag is pointed to the new version
  + A GitHub Release should be created with notes "Releasing $version" in AND automatically generated release notes.
  + **Note:** The `version` key should never exist in `version.json` in the `main` branch.


Resources:

- [container registry on the repo](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry#about-the-container-registry)
- [GLEW branching strategy (this exercise is loosely modeled after GLEW)](https://sam.gleske.net/blog/engineering/2019/11/12/git-low-effort-workflow.html)


### Acceptance Criteria

- Pipeline(s) to meet all of the Cases above
- The resulting version tagged image `/version` endpoint responds with their version instead of "0.0.0"



## [Optional] 4. Helm Chart

One of the things that this position is going to be designing, building, and maintaining an official Helm Chart install
of our self-host deployment.


### Acceptance Criteria

- A Helm Chart that has
  + Two deployments: 1) the python app, and 2) the nginx proxy; configurable image versions
  + ConfigMap for the Nginx configuration
  + Ingress pointing to the nginx proxy; configurable for TLS and custom hostname
