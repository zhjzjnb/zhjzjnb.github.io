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
