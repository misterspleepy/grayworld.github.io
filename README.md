
## Quick Start
### Setup env
```bash
# deploy repo
git clone git@github.com:misterspleepy/grayworld.github.io.git
# build hexo image
cd env; docker build --tag hexo:latest ./
# run local blog system, and preview
docker run --name grayworld -it --rm -p 4000:4000 -v `pwd`:/workspace hexo:latest
# attach a new terminal to container, for post create
docker exec -it grayworld /bin/bash
```
### Create a new post

``` bash
$ hexo new "My New Post"
```

More info: [Writing](https://hexo.io/docs/writing.html)

### Generate static files

``` bash
$ hexo generate
```

More info: [Generating](https://hexo.io/docs/generating.html)

### Deploy to remote sites

``` bash
$ hexo deploy
```

More info: [Deployment](https://hexo.io/docs/one-command-deployment.html)

## Reference

- [hexo document in chinese](https://hexo.io/zh-cn/docs/configuration)

