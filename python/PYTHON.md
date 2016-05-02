# Python

<!-- MarkdownTOC -->

- [Get the parent directory of a path](#get-the-parent-directory-of-a-path)
- [Run a simple Python http server](#run-a-simple-python-http-server)
    - [Python 2](#python-2)
    - [Python 3](#python-3)

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
