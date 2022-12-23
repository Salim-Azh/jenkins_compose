# Jenkins Compose

This template provides fully setup Jenkins environment for testing [Jenkins Pipeline Data Extractor Plugin](https://gitlab.com/azharhoussen.salim/pipeline-data-extractor-plugin).

Based on the official Docker images from Jenkins:

* [jenkins:2.176.3-jdk11](https://hub.docker.com/layers/jenkins/jenkins/2.176.3-jdk11/images/sha256-8b2fde0554da35eab7817d3d1eec183ec0c3cc7a14870f51e5f5a50fd992cb51?context=explore)

* [ssh-agent:jdk11](https://hub.docker.com/layers/jenkins/ssh-agent/jdk11/images/sha256-7f36437b3347a6afddfc926e97afc203b815ca6a4a4bda08d81ecdd074255523?context=explore)

## Requirements

### Host setup

* [Docker Engine](https://docs.docker.com/get-docker/)
* [Docker Compose](https://docs.docker.com/compose/install/)

By default, Jenkins Compose exposes the port **8082** to Jenkins.

## Specifications

This compose stack contains two services :

### jenkins

A Jenkins controller based on Jenkins 2.176.3 and JDK11.

### agent

A Jenkins SSH agent to connect over SSH to the Jenkins controller.

### How it works

During the Jenkins Compose image building phase all the plugins specified inside the `/docker/jenkins/plugins.txt` will be installed on the Jenkins instance with the help of [Plugin Installation Manager Tool for Jenkins](https://github.com/jenkinsci/plugin-installation-manager-tool).
If you add a pipeline-data-extractor hpi file to `/docker/jenkins/` then the [Pipeline Data Extractor Plugin](#) plugin will also be installed.

At Jenkins Compose start up the SSH public key inside `.env` will be passed as an environment variable to the agent service.
The jenkins service will start as would a standard new Jenkins instance. See the logs to get the secret head to your web browser at [http://localhost:8082](http://localhost:8082). Skip the plugin installation phase by selecting none to install. Setup your SSH agent and start using Jenkins.

## Getting Started

1. Download the latest jenkins-plugin-manager jar [from here](https://github.com/jenkinsci/plugin-installation-manager-tool/releases/latest). And move the jar inside `./docker/jenkins`

    > **Note**
    > See full documentation for [Plugin Installation Manager Tool for Jenkins](https://github.com/jenkinsci/plugin-installation-manager-tool).

2. Place your `pipeline-data-extractor.hpi` inside `./docker/jenkins`

3. Set the `JENKINS_AGENT_SSH_PUBKEY` environment variable inside the `.env` file with your SSH pub key.
    > **Note**
    > The private key will be required to connect with the SSH agent in Jenkins.
    >
    >To create a new authentication key pairs for SSH use `ssh-keygen`

4. Uncomment the first line inside the `.gitignore` file.

## Usage

> **Warning**
> If you modify anything inside `/docker/jenkins/` or `docker/agent/`, remember to rebuild all container images using the `docker compose build` command.

### Start Jenkins Compose

1. Start Jenkins Compose services

    ```sh
    docker compose up -d
    ```

2. Open your web browser at [http://localhost:8082](http://localhost:8082)

### Clean up

```sh
docker compose down
```

### Setup the SSH agent on Jenkins controller

1. Add new global credentials
![Credendials form](../media/credentials-config.jpg?raw=true)

2. Create new node
![Node configuration form](../media/node-config.jpg?raw=true)

