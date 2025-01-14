# What is Elasticsearch?

> Elasticsearch is a highly scalable open-source full-text search and analytics engine. It allows you to store, search, and analyze big volumes of data quickly and in near real time

[elastic.co/products/elasticsearch](https://www.elastic.co/products/elasticsearch)

# TL;DR

```bash
$ docker run --name elasticsearch bitnami/elasticsearch:latest
```

## Docker Compose

```bash
$ curl -sSL https://raw.githubusercontent.com/bitnami/bitnami-docker-elasticsearch/master/docker-compose.yml > docker-compose.yml
$ docker-compose up -d
```

# Why use Bitnami Images?

* Bitnami closely tracks upstream source changes and promptly publishes new versions of this image using our automated systems.
* With Bitnami images the latest bug fixes and features are available as soon as possible.
* Bitnami containers, virtual machines and cloud images use the same components and configuration approach - making it easy to switch between formats based on your project needs.
* All our images are based on [minideb](https://github.com/bitnami/minideb) a minimalist Debian based container image which gives you a small base container image and the familiarity of a leading linux distribution.
* All Bitnami images available in Docker Hub are signed with [Docker Content Trust (DTC)](https://docs.docker.com/engine/security/trust/content_trust/). You can use `DOCKER_CONTENT_TRUST=1` to verify the integrity of the images.
* Bitnami container images are released daily with the latest distribution packages available.


> This [CVE scan report](https://quay.io/repository/bitnami/elasticsearch?tab=tags) contains a security report with all open CVEs. To get the list of actionable security issues, find the "latest" tag, click the vulnerability report link under the corresponding "Security scan" field and then select the "Only show fixable" filter on the next page.

# How to deploy Elasticsearch in Kubernetes?

Deploying Bitnami applications as Helm Charts is the easiest way to get started with our applications on Kubernetes. Read more about the installation in the [Bitnami Elasticsearch Chart GitHub repository](https://github.com/bitnami/charts/tree/master/bitnami/elasticsearch).

Bitnami containers can be used with [Kubeapps](https://kubeapps.com/) for deployment and management of Helm Charts in clusters.

# Why use a non-root container?

Non-root container images add an extra layer of security and are generally recommended for production environments. However, because they run as a non-root user, privileged tasks are typically off-limits. Learn more about non-root containers [in our docs](https://docs.bitnami.com/containers/how-to/work-with-non-root-containers/).

# Supported tags and respective `Dockerfile` links

> NOTE: Debian 8 images have been deprecated in favor of Debian 9 images. Bitnami will not longer publish new Docker images based on Debian 8.

Learn more about the Bitnami tagging policy and the difference between rolling tags and immutable tags [in our documentation page](https://docs.bitnami.com/containers/how-to/understand-rolling-tags-containers/).


* [`7-ol-7`, `7.3.0-ol-7-r22` (7/ol-7/Dockerfile)](https://github.com/bitnami/bitnami-docker-elasticsearch/blob/7.3.0-ol-7-r22/7/ol-7/Dockerfile)
* [`7-debian-9`, `7.3.0-debian-9-r7`, `7`, `7.3.0`, `7.3.0-r7`, `latest` (7/debian-9/Dockerfile)](https://github.com/bitnami/bitnami-docker-elasticsearch/blob/7.3.0-debian-9-r7/7/debian-9/Dockerfile)
* [`6-ol-7`, `6.8.2-ol-7-r23` (6/ol-7/Dockerfile)](https://github.com/bitnami/bitnami-docker-elasticsearch/blob/6.8.2-ol-7-r23/6/ol-7/Dockerfile)
* [`6-debian-9`, `6.8.2-debian-9-r23`, `6`, `6.8.2`, `6.8.2-r23` (6/debian-9/Dockerfile)](https://github.com/bitnami/bitnami-docker-elasticsearch/blob/6.8.2-debian-9-r23/6/debian-9/Dockerfile)

Subscribe to project updates by watching the [bitnami/elasticsearch GitHub repo](https://github.com/bitnami/bitnami-docker-elasticsearch).
# Get this image

The recommended way to get the Bitnami Elasticsearch Docker Image is to pull the prebuilt image from the [Docker Hub Registry](https://hub.docker.com/r/bitnami/elasticsearch).

```bash
$ docker pull bitnami/elasticsearch:latest
```

To use a specific version, you can pull a versioned tag. You can view the [list of available versions](https://hub.docker.com/r/bitnami/elasticsearch/tags/) in the Docker Hub Registry.

```bash
$ docker pull bitnami/elasticsearch:[TAG]
```

If you wish, you can also build the image yourself.

```bash
$ docker build -t bitnami/elasticsearch:latest 'https://github.com/bitnami/bitnami-docker-elasticsearch.git#master:7/debian-9'
```

# Persisting your application

If you remove the container all your data will be lost, and the next time you run the image the application will be reinitialized. To avoid this loss of data, you should mount a volume that will persist even after the container is removed.

For persistence you should mount a directory at the `/bitnami` path. If the mounted directory is empty, it will be initialized on the first run.

```bash
$ docker run \
    -v /path/to/elasticsearch-data-persistence:/bitnami/elasticsearch/data \
    bitnami/elasticsearch:latest
```

or by making a minor change to the [`docker-compose.yml`](https://github.com/bitnami/bitnami-docker-elasticsearch/blob/master/docker-compose.yml) file present in this repository:

```yaml
mariadb:
  ...
  volumes:
    - /path/to/elasticsearch-data-persistence:/bitnami/elasticsearch/data
  ...
```

# Connecting to other containers

Using [Docker container networking](https://docs.docker.com/engine/userguide/networking/), a Elasticsearch server running inside a container can easily be accessed by your application containers.

Containers attached to the same network can communicate with each other using the container name as the hostname.

## Using the Command Line

### Step 1: Create a network

```bash
$ docker network create app-tier --driver bridge
```

### Step 2: Launch the Elasticsearch server instance

Use the `--network app-tier` argument to the `docker run` command to attach the Elasticsearch container to the `app-tier` network.

```bash
$ docker run -d --name elasticsearch-server \
    --network app-tier \
    bitnami/elasticsearch:latest
```

### Step 3: Launch your application container

```bash
$ docker run -d --name myapp \
    --network app-tier \
    YOUR_APPLICATION_IMAGE
```

> **IMPORTANT**:
>
> 1. Please update the **YOUR_APPLICATION_IMAGE_** placeholder in the above snippet with your application image
> 2. In your application container, use the hostname `elasticsearch-server` to connect to the Elasticsearch server

## Using Docker Compose

When not specified, Docker Compose automatically sets up a new network and attaches all deployed services to that network. However, we will explicitly define a new `bridge` network named `app-tier`. In this example we assume that you want to connect to the Elasticsearch server from your own custom application image which is identified in the following snippet by the service name `myapp`.

```yaml
version: '2'

networks:
  app-tier:
    driver: bridge

services:
  elasticsearch:
    image: 'bitnami/elasticsearch:latest'
    networks:
      - app-tier
  myapp:
    image: 'YOUR_APPLICATION_IMAGE'
    networks:
      - app-tier
```

> **IMPORTANT**:
>
> 1. Please update the **YOUR_APPLICATION_IMAGE_** placeholder in the above snippet with your application image
> 2. In your application container, use the hostname `elasticsearch` to connect to the Elasticsearch server

Launch the containers using:

```bash
$ docker-compose up -d
```

# Configuration

## Environment variables

When you start the elasticsearch image, you can adjust the configuration of the instance by passing one or more environment variables either on the docker-compose file or on the docker run command line. If you want to add a new environment variable:

 * For Docker Compose, add the variable name and value under the application section:

```yaml
elasticsearch:
  ...
  environment:
    - ELASTICSEARCH_PORT_NUMBER=9201
  ...
```

 * For manual execution add a `-e` option with each variable and value:

```bash
 $ docker run -d --name elasticsearch \
    -p 9201:9201 --network=elasticsearch_network \
    -e ELASTICSEARCH_PORT_NUMBER=9201 \
    -v /path/to/elasticsearch-data-persistence:/bitnami/elasticsearch/data \
    bitnami/elasticsearch
```

Available variables:

 - `BITNAMI_DEBUG`: Increase verbosity on initialization logs. Default **false**
 - `ELASTICSEARCH_CLUSTER_NAME`: The Elasticsearch Cluster Name. Default: **elasticsearch-cluster**
 - `ELASTICSEARCH_CLUSTER_HOSTS`: List of elasticsearch hosts to set the cluster. Available separators are ' ', ',' and ';'. No defaults.
 - `ELASTICSEARCH_CLUSTER_MASTER_HOSTS`: List of elasticsearch master-eligible hosts. Available separators are ' ', ',' and ';'. If no values are provided, it will have the same value than `ELASTICSEARCH_CLUSTER_HOSTS`.
 - `ELASTICSEARCH_IS_DEDICATED_NODE`: Elasticsearch node to behave as a 'dedicated node'. Default: **no**
 - `ELASTICSEARCH_NODE_TYPE`: Elasticsearch node type when behaving as a 'dedicated node'. Valid values: *master*, *data*, *coordinating* or *ingest*.
 - `ELASTICSEARCH_NODE_NAME`: Elasticsearch node name. No defaults.
 - `ELASTICSEARCH_BIND_ADDRESS`: Address/interface to bind by Elasticsearch. Default: **0.0.0.0**
 - `ELASTICSEARCH_PORT_NUMBER`: Elasticsearch port. Default: **9200**
 - `ELASTICSEARCH_NODE_PORT_NUMBER`: Elasticsearch Node to Node port. Default: **9300**
 - `ELASTICSEARCH_PLUGINS`: Comma, semi-colon or space separated list of plugins to install at initialization. No defaults.
 - `ELASTICSEARCH_HEAP_SIZE`: Memory used for the Xmx and Xms java heap values. Default: **1024m**

## Setting up a cluster

A cluster can easily be setup with the Bitnami Elasticsearch Docker Image using the following environment variables:

 - `ELASTICSEARCH_CLUSTER_NAME`: The Elasticsearch Cluster Name. Default: **elasticsearch-cluster**
 - `ELASTICSEARCH_CLUSTER_HOSTS`: List of elasticsearch hosts to set the cluster. Available separators are ' ', ',' and ';'. No defaults.
 - `ELASTICSEARCH_CLIENT_NODE`: Elasticsearch node to behave as a 'smart router' for Kibana app. Default: **false**
 - `ELASTICSEARCH_NODE_NAME`: Elasticsearch node name. No defaults.
 - `ELASTICSEARCH_MINIMUM_MASTER_NODES`: Minimum Elasticsearch master nodes for quorum. No defaults.

For larger cluster, you can setup 'dedicated nodes' using the following environment variables:

 - `ELASTICSEARCH_IS_DEDICATED_NODE`: Elasticsearch node to behave as a 'dedicated node'. Default: **no**
 - `ELASTICSEARCH_NODE_TYPE`: Elasticsearch node type when behaving as a 'dedicated node'. Valid values: *master*, *data*, *coordinating* or *ingest*.
 - `ELASTICSEARCH_CLUSTER_MASTER_HOSTS`: List of elasticsearch master-eligible hosts. Available separators are ' ', ',' and ';'. If no values are provided, it will have the same value than `ELASTICSEARCH_CLUSTER_HOSTS`.

Find more information about 'dedicated nodes' in the [official documentation](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-node.html).

### Step 1: Create a new network.

```bash
$ docker network create elasticsearch_network
```

### Step 2: Create a first node.

```bash
$ docker run --name elasticsearch-node1 \
  --net=elasticsearch_network \
  -p 9200:9200 \
  -e ELASTICSEARCH_CLUSTER_NAME=elasticsearch-cluster \
  -e ELASTICSEARCH_CLUSTER_HOSTS=elasticsearch-node1,elasticsearch-node2 \
  -e ELASTICSEARCH_NODE_NAME=elastic-node1 \
  bitnami/elasticsearch:latest
```

In the above command the container is added to a cluster named `elasticsearch-cluster` using the `ELASTICSEARCH_CLUSTER_NAME`. The `ELASTICSEARCH_CLUSTER_HOSTS` parameter set the name of the nodes that set the cluster so we will need to launch other container for the second node. Finally the `ELASTICSEARCH_NODE_NAME` parameter allows to indicate a known name for the node, otherwise elasticsearch will generate a randon one.

### Step 3: Create a second node

```bash
$ docker run --name elasticsearch-node2 \
  --link elasticsearch-node1:elasticsearch-node1 \
  --net=elasticsearch_network \
  -e ELASTICSEARCH_CLUSTER_NAME=elasticsearch-cluster \
  -e ELASTICSEARCH_CLUSTER_HOSTS=elasticsearch-node1,elasticsearch-node2 \
  -e ELASTICSEARCH_NODE_NAME=elastic-node2 \
  bitnami/elasticsearch:latest
```

In the above command a new elasticsearch node is being added to the elasticsearch cluster indicated by `ELASTICSEARCH_CLUSTER_NAME`.

You now have a two node Elasticsearch cluster up and running which can be scaled by adding/removing nodes.

With Docker Compose the cluster configuration can be setup using:

```yaml
version: '2'
services:
  elasticsearch-node1:
    image: bitnami/elasticsearch:latest
    environment:
      - ELASTICSEARCH_CLUSTER_NAME=elasticsearch-cluster
      - ELASTICSEARCH_CLUSTER_HOSTS=elasticsearch-node1,elasticsearch-node2
      - ELASTICSEARCH_NODE_NAME=elastic-node1

  elasticsearch-node2:
    image: bitnami/elasticsearch:latest
    environment:
      - ELASTICSEARCH_CLUSTER_NAME=elasticsearch-cluster
      - ELASTICSEARCH_CLUSTER_HOSTS=elasticsearch-node1,elasticsearch-node2
      - ELASTICSEARCH_NODE_NAME=elastic-node2
```

## Configuration file

In order to use a custom configuration file instead of the default one provided out of the box, you can create a file named `elasticsearch.yml` and mount it at `/opt/bitnami/elasticsearch/config/elasticsearch.yml` to overwrite the default configuration.
Please, note that the whole configuration file will be replaced by the provided one, ensure that the syntax and fields are properly set.

```bash
$ docker run -d --name elasticsearch \
    -p 9201:9201 \
    -v /path/to/elasticsearch.yml:/opt/bitnami/elasticsearch/config/elasticsearch.yml \
    -v /path/to/elasticsearch-data-persistence:/bitnami/elasticsearch/data \
    bitnami/elasticsearch:latest
```

or by changing the [`docker-compose.yml`](https://github.com/bitnami/bitnami-docker-elasticsearch/blob/master/docker-compose.yml) file present in this repository:

```yaml
elasticsearch:
  ...
  volumes:
    - /path/to/elasticsearch.yml:/opt/bitnami/elasticsearch/config/elasticsearch.yml
    - /path/to/elasticsearch-data-persistence:/bitnami/elasticsearch/data
  ...
```

# Logging

The Bitnami Elasticsearch Docker image sends the container logs to the `stdout`. To view the logs:

```bash
$ docker logs elasticsearch
```

or using Docker Compose:

```bash
$ docker-compose logs elasticsearch
```

You can configure the containers [logging driver](https://docs.docker.com/engine/admin/logging/overview/) using the `--log-driver` option if you wish to consume the container logs differently. In the default configuration docker uses the `json-file` driver.

# Maintenance

## Upgrade this image

Bitnami provides up-to-date versions of Elasticsearch, including security patches, soon after they are made upstream. We recommend that you follow these steps to upgrade your container.

### Step 1: Get the updated image

```bash
$ docker pull bitnami/elasticsearch:latest
```

or if you're using Docker Compose, update the value of the image property to
`bitnami/elasticsearch:latest`.

### Step 2: Stop and backup the currently running container

Stop the currently running container using the command

```bash
$ docker stop elasticsearch
```

or using Docker Compose:

```bash
$ docker-compose stop elasticsearch
```

Next, take a snapshot of the persistent volume `/path/to/elasticsearch-data-persistence` using:

```bash
$ rsync -a /path/to/elasticsearch-data-persistence /path/to/elasticsearch-data-persistence.bkp.$(date +%Y%m%d-%H.%M.%S)
```

You can use this snapshot to restore the application state should the upgrade fail.

### Step 3: Remove the currently running container

```bash
$ docker rm -v elasticsearch
```

or using Docker Compose:

```bash
$ docker-compose rm -v elasticsearch
```

### Step 4: Run the new image

Re-create your container from the new image, [restoring your backup](#restoring-a-backup) if necessary.

```bash
$ docker run --name elasticsearch bitnami/elasticsearch:latest
```

or using Docker Compose:

```bash
$ docker-compose up elasticsearch
```

# Notable Changes

## 6.6.1-debian-9-r12, 6.6.1-ol-7-r13, 6.6.1-rhel-7-r13, 5.6.15-debian-9-r12 and 5.6.15-ol-7-r13

- Deprecate the use of `elasticsearch_custom.yml` in favor of replacing the whole `elasticsearch.yml` file.

## 6.4.0-debian-9-r19, 6.4.0-ol-7-r18, 5.6.4-debian-9-r54, and 5.6.4-ol-7-r60

- Decrease the size of the container. It is not necessary Node.js anymore. Elasticsearch configuration moved to bash scripts in the `rootfs/` folder.
- The recommended mount point to persist data changes to `/bitnami/elasticsearch/data`.
- The Elasticsearch configuration files are not persisted in a volume anymore. Now, they can be found at `/opt/bitnami/elasticsearch/config`.
- Elasticsearch `plugins` and `modules` are not persisted anymore. It's necessary to indicate what plugins to install using the env. variable `ELASTICSEARCH_PLUGINS`
- Backwards compatibility is not guaranteed when data is persisted using docker-compose. You can use the workaround below to overcome it:

```bash
docker-compose down
# Change the mount point
sed -i -e 's#elasticsearch_data:/bitnami#elasticsearch_data:/bitnami/elasticsearch/data#g' docker-compose.yml
# Pull the latest bitnami/elasticsearch image
docker pull bitnami/elasticsearch:latest
docker-compose up -d
```

## 6.2.3-r7 & 5.6.4-r18

- The Elasticsearch container has been migrated to a non-root user approach. Previously the container ran as the `root` user and the Elasticsearch daemon was started as the `elasticsearch` user. From now on, both the container and the Elasticsearch daemon run as user `1001`. As a consequence, the data directory must be writable by that user. You can revert this behavior by changing `USER 1001` to `USER root` in the Dockerfile.

## 6.2.3-r2 & 5.6.4-r6

- Elasticsearch container can be configured as a dedicated node with 4 different types: *master*, *data*, *coordinating* or *ingest*.
  Previously it was only achievable by using a custom `elasticsearch_custom.yml` file. From now on, you can use the environment variables `ELASTICSEARCH_IS_DEDICATED_NODE` & `ELASTICSEARCH_NODE_TYPE` to configure it.

# Contributing

We'd love for you to contribute to this container. You can request new features by creating an [issue](https://github.com/bitnami/bitnami-docker-elasticsearch/issues), or submit a [pull request](https://github.com/bitnami/bitnami-docker-elasticsearch/pulls) with your contribution.

# Issues

If you encountered a problem running this container, you can file an [issue](https://github.com/bitnami/bitnami-docker-elasticsearch/issues). For us to provide better support, be sure to include the following information in your issue:

- Host OS and version
- Docker version (`docker version`)
- Output of `docker info`
- Version of this container (`echo $BITNAMI_IMAGE_VERSION` inside the container)
- The command you used to run the container, and any relevant output you saw (masking any sensitive information)

# License
Copyright 2016-2019 Bitnami

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
