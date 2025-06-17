# bw-shot-exercise

## Exercises Overview

To be successful in the "Self-host Orchestration & Tooling Engineer" role, there are a few necessary skills. This role will be taking on
responsibility of a few key areas in an fairly autonomous way. The following exercises will demonstrate those needed
skills. Each one of these skills will be used on a weekly basis, if not a daily one.

1. Writing Dockerfiles
2. Writing docker-compose files
3. Deploying a Kubernetes cluster
4. Writing a Helm Chart

**Note:** Feel free to use documentation, articles, and/or videos on the topics here; just like you would do in such a
position in daily work. This is **NOT** a knowledge test. 

### Getting started

Fork this repo

Proceed to the exercises in `/exercises`

Once complete, share the link to your forked repo with your recruiter.

---

## Requirements

- pipenv
- docker

## Development

### Setup

```bash
pipenv install
pipenv install --dev
```

### Testing

```bash
pipenv run pytest
```

### Running Api

```bash
pipenv shell
uvicorn src.app:app
```
