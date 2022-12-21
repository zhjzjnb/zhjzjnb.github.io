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
#kill
ps aux|grep newp_iot_server|grep -v "grep"|awk '{print $2}'|xargs kill

```

### find删除3天的前文件
```bash
find . -mtime +3 -type f -name '*.bak'  | xargs rm -rf 
mtime is modify time
```

### 51单片机p0口需要上拉电阻
```c
机器周期=12/n(n指晶振频率,12一个机器周期)
定时的初值=M/机器周期
TH0 = (65536-初值)/256
TL0 = (65536-初值)%256
10ms定时器初值的计算
1.晶振12M.12MHz/12=1MHz,也就是说一秒=1000000次机器周期.10ms=10000次机器周期.65536-10000=55536(d8f0),TH0=0xd8,TL0=0xf0；
2.晶振11.0592M.11.0592MHz/12=921600Hz,就是一秒921600次机器周期,10ms=9216次机器周期.65536-9216=56320(dc00),TH0=0xdc,TL0=0x00.
```

### proteus8工具
```bash
常用51单片机	AT89C52
晶振	CRYSTAL
电阻	RES
排阻	RESPACK-8
瓷片电容	CAP
电解电容	CAP-ELEC
单刀单掷开关	SW-SPST
单刀双掷开关	SW-SPDT
按钮	BUTTON
发光二极管	LED
蜂鸣器	BUZZER
三极管	NPN/PNP
数码管	7SEG
```

### xcode provision位置
```bash
~/Library/MobileDevice/Provisioning Profiles
```
### 日志宏
```c
#define log(format, args...)    \
{\
    fprintf( stderr, format , ## args );\
}
```
