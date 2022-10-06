---
layout: post
title: code snippet
category: 技术
tags:  code snippet
keywords: code snippet
---
### 读取目录
```c
#include <dirent.h>
DIR *d = opendir(dirpath);
struct dirent *dp;
while ((dp = readdir(d)) != NULL) {
	if (strcmp(dp->d_name,".")==0||strcmp(dp->d_name,"..")==0||strcmp(dp->d_name,".DS_Store")==0){
		continue;
	}
	if (dp->d_type==DT_DIR) {
		printf("%s\n",dp->d_name);
	}
}
closedir(d);
```

### byte转16进制字符串
```c
void stringtohexstr(char* dst, const char* src, int len){
    for (int i = 0; i < len; i++) {
        sprintf(dst + 2*i, "%02x", (unsigned char)src[i]);
    }
    dst[len*2] = '\0';
}
```

### shell ssh隧道
```bash
192.168.0.1的mysql3306绑定本地,外部无法访问
#打开隧道转发,直接连接本地13306端口
ssh -CfNgf -L 13306:127.0.0.1:3306 root@192.168.0.1
#删除
lsof -i:13306|awk '{print $2}'|tail -1|xargs kill


```