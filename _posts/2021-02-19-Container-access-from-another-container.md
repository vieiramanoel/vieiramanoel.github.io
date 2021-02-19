---
title: "Reach host's localhost from inside a vscode devcontainer"
layout: post
date: 2021-02-19 03:19
headerImage: false
tag:
- vscode
- devcontainer
- docker
- localhost
- host.docker.internal
- host-gateway
star: true
image: /assets/images/extra_hosts.png
category: blog
author: vieira
description: 
---


I intend to make this text short: 

Imagine you're migrating your application to a [vscode devcontainer](https://code.visualstudio.com/docs/remote/containers) and your application consumes some local services, i.e a database `localhost:27017`. How do you access this local port from inside your dev container?

If you have one of those dev containers and you're struggling to access your development database running in another container in your machine then you've come to the right place.

<details markdown="1">
 <summary markdown="1">
  Optional: enable docker inside your dev container
 </summary>


  First of all enable to run dockers from your container: 


  In your docker image install docker, follow the appropriate tutorial for your case:


  - [Enabling docker from docker ](https://github.com/microsoft/vscode-dev-containers/tree/master/containers/docker-from-docker)(devcontainer created from a Dockerfile)
  - [Enabling docker from docker-compose ](https://github.com/microsoft/vscode-dev-containers/tree/master/containers/docker-from-docker)(devcontainer created from docker-compose)

</details>

## Enable host.docker.internal route

Once you've installed Docker into the image you want to access those containers that you used to access from localhost.

At your docker-compose.yml inside your `.devcontainer` add these lines to your service

```yaml
# your file should look like this
services:
  dev:
    # other definitions
    extra_hosts: 
      - "host.docker.internal:host-gateway"
```

This maps your `localhost` from your machine host to a name `host.docker.internal`. Now substitute all occurrences of `localhost` to `host.docker.internal` and it's done!

So if you've been connecting to your database using `localhost:27017` as the conn URL, you'll need to change it to `host.docker.internal:27071`, for example.