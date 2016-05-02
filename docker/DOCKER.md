# Docker

<!-- MarkdownTOC -->

- [Delete old containers](#delete-old-containers)
- [Delete old images](#delete-old-images)
- [Container stats](#container-stats)

<!-- /MarkdownTOC -->

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

The great thing about `docker stats` is that it is a live data stream, which means you can leave it running and monitor your per-container stats. This can be especially useful when trying to establish the impact tasks/api calls etc have on CPU/memory, and it can help you decide on the instance size requirements when deploying your application/containers into AWS ECS for example.

[Source](https://docs.docker.com/engine/reference/commandline/stats/)
