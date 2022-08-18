---
layout: post
title: ubuntu jdk
category: 技术
tags:  jdk 
keywords: jdk
---



```
sudo mkdir /usr/lib/jvm
sudo tar -zxvf jdk-8u321-linux-x64.tar.gz -C /usr/lib/jvm
sudo vi ~/.bashrc

export LD_LIBRARY_PATH=/usr/local/lib
export JAVA_HOME=/usr/lib/jvm/jdk8  ## 这里要注意目录要换成自己解压的jdk 目录
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH

source ~/.bashrc

```
