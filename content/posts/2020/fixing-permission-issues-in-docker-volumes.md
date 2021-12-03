---
title: "Fixing Permission Issues in Docker Volumes"
date: 2020-02-12T00:00:00+05:30
draft: false
author: "Daniccan"
categories:
  - Docker
tags:
  - Docker
  - Linux
  - Container
  - Volumes
---

## Background

I recently worked on a project which would help automate the [build](https://hub.docker.com/r/daniccan/kibana-plugin-builder) and [test](https://hub.docker.com/r/daniccan/kibana-plugin-tester) of Kibana plugins using Docker. You can read more about the project [here](../automated-kibana-plugin-builds-and-tests).

The [kibana-plugin-builder](https://hub.docker.com/r/daniccan/kibana-plugin-builder) Docker container was initially developed to run as `root` and required a volume mount to access the source code of the plugin.

## The Problem

There were no issues with the above setup until 7.1.x version of Kibana. However, in 7.2.x, there was a new constraint introduced that Kibana cannot run as `root` and required its own user. 

Here, I did what anyone else would do. I added the steps to create a new user called `ubuntu` in the `Dockerfile`, so that the container will use the `ubuntu` user instead of `root`. 

```Dockerfile
RUN useradd -ms /bin/bash ubuntu
USER ubuntu
WORKDIR /home/ubuntu
```

That resolved the constraint and Kibana was able to run successfully. But, this introduced a new problem. The `ubuntu` user did not have the required permissions to access the Docker volume, as the volume was mounted from my host machine which had a separate user.

## The Solution

### Reusing the UID of the Local User

Every user in a system has a UID assigned to it (e.g: `root` has a UID of 0). When checking for permissions, systems care about the UID and not the username. Luckily, Docker provides a way to dynamically set a UID to the user in the container. You can set a `-u` flag in the `docker run` command to pass your system user's UID to the container.

**Without -u Flag**

```sh
docker run -it --rm bash:4.4
bash-4.4$ id
uid=0(root) gid=0(root) groups=0(root),1(bin),2(daemon),3(sys),4(adm),6(disk),10(wheel),11(floppy),20(dialout),26(tape),27(video)
```

**With -u Flag**

```sh
docker run -it -u `id -u $USER` --rm bash:4.4
bash-4.4$ id
uid=501 gid=0(root)
```

But, the above solution was not enough, as the group was still pointing to `root` and there was no user with UID of 501 in the container's list of users (`cat /etc/passwd`).

### Building the Base Docker Image

The next step was to create and use a new user with the UID passed via the `-u` flag instead of just reusing the UID, so that the new user is present in a non-root group and is also available in the `/etc/passwd` file in the container.

To do this, I needed a base docker image which would do the following:
1. Create a new user with the given UID.
2. Create a new user with a random UID, if the UID is not given.
3. Assign the user to a non-root group.
4. Create a home directory for the user.
5. Execute any given process as the new user.

To `execute any given process as the new user`, the container has to switch to the new user instead of `root`. I used [gosu](https://github.com/tianon/gosu) for this. [gosu](https://github.com/tianon/gosu) is a simple tool written in [Go](https://golang.org/) to switch between users and to execute processes as the given user.

To make this base image re-usable, I created a project called [docker-gosu](https://github.com/daniccan/docker-gosu), which is a set of linux base images with [gosu](https://github.com/tianon/gosu) support to run as a non-root user.

### Using the Base Docker Image

Now, all I had to do in terms of development was change the base image in the [kibana-plugin-builder Dockerfile](https://github.com/daniccan/kibana-plugin-builder/blob/master/Dockerfile) to the [ubuntu-gosu](https://hub.docker.com/r/daniccan/ubuntu-gosu) base image that I built.

```Dockerfile
FROM daniccan/ubuntu-gosu:18.04
```

### Running the Container

The following command now runs the Docker container as a non-root user, `ubuntu`, who has the same UID as the user on my host system and is now able to access the volume mount with no issues.

```sh
docker run -it -e LOCAL_USER_ID=`id -u $USER` -v /path/in/host:/path/in/container --rm daniccan/kibana-plugin-builder
```

## Conclusion

If you are creating your Docker image from base images such as [Ubuntu](https://hub.docker.com/_/ubuntu), [CentOS](https://hub.docker.com/_/centos) or any other Linux distro and if you are not creating a user in your `Dockerfile`, it is highly likely that your container will run its processes as `root`. This is [not recommended](https://blog.trendmicro.com/trendlabs-security-intelligence/why-running-a-privileged-container-in-docker-is-a-bad-idea/) for a number of reasons. Additionally, if you are using volumes in your container, then a normal user creation will not suffice as seen above. Ensure to create your image from base images such as [docker-gosu](https://github.com/daniccan/docker-gosu) with support to create users with given UID and to run processes as a non-root user.
