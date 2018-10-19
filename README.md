# Using GitLab for CI/CD on Docker Swarm

This repository is used as a demonstration on How we can leverage GitLab to 
automatically deploy an application on a docker Swarm cluster for the airports v1.0.1 and countries v1.0.1  
(sample Java services for test purpose)


## What

Since we deploy our application on a [Docker Swarm
Mode](https://docs.docker.com/engine/swarm/) cluster, we will make use of
[Docker overlay
network](https://docs.docker.com/engine/userguide/networking/get-started-overlay/#overlay-networking$
and create a `public` . Our application will have 2
frontend services that must be exposes on the Internet, so we set-up a double
network attachment on public network. The
[Traefik](https://traefik.io/) Reverse Proxy role is to dispatch our requests
from internet to the frontend services and will be listening only on the public
Network. Only Traefik will exposes ports on the swarm.

This microservices application can be describe as :

- A Docker Swarm **public network** name `traefik_net` to expose our frontend
  services :
    - a **Traefik** reverse Proxy which is the public endpoint for the 2
      frontend services:
    - a **countries** **Java** web-app, which lets you display a list of countries by reference name
    - a **airports** **Java** web-app which shows the list of airports.

![Application architecture](architecture.png)

## How

This is a sampl dockerzing of the original [Java Assembly Testing
application]on which I
added :
- a `traefik.yml` docker stack file to create the traefik stack and docker
  public network
- a `docker-compose-gitlab.yml` file to build and push each microservice app in
  the gitlab docker registry
- a `docker-stack-traefik.yml` stack file to deploy tha application as a docker
  stack
- a `.gitlab-ci.yml` file to enable our Gitlab Pipeline
- a `Dockerfile` for each Java service
