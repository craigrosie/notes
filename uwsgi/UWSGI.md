# uwsgi

<!-- MarkdownTOC -->

- [Sane defaults](#sane-defaults)
- [Diagnosing uwsgi seg faults](#diagnosing-uwsgi-seg-faults)
    - [Check function return values](#check-function-return-values)
    - [Check uwsgi has enough resources to run](#check-uwsgi-has-enough-resources-to-run)

<!-- /MarkdownTOC -->

## Sane defaults

`uwsgi` has more options that you can shake a stick at. I did a bit of googling to try and come up with a sensible `uwsig.ini` file, and this is what I'm working with at the moment. (**NOTE:** I use Flask as my framework of choice).

```ini
[uwsgi]
http-socket = :9090
module = <path.to.app>
callable = app
master = true

workers = 5
reaper
enable-threads = true
catch-exceptions = true
ignore-sigpip= true
socket-timeout = 120
reload-mercy = 120
vacuum = true
no-orphans = true
pidfile = /tmp/uwsgi.pid
buffer-size = 65535
memory-report
```

## Diagnosing uwsgi seg faults

In my short experience with it, `uwsgi` can seg fault for a number of reasons.

### Check function return values

If your application returns something uwsgi cannot handle, it can seg fault. For example, I've come across an issue where a function in a `Flask` app was producing an error, and therefore was returning `None` when `uwsgi` was expecting a response. Try adding a `try except` block around the code in your function and work out what the function is actually returning.

### Check uwsgi has enough resources to run

Make sure wherever you are running `uwsgi` has enough resources (and specifically memory) to run. I discovered this issue when running a `Flask` app behind `uwsgi` in a `Docker` container deployed in AWS ECS. The `Docker` container was only allocated 64mb of RAM, and after the first request to the service, memory utilisation was already at 98%. A second request caused `uwsgi` to fall over, the worker would die and respawn, the next request would work, and the cycle would continue. Bumping the `Docker` container to have 128mb of allocated RAM completely fixed the issue.
