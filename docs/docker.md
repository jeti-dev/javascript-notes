---
title: Docker
layout: default
---

# Docker
[Notes of this video](https://www.youtube.com/watch?v=RqTEHSBrYFw&t=165s)

- container image: like a class
- container: like the object from a class
    - containers share the kernel with the host system
- 3 linux building blocks
    - namespace: make a part of a global resource appear to a process that the process thinks it owns the whole resource
        - e.g. using the User namespace the user of the container can be an admin in the container BUT on the host system that user is mapped to a non-admin user
    - cgroup: organize processess to hierarchical groups -> the resources these groups use can then be limited and monitored e.g. 30% CPU for group1 and 20% for group2
    - union mount filesystem (overlayfs): virtual file system: if there are files at multiple levels, they can appear as they were in on the same level -> images can share stuff
- keeping data:
    - by default, everything is cleared on shutdown
    - dependency (we know the data at build time): build into the image *1
    - data generated: use a Volume to store it outside of the container -> will be stored on the virtual machine docker is running (volume mount, preferred)
        - bind mount: bind the data on the host system to the container
- *1 container filesyetem: if you install a linux package and not delete the container after stopping it, the package will be present in the container the next time you start it: but we don't want to do that so we have to build the dependency into the image
- dockerfile: what and how to build
    - build context: other files included in the build
    - .dockerignore