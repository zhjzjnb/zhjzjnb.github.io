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

### 大小端
```c
union Un {
	char c;
	int i;
} un;
un.i = 0x1;
//如果是大端机则是0x0001 0000 0000 0000
//如果是小端机则是0x0000 0000 0000 0001
//大端存储：就是把一个数的低位字节序的内容存放到高地址处，高位字节序的内容存放在低地址处。
//小端存储：就是把一个数的低位字节序的内容存放到低地址处，高位字节序的内容存放在高地址处。
if (1 == un.c) {
	printf("当前模式为小端存储\n");
} else {
	printf("当前模式为大端存储\n");
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
void Timer0Routine() interrupt 1{}
void Timer1Routine() interrupt 3{}
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

### android 判断64位
```java
private String getos(){

    StringBuilder sb = new StringBuilder();
    sb.append("android.os.Process.is64Bit():").append(android.os.Process.is64Bit()).append("\n");
    sb.append("is64:").append(is64()).append("\n");
    sb.append("isART64:").append(isART64()).append("\n");
    sb.append("is64bit:").append(is64bit()).append("\n");

    return sb.toString();

}

public  boolean is64() {
    String arch = System.getProperty("os.arch");
    if (arch != null && arch.contains("64")) {
        return true;
    } else {
        return false;
    }
}

private  boolean isART64() {

    final String tag = "is64ART";
    final String fileName = "art";

    try {
        ClassLoader classLoader = getClassLoader();
        Class<?> cls = ClassLoader.class;
        Method method = cls.getDeclaredMethod("findLibrary", String.class);
        Object object = method.invoke(classLoader, fileName);
        if (object != null) {
            return ((String)object).contains("lib64");
        }
    } catch (Exception e) {

    }

    return false;
}

public  boolean is64bit() {
    boolean is64bit = false;
    if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.LOLLIPOP) {
        is64bit = Build.SUPPORTED_64_BIT_ABIS.length > 0;
    } else {
        try {
            BufferedReader localBufferedReader =
                    new BufferedReader(new FileReader("/proc/cpuinfo"));
            if (localBufferedReader.readLine().contains("aarch64")) {
                is64bit = true;
            }
            localBufferedReader.close();
        } catch (IOException exception) {
            exception.printStackTrace();
        }
    }
    return is64bit;
}
```

## h5视频加速
```js
/* 先找到video class类别 然后点击console输入代码并运行 */
document.querySelector('video').defaultPlaybackRate = 1.0;  // 默认值
document.querySelector('video').play();
document.querySelector('video').playbackRate = 16.0;    // main语句
```

## mybatis generator 大坑，nullCatalogMeansCurrent指定只获取本库表
```xml
 <jdbcConnection driverClass="com.mysql.jdbc.Driver"
    connectionURL="jdbc:mysql://127.0.0.1:3306/zfuck?useSSL=false&amp;nullCatalogMeansCurrent=true"
		userId="root"
    password="root">
</jdbcConnection>
```

## cocos creator vscode
```json
{
    "files.exclude": {
        "**/*.meta": true,
        "library/": true,
        "local/": true,
        "temp/": true,
        "**/*.png": true,
        "**/*.skel": true,
        "**/*.atlas": true,
    },
    "search.exclude": {
        "\"library/\"": true,
        "\"temp/\"": true,
        "build/": true,
        "tools/": true
    },
    "editor.codeActionsOnSave": {
        "source.fixAll": true
    },
    "files.eol": "\n",
    "prettier.singleQuote": true
}
```


## beyond compare
```
--- BEGIN LICENSE KEY ---
ayvZeJDYPBHS4J-1K6g6bDBuPoo0G-oGAq35blZtAoRqC-qQeSz28XAzX
6nTx9laTMLRCp6nAIhHNGZ2ehkeUfbnFaxEeLvI8fJavn-XQLNbOumCLU
qgdNbNMZiFRU03+OTQnw4V-E2YKTYi-LkgPzE6R-yIJGDNWfxH2AdpIgg
8rlpsbrTs9Dt1zysUfvAEi0dKbmGIi3rqf7yWmwDh1AI5VyoWFIejvJwJ
Lmlr2CjQ1VZ3DySCfBDuKcYmOCeK7jzEWPUnAw+f9360nIiiNEB0YGkwB
kdtgaKEEik7aNiI3jXvp5r34wViVJCiX7m2y7pqBV9gZIvP9hP9KPnP++++
--- END LICENSE KEY -----

--- BEGIN LICENSE KEY ---
GXN1eh9FbDiX1ACdd7XKMV7hL7x0ClBJLUJ-zFfKofjaj2yxE53xauIfkqZ8FoLpcZ0Ux6McTyNmODDSvSIHLYhg1QkTxjCeSCk6ARz0ABJcnUmd3dZYJNWFyJun14rmGByRnVPL49QH+Rs0kjRGKCB-cb8IT4Gf0Ue9WMQ1A6t31MO9jmjoYUeoUmbeAQSofvuK8GN1rLRv7WXfUJ0uyvYlGLqzq1ZoJAJDyo0Kdr4ThF-IXcv2cxVyWVW1SaMq8GFosDEGThnY7C-SgNXW30jqAOgiRjKKRX9RuNeDMFqgP2cuf0NMvyMrMScnM1ZyiAaJJtzbxqN5hZOMClUTE+++
--- END LICENSE KEY -----

--- BEGIN LICENSE KEY ---
H1bJTd2SauPv5Garuaq0Ig43uqq5NJOEw94wxdZTpU-pFB9GmyPk677gJ
vC1Ro6sbAvKR4pVwtxdCfuoZDb6hJ5bVQKqlfihJfSYZt-xVrVU27+0Ja
hFbqTmYskatMTgPyjvv99CF2Te8ec+Ys2SPxyZAF0YwOCNOWmsyqN5y9t
q2Kw2pjoiDs5gIH-uw5U49JzOB6otS7kThBJE-H9A76u4uUvR8DKb+VcB
rWu5qSJGEnbsXNfJdq5L2D8QgRdV-sXHp2A-7j1X2n4WIISvU1V9koIyS
NisHFBTcWJS0sC5BTFwrtfLEE9lEwz2bxHQpWJiu12ZeKpi+7oUSqebX+
--- END LICENSE KEY -----

在路径打开trial.key：/Applications/Beyond\Compare.app/Contents/Resources/trial.key
--- BEGIN LICENSE KEY ---
H1bJTd2SauPv5Garuaq0Ig43uqq5NJOEw94wxdZTpU-pFB9GmyPk677gJ
vC1Ro6sbAvKR4pVwtxdCfuoZDb6hJ5bVQKqlfihJfSYZt-xVrVU27+0Ja
hFbqTmYskatMTgPyjvv99CF2Te8ec+Ys2SPxyZAF0YwOCNOWmsyqN5y9t
q2Kw2pjoiDs5gIH-uw5U49JzOB6otS7kThBJE-H9A76u4uUvR8DKb+VcB
rWu5qSJGEnbsXNfJdq5L2D8QgRdV-sXHp2A-7j1X2n4WIISvU1V9koIyS
NisHFBTcWJS0sC5BTFwrtfLEE9lEwz2bxHQpWJiu12ZeKpi+7oUSqebX+
--- END LICENSE KEY -----
```
