# ESD

## Important
#### Please do not push code here.
#### Please push code to their respective repo.
#### This a consolidation of the different micro service so we can run all with docker.

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
git submodule foreach git pull origin develop

```
