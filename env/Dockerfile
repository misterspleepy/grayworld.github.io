FROM node:latest
RUN npm install hexo-cli -g
RUN npm install hexo-browsersync --save -g
CMD cd /workspace/blog && npm install && hexo server