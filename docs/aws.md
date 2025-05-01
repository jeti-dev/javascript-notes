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

### S3
- bucket names must have a globally unique name: across regions and accounts
- although the URL to a file looks like a folder structure, it is just a key to the object in the bucket
- security:
    - user: IAM policies
    - resource: bucket policies, object and bucket access control list (ACL)
- has versioning
- replication: cross region or same region
- lifecycle rules:
    - transiton: move to a different storage class
    - expiration: delete
- events: e.g. ObjectCreated, ObjectRemoved
- pre-signed URL: e.g. to delete a premium video for some time

### CloudFron (CDN)
- origin: S3 or HTTP (EC2, ALB etc)
- caching on: headers, cookies, query strings
- security:
    - signed URL: 1 URL for a file to access
    - signed cookies: access to multiple files

### Containers
- ECS
    - EC2 launch type: I have to provision and maintain the infrastructure
    - Fargate launch type: serverless, no EC2 instances to manage
    - EFS as data volume
        - or bind mounts to share data between containers in the same task definition
    - auto scaling
    - task definition: JSON blueprint for the containers
    - task placement strategies:
        - binpack: as less EC2 as possible
        - random
        - spread: containers are even on the EC2s
- EKS: kubernetes - alternative to ECS

### Beanstalk
- managed service for EC2, ASG, ELB, RDS
- deployment options: all at once, rolling, rolling with additional batches, immutable, blue green, traffic splitting
- lifecycle policy: when to delete old versions of the app

### Cloudformation
- building blocks:
    - resources
    - parameters
    - mappings: static variables in the template
    - outputs
    - conditionals: references to what has been created
    - metadata
- on fail, rollback
- cross stacks: stacks reference each other
- nested stacks: e.g. app stack = RDS, ASG and ELB stacks
- stackset: manage stacks across accounts and regions
- stack policy: what is allowed on the resources during stack updates

### Messaging
- SQS
    - to decouple applications
    - 256kb message, 4-14 days retention
    - clients poll the que (get up to 10 messages) then delete them
    - visibility timeout: for how much time a message is invisible to other clients when 1 client polled for that message -> if not processed, the message becomes visible again
        - if a message returned to the queue X times, it is moved to the dead letter queue -> can be processed or debugged
    - messages might be out of order + might be duplicates -> we can fix these by using a FIFO queue
    - delay queue: for how much time a new message is not yet visible in the queue
    - long polling: consumers can wait a bit for messages to appear in the queue if there is no message in the queue at the moment
    - FIFO queue MessageGroupId: messages with the same MessageGroupID are always processed in order and only by 1 consumer
- SNS
    - send one message to many receivers
    - the clients subscribe to topics
    - SNS + SQS fan out: SQS queues are the subscribers of SNS topics
    - filter policy: which topic to receive
- Kinesis
    - types:
        - data stream: capture, process and store data streams
        - data firehose: load data stream into AWS data stores
        - data analytics: analyze data streams with SQL of Apache Flink
        - video streams: capture, process and store video streams
    - data streams
        - shard: a unit of streaming throughput
        - partition key: determines to which shard the data belongs
        - shard splitting: I need more throughput for a particular partition key
        - merging: make 1 shard from 2
