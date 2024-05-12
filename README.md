# k8s 101

> [!NOTE]
> For reading the notes go to the [docs](./docs) dir or to the built site (TODO)

Docs managed with [Material for Mkdocs](https://squidfunk.github.io/mkdocs-material/)

* getting starter: https://squidfunk.github.io/mkdocs-material/getting-started/

## Run docs site locally

```shell
mkdocs serve
```

## Set up

```shell
# create environment
python3.12 -m venv venv
# install mkdocs-material
python3.12 -m pip install mkdocs-material
```

[![Built with Material for MkDocs](https://img.shields.io/badge/Material_for_MkDocs-526CFE?style=for-the-badge&logo=MaterialForMkDocs&logoColor=white)](https://squidfunk.github.io/mkdocs-material/)

## TODO

* [x] add `index` pages per each dir to improve experience while navigating on the repo instead of on the site. Those
  index should not interfere with the mkdoc table of content
* [ ] move code snipped in md files to independent files
* [ ] order content menu
* [x] publish site -> https://jcabrerizo.github.io/k8s101/
* [x] action to publish the site when merged / commit on main -> https://github.com/jcabrerizo/k8s101/blob/main/.github/workflows/build-and-publish-mkdocs.yaml
* [ ] git hook for format the md files
