# Docker

<!-- MarkdownTOC -->

- [Getting access to a bash shell in a Docker container](#getting-access-to-a-bash-shell-in-a-docker-container)
- [Delete old containers](#delete-old-containers)
- [Delete old images](#delete-old-images)
- [Container stats](#container-stats)
    - [By ID](#by-id)
    - [By name](#by-name)
- [Bash'ing straight into a container](#bashing-straight-into-a-container)
- [Network timed out while trying to connect to https://index.docker.io](#network-timed-out-while-trying-to-connect-to-httpsindexdockerio)

<!-- /MarkdownTOC -->

## Getting access to a bash shell in a Docker container

You can get access to a `bash` shell in a running `Docker` container by running the following command:

```bash
docker exec -i -t <container_id/name> bash
```

Breaking that command down:

* `docker exec` - Run a command in a running container
* `-i` - Keep STDIN open even if not attached
* `-t` - Allocate a pseudo-TTY
* `<container_id/name>` - The id or name of your container (run `docker ps` to find this)
* `bash` - The command you want to run in the `Docker` container

[Source](http://askubuntu.com/a/543057)

## Delete old containers

```bash
docker rm $(docker ps -q -f status=exited)
```

Composing several different hints above, the most elegant way to remove all non-running containers seems to be:

`docker rm $(docker ps -q -f status=exited)`

`-q` prints just the container ids (without column headers)

`-f` allows you to filter your list of printed containers (in this case we are filtering to only show exited containers)

[Source](http://stackoverflow.com/a/29474367/1238596)

## Delete old images

```bash
docker rmi -f $(docker images | grep "<none>" | awk "{print \$3}")
```

This will delete all untagged images. Run the command in [Delete old containers](#delete-old-containers) first.

[Source](https://forums.docker.com/t/command-to-remove-all-unused-images/20/5)

## Container stats

### By ID

You can see per-container stats such as CPU/MEM usage and NET/BLOCK I/O, by running:

```bash
docker stats [CONTAINER...]
```

It will present you a table a bit like the one below:

| CONTAINER | CPU % | MEM USAGE / LIMIT | MEM % | NET I/O | BLOCK I/O |
| --------- | ----- | ----------------- | ----- | ------- | --------- |
1285939c1fd3 | 0.07% | 796 KB / 64 MB | 1.21% | 788 B / 648 B | 3.568 MB / 512 KB
9c76f7834ae2 | 0.07% | 2.746 MB / 64 MB | 4.29% | 1.266 KB / 648 B | 12.4 MB / 0 B
d1ea048f04e4 | 0.03% | 4.583 MB / 64 MB | 6.30% |2.854 KB / 648 B | 27.7 MB / 0 B

[Source](https://docs.docker.com/engine/reference/commandline/stats/)

### By name

You can also pass the names of containers to `docker stats` instead of the container IDs. If you are in a situation where the container names are too long to type (i.e. they are auto-generated, but still meaningful), then you can run the following command to run `docker stats` for all running containers by container name:

```bash
docker stats $(docker ps --format "{{.Names}}")
```

and thus you will get a table like the following. Notice that it is the container name and not the container ID that is in the CONTAINER column:

| CONTAINER | CPU % | MEM USAGE / LIMIT | MEM % | NET I/O | BLOCK I/O |
| --------- | ----- | ----------------- | ----- | ------- | --------- |
front-end-container | 0.07% | 796 KB / 64 MB | 1.21% | 788 B / 648 B | 3.568 MB / 512 KB
back-end-container | 0.07% | 2.746 MB / 64 MB | 4.29% | 1.266 KB / 648 B | 12.4 MB / 0 B
logging-container | 0.03% | 4.583 MB / 64 MB | 6.30% |2.854 KB / 648 B | 27.7 MB / 0 B

The great thing about `docker stats` is that it is a live data stream, which means you can leave it running and monitor your per-container stats. This can be especially useful when trying to establish the impact tasks/api calls etc have on CPU/memory, and it can help you decide on the instance size requirements when deploying your application/containers into AWS ECS for example.

[Source](https://docs.docker.com/engine/reference/commandline/ps/#formatting)

## Bash'ing straight into a container

Normally if you want to get access to a `bash` shell in a running container, you'd need to run `docker ps`, read the convoluted output to find (part of) the relevant container ID, and then run `docker exec -it <container ID> bash`.

A better way to do this is to use the filtering functionality of `docker ps`, and the cherry on top is to put this into a `bash` function:

```bash
function dbash() {
    docker exec -it $(docker ps -q -f name=${1}) bash
}
```

[Source](https://docs.docker.com/engine/reference/commandline/ps/#/filtering)

## Network timed out while trying to connect to https://index.docker.io

This can happen when you start your `docker-machine` on one network, and then switch networks while it is still running.

Thankfully, the fix is quick. You just need to run:

```bash
docker-machine restart <docker-machine name>
eval $(docker-machine env default)
```

[Source](http://stackoverflow.com/a/32023104/1238596)

