---
title: 利用Hexo和github pages搭建博客网站
date: 2023-10-15 10:12:08
tags:
---
## 基本原理
### hexo介绍
hexo是一个静态网站生成工具

### github pages
支持将github的一个仓库中的指定分支作为网页访问，提供域名

## 特点
- 公共域名，任何人均可访问
- 版本控制
- 自动化部署
- 本地开发方便

## 步骤
### Github仓库配置
- 新建或重命名仓库名，以github.io结尾
### 本地环境搭建
- 安装docker
- 将代码仓从github clone到本地目录
- 本地执行NODE容器，安装hexo
- 参考hexo相关文档，执行hexo init创建网站
- 执行hexo server，并本地查看网站效果
- 将修改commit，push到远端master分支
### 自动化集成和部署
- 写worker flow，具体参考本仓.github/workflows/pages.yml
- 在github上打开本仓的设置，配置pages/deploy选项