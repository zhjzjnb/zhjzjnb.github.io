---
layout: post
title: 微信sdk链接冲突解决
category: 技术
tags:  sdk
keywords: duplicate sdk
---

微信登录的sdk和cocos2dx自带的base64冲突了,所以需要删除

主要用到的命令是lipo,ar

```
DIR="$(cd $(dirname $0); pwd)"
cd $DIR

arr_string=("i386" "armv7" "armv7s" "x86_64" "arm64")

for (( i = 0; i < ${#arr_string[*]}; i++ )); do
	echo ${arr_string[i]}
	rm -rf ${arr_string[i]}
	rm -rf ${arr_string[i]}.a
	mkdir -p ${arr_string[i]}
	lipo -thin ${arr_string[i]} libWeChatSDK.a -output ./${arr_string[i]}/${arr_string[i]}.a
	ar -dv ./${arr_string[i]}/${arr_string[i]}.a base64.o
	mv ./${arr_string[i]}/${arr_string[i]}.a ./
done

lipo -create  i386.a armv7.a armv7s.a x86_64.a arm64.a -output libWeChatSDK_no_base64.a 

```
