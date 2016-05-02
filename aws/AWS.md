## AWS

### ECS - EC2 Container Service

#### Determining available storage for Docker

You may run into issues on ECS if the `docker-pool` becomes full. This can happen after multiple deployments (i.e. pulling lots of different Docker images), or if you have exceptionally large Docker images.

To check the remaining free space in your `docker-pool`, `ssh` on the EC2 instance(s) in your ECS Cluster and run:

```bash
sudo lvs
```

You will see output similar to this:

![ecs_sudo_lvs](https://cloud.githubusercontent.com/assets/6367914/14952091/59dc34fc-1055-11e6-9c0b-ea8f26bd8207.png)

It is the date column that is of interest - it shows how full the `docker-pool` is.

This if often a good thing to check if you start to experience strange issues, such as deployments not working, or the EC2 instance being unable to pull new images.

If you are lucky, you might be able to try executing the commands to [delete old containers](docker/DOCKER.md#Delete-old-containers) and/or [delete old images](docker/DOCKER.md#Delete-old-images).

If you are not so lucky, you may have to terminate the EC2 instance and let the ASG bring up a replacement. I have personally had to do this on a few occasions, as the `docker-pool` being full seems to causes Docker commands to hang - i.e. `docker ps` wouldn't even complete.

[Source](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs-optimized_AMI.html)
