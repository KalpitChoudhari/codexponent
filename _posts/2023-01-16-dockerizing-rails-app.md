---
layout: post
title:  "Dockerize Ruby on Rails Application containing private repository or mounted engine in it
"
author: Mayuresh
categories: [ Docker, Rails ]
featured: true
image: assets/images/docker-ror.png
---
Dockerizing a ruby on rails application in which you have mounted a **Rails Engine** or include **private repository** in Gemfile is a bit tricky.

Bundle install on Gemfile needs ssh access to access the engine or private repo

so, while creating docker image need to pass ssh access keys to docker container 

for that you need to follow the following process

first of all, you need to add below comment on the top of the Dockerfile:

```docker
# syntax=docker/dockerfile:experimental
```

Note: As adding ssh access to docker container may change in future revisions

Next step is to pass ssh  private key 🔑  and public key 🔑  to docker build arguments

If building docker image in CI pipeline then pass from configuration of CI 

two ways to pass ssh keys Docker builld command

```docker
echo "export DOCKER_BUILD_EXTRAARGS='--ssh default --build-arg ssh_prv_key="$(cat ~/.ssh/id_rsa)" 
																												 --build-arg ssh_pub_key="$(cat ~/.ssh/id_rsa.pub)
```

and then call docker build as

```docker
env DOCKER_BUILDKIT=1 DOCKER_BUILD_EXTRAARGS=$DOCKER_BUILD_EXTRAARGS docker-build -f ${CONFIG}"
```

> Note:- we have used ROK8S configuration
> 

—ssh default which will pass the default ssh keys to the docker which will forward your existing SSH agent connection or a key to the builder. This enables for example to clone your private repositories during build but sometimes it doesnt so it is safe to pass them explicitly

OR pass them to the docker build

```docker
 env DOCKER_BUILDKIT=1 docker build --ssh default \
          --build-arg ssh_prv_key="$(cat ~/.ssh/id_rsa)" \
          --build-arg ssh_pub_key="$(cat ~/.ssh/id_rsa.pub)" \
```

Docker buildkit is the next generation container image builder

Add ssh keys to Docker builder as follows :-

```docker
RUN mkdir -p -m 0700 ~/.ssh && ssh-keyscan github.com >> ~/.ssh/known_hosts
ssh-keyscan(gather ssh public keys)

RUN echo "$ssh_prv_key" > /root/.ssh/id_rsa && \
    echo "$ssh_pub_key" > /root/.ssh/id_rsa.pub && \
    chmod 600 /root/.ssh/id_rsa && \
    chmod 600 /root/.ssh/id_rsa.pub
```

This command will put ssh public 🔑 and private 🔑 in your ssh config.

Now the tricky part while doing bundle install in dockerfile needs some changes 

```docker
RUN --mount=type=ssh bundle install
```

By which it will use the ssh keys we provided here and then it can access the Engine or private repository
