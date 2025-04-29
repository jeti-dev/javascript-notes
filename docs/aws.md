---
title: AWS
layout: default
---

# AWS

### IAM
- identity and access management
- users, groups
- users and groups can be assigned to JSON documents = policies which defines the permissions of the users
- IAM role: how we assign permissions to AWS services

### EC2
- virtual machine
- user data: script that runs when we first start the instance -> install updates, software etc dynamically
- AMI (amazon machine image): blueprint for creating EC2 instances -> not dynamic
- security group ~ firewall: how traffic is allowed into or out of the EC2 instances
    - only contain allow rules
    - can reference IP address or other security groups
    - all inbound traffic is blocked, all outbound is allowed by default

### EC2 instance storage
- EBS: network drive
    - persist data even if the EC2 instance was terminated
    - can be mounted only to 1 instance at a time (but has option to attach to multi)
    - use case: database
- Instance Store: faster than EBS
    - lose data if EC2 is termindated
    - use case: cache
- EFS (elastic file system): network file system
    - can be attached to many EC2 instances
    - use case: storage for web servers, CMS -> e.g. access the same media files

### Load balancers
- classic
- application: http/2, websocket, redirect, load balancing containers
    - route by path, hostname, query string
- network: TCP, UDP
- gateway: 3rd party network stuff e.g. firewalls
- sticky session: always send the client request to the same target
- connection draining: when a target is getting shut down, traffic is not sent to it for some time

### Auto scaling group
- a Target Group is registered by the ASG
- load balancers send traffic to the instances of the given Target Group
- scaling policies:
    - target: I want the average CPU to stay around 40%
    - simple/step: when a X CloudWatch alarm is triggered then add/remove Y units
    - scheduled: set min capacity to 10 on every friday at 5pm
    - predictive: based on previous statistics

### RDS
- relational database service: managed, can handle many types of databases
- auto scaling storage size
- read replicas: eventually consistent
- multi AZ: disaster recovery, sync replication
- Aurora: their own DB

### ElastiCache
- Redis or Memcached
- caching strategies:
    - lazy loading: get data from cache, if data is not found in cache then get from DB then write back to cache
    - write through: when you update the DB, you update the cache too
- cache cleaning:
    - TTL is over
    - I delete somehow
    - memory is full + data is not used recently
- MemoryDB for Redis: their own

### Route53
- record types:
    - A: hostname to IPv4
    - AAAA: hostname to IPv6
    - CNAME: hostname to hostname
    - NS: name servers for the hosted zone
    - alias: hostname to AWS resource
- hosted zones:
    - public: public records
    - private: records for within one or more VPCs
- routing policies:
    - simple
    - weighted: different routes ahs different weights -> propotions
    - failover
    - latency based: latency between AWS and the user
    - geolocation: based on user location
    - multi-value answer: to multiple resources
    - geoproximity

### VPC
- subnet: to partition the network
    - public: accessible from the internet
    - private: not accessible from the internet
- Route Tables: to define access to the internet and between subnets
- gateways:
    - internet: helps VPC connect with the internet
    - NAT (AWS managed): allow instances in private subnets to access the internet while remaining private
    - NAT instances (not gateway): the same as gateway but I manage 
- NACL (network ACL): controls traffic from and to subnets
- security groups: a firewall that controls traffic to and from an ENI or EC2 instance
- peering: connect two VPC privately using AWS' network
- endpoint: allows us to connect to AWS services using private network and not the internet
