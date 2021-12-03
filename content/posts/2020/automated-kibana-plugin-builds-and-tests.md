---
title: "Automated Kibana Plugin Builds and Tests"
date: 2020-01-29T00:00:00+05:30
draft: false
author: "Daniccan"
categories:
  - Kibana
tags:
  - Elasticsearch
  - Kibana
  - Plugin
  - Docker
---

## Elasticsearch and Kibana

[Elasticsearch](https://www.elastic.co/elasticsearch/) is a distributed search and analytics engine based on the [Lucene](https://lucene.apache.org/) library. It provides fast and flexible text-based searches on structured and unstructured data. Some of its major use cases include Logging & Monitoring, Big Data Analytics, Security Analytics, etc.

[Kibana](https://www.elastic.co/kibana) is the UI or presentation layer for Elasticsearch. It provides powerful visualizations for dashboards and reports along with the ability to run searches on Elasticsearch via REST calls.

Elasticsearch and Kibana are tightly-coupled during every release in such a way that both versions should match for them to run alongside each other. In simpler words, you cannot use one version of Kibana with a different version of Elasticsearch. 

The current version of [Elasticsearch](https://www.elastic.co/blog/elasticsearch-7-5-0-released) and [Kibana](https://www.elastic.co/blog/kibana-7-5-0-released) at the time of writing this post is 7.5.0.

## Kibana Plugins

Along with the set of default features that Kibana has, it also supports plugins which expands the functionalities of Kibana. 

The [installation](https://www.elastic.co/guide/en/kibana/7.x/install-plugin.html) of Kibana plugins is straight-forward with the use of a simple command.

```sh
bin/kibana-plugin install <package name or URL>
```

**Note:** In 7.5.x, it is still necessary to restart the Kibana service for the plugin to be loaded. The Elastic team is working on a [feature](https://github.com/elastic/kibana/issues/52756) to load dynamic configuration without restarts which should be available in the future releases.

Kibana supports the installation of two types of plugins.

* *Official Kibana Plugins* are developed and supported by [Elastic](https://www.elastic.co/). These plugins can be installed by mentioning the package name alone. Some of these plugins are supported only for premium subscriptions and are not available in Open Source.

* *Custom Kibana Plugins* are developed by either a business or the Open Source community. These plugins are usually installed by specifying the URL or file path of the package.

[Creating a Kibana Plugin](https://chunkbytes.com/2019/05/how-to-create-a-plugin-for-kibana/) is simple if you are familiar with developing applications using [React](https://reactjs.org/) and [Node.js](https://nodejs.org/en/).

## The Problem

Similar to how Elasticsearch and Kibana work together, the plugins are also tightly-coupled with Kibana. Every release of Kibana requires a new build and release of the plugin.

Let us assume that you have developed a plugin for Kibana version 7.0.0. There have been around 10+ releases after that till date. For every release of Kibana, you will have to release the plugin mapped to that Kibana version. 

What happens now, if you make a change to your plugin and want to check if it still works on all Kibana versions? You will have to go through the process of ensuring the versions of node.js, Elasticsearch, and Kibana in your system, bootstrapping Kibana, building the plugin, installing and testing it out. This has to be done for every version. 

Imagine having to download and install different versions of node.js, Elasticsearch and Kibana and switching between them for each build and test. This becomes a time-consuming process which decreases productivity and requires continuous manual effort.

## The Solution

What if you could build the plugin, install it, all in just one command, without worrying about installing node.js, Elasticsearch and Kibana or bootstrapping and building with the right version of Kibana? Then the only manual effort involved would be to login to Kibana UI and test if the plugin works.

This is the idea that led to the development of two docker images.

* [Kibana Plugin Builder](https://hub.docker.com/r/daniccan/kibana-plugin-builder)
* [Kibana Plugin Tester](https://hub.docker.com/r/daniccan/kibana-plugin-tester)

### Kibana Plugin Builder

The Kibana Plugin Builder can be used to build the plugin for any given version of Kibana. A simple `docker run` command with `KIBANA_VERSION` and `PLUGIN_VERSION` set as environment variables along with the source code of the plugin mounted onto the image via docker volumes is enough to build the plugin and place the package in the `build` directory of the plugin source.

This docker container handles the cloning of Kibana source from [GitHub](https://github.com/elastic/kibana), installing node.js, bootstrapping Kibana and building the plugin.

The source code for the image is available in [GitHub](https://github.com/daniccan/kibana-plugin-builder) and the docker image is available in [DockerHub](https://hub.docker.com/r/daniccan/kibana-plugin-builder).

### Kibana Plugin Tester

The Kibana Plugin Tester can be used to install and test the plugin for any given version of Kibana. This too is straight-forward with a `docker run` command expecting the `KIBANA_VERSION` and `PLUGIN_URL` (if the package is available via URL) or `PLUGIN_FILE_NAME` (if the package is available via file path) as environment variables. If the plugin package is available in a file path, it has to be mounted using docker volumes.

This docker container handles the installation and loading of Elasticsearch, Kibana and the plugin itself. Once the container is up and running, you can visit the Kibana UI in your browser to test the plugin.

The source code for the image is available in [GitHub](https://github.com/daniccan/kibana-plugin-tester) and the docker image is available in [DockerHub](https://hub.docker.com/r/daniccan/kibana-plugin-tester).
