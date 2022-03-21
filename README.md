# esd

Submodule Configuration
------------
#### Getting the git submodules ready for NSI
[Pulling all submodules](https://stackoverflow.com/questions/1030169/easy-way-to-pull-latest-of-all-git-submodules)

First-time check ins
```shell
git submodule update --recursive --init
```

Subsequent check ins
```shell
git submodule update --recursive --remote
```

Submodule repository has changed
```shell
git submodule sync --recursive
```
