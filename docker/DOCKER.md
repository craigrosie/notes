## Docker

#### Delete old containers

```bash
docker rm $(docker ps -q -f status=exited)
```

Composing several different hints above, the most elegant way to remove all non-running containers seems to be:

`docker rm $(docker ps -q -f status=exited)`

`-q` prints just the container ids (without column headers)

`-f` allows you to filter your list of printed containers (in this case we are filtering to only show exited containers)

[Source](http://stackoverflow.com/a/29474367/1238596)

#### Delete old images

```bash
docker rmi -f $(docker images | grep "<none>" | awk "{print \$3}")
```

This will delete all untagged images. Run the command in [Delete old containers](#Delete-old-containers) first.

[Source](https://forums.docker.com/t/command-to-remove-all-unused-images/20/5)
