---
title: Deploying Docker Containers to Amazon AWS using the EC2 Container Service
tags: [docker, aws, techops, devops, mike-klass]
categories: []
name: [Mike Klass]
author: [mike-klass]
snippet: [I have been looking into ways to deploy code using Docker easily and efficiently within AWS, and found that there is a new service called EC2 Container Service (ECS) which allows you to do so pretty easily. I thought I would write my second blog post about my findings with ECS.]
---

### Intro
I have been looking into ways to deploy code using Docker easily and efficiently within AWS, and found that there is a new service called EC2 Container Service (ECS) which allows you to do so pretty easily. I thought I would write my second blog post about my findings with ECS.

### What is ECS
ECS is a way to create EC2 instances for Docker-based applications on the fly without any additional setup. This allows you to scale horizontally without having to manually spin up EC2 instances to deploy Docker containers to. AWS also helps you create Docker clusters, which are able to be scaled automatically as CPU and RAM usage changes across all of the ECS instances in the cluster, rather than having to monitor each EC2 instance separately. It is somewhat exciting to learn that AWS has a set of brand new monitoring tools for clusters, which would help us with monitoring our applications' resource utilization mofing forward.

### What is Docker
I was writing the description of ECS and thought as I was writing... it would probably help if I described what Docker is and could possibly help us with in our situation so that someone who wasn't experienced using Docker could follow along with the post. You can think of docker like a container. It runs on top of a common host OS, and multiple containers can run on the same server sharing that same host OS. The containers are completely separate from each other, but share the same OS and certain bins and libraries which are common among all of the containers. Since the containers are completely separate, they will not interfere with each other, and it makes them completely portable... you can move a container to a new server as long as the host OS contains the required bins and libraries. The Docker Registry handles the requirements for you.

### How does Docker fit in with ECS
ECS allows you to deploy Docker containers to new instances automatically. Instances are created in a cluster, which acts like a common physical hardware server. The containers are then deployed within that cluster. A cluster has a backbone of one or more EC2 instances, based on how you set up the cluster. So if you have only one container, you could theoretically have many EC2 instances which that container shares resources with... or many containers using one EC2 instance of adequate size. The choice is yours.

### Scalability
EC2 instances can be created in a cluster on the fly, or via logic. So if you have, for instance, 4 Docker containers inside a cluster, and only one EC2 instance that powers those 4 containers... if one or more of the containers requests a sufficient amount of CPU to peg (100%) your only EC2 instance, you will have service interruption on the other containers. You can then manually add an EC2 instance to the cluster to distribute the load... so if you were at 90% CPU on one EC2 instance, you could launch another one in that cluster to bring it down to 45% each. Manually doing things sucks though, so what can we do to make this automatic? You can write scripts which automatically create or destroy EC2 instances as the load changes. So for instance if one EC2 instance has 100% CPU, another can be launched via script to bring it down to 50% each. If load increases further, more can be launched. If load decreses, instances can be destroyed. You can set an upper limit and which type of instances are launched, and what the base EC2 unit is... to make it less expensive than having a single large EC2 instance at 1% CPU during lull periods.

Here is an example of a cluster:
NEED way to embed imgs in repo. 
https://github.com/sculpin/sculpin/issues/118

### Monitoring
Having many instances inside a cluster can create a monitoring nightmare. Luckily AWS thought of that and made things easier with CloudWatch. Clustered EC2 instances can be monitored together, so that each cluster and child EC2 instances reports aggregate cluster load and RAM usage as well as singular instance load and RAM usage. This way you can see at a glance if a specific EC2 server is pegged, or if the entire cluster is pegged... and figure out how to fix it (either log into the stuck EC2 instance and figure out why it's stuck, reboot it, or start adding more servers if the aggregate load or RAM is pegged).

### Where Do We Go From Here
I think this technology could be incredibly useful for us from an architectural standpoint. New applications could be deployed as docker containers, and logic for the autoscale can be added to provide enough aggregate power for our entire future infrastructure. If we have time, I would like to explore this technology further and come up with a way to utilize it for DietBet and any additional applications we roll forward with.