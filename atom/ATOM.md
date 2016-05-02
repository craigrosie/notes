# Atom

<!-- MarkdownTOC -->

- [Create package list](#create-package-list)
- [Install package list](#install-package-list)

<!-- /MarkdownTOC -->

## Create package list

```bash
apm list --installed --bare > package-list.txt
```

## Install package list

```bash
apm install --packages-file package-list.txt
```

[Source](https://discuss.atom.io/t/installed-packages-list-into-single-file/12227/2)
