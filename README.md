# grayworld

## Get started
```bash
cd env; docker build --tag hexo:latest ./
docker run --name grayworld -it --rm -p 4000:4000 -v `pwd`:/workspace hexo:latest
docker exec -it grayworld /bin/bash
```
