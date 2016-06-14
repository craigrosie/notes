# Python

<!-- MarkdownTOC -->

- [Get the parent directory of a path](#get-the-parent-directory-of-a-path)
- [Run a simple Python http server](#run-a-simple-python-http-server)
    - [Python 2](#python-2)
    - [Python 3](#python-3)
- [Requests being slow](#requests-being-slow)

<!-- /MarkdownTOC -->

## Get the parent directory of a path

```python
import os
print os.path.abspath(os.path.join(yourpath, os.pardir))
```

where `yourpath` is the path you want the parent for.

[Source](http://stackoverflow.com/a/2860193/1238596)

## Run a simple Python http server

### Python 2

```bash
python -m SimpleHTTPServer
```

### Python 3

```bash
python -m http.server
```

[Source](http://stackoverflow.com/a/7943768/1238596)

## Requests being slow

Do you notice a significant slowdown when doing multiple requests (to the same host!) using the `requests` library compared to making the same requests in the browser/Postman/etc?

Whenever you execute `requests.get()`, `requests` actually creates a `session` object behind the scenes. I have personally witnessed this overhead slowing down code by around x10 compared to making the requests using Postman.

The solution is to create your own `requests.session` object, which can then be re-used across multiple requests. If you are hitting the same host, it will re-use the underlying TCP connection.

Old, slow version using plain `requests.get`:

```python
import requests

for x in range(10):
    # Has to create a new session for each .get()
    requests.get('test.com?id={}'.format(x))
```

New, fast version using `requests.session`:

```python
import requests

ses = requests.session()

for x in range(10):
    # Re-uses the same session object for each .get()
    ses.get('test.com?id={}'.format(x))
```

[Source](http://docs.python-requests.org/en/master/user/advanced/)
