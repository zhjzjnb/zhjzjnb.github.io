---
layout: post
title: raspberry 配置正确时区
category: raspberry
tags: [raspberry,date]
keywords: raspberry
---

1.安装NTP
```
sudo apt-get install ntpdate
```

2.启动NTP
```
sudo timedatectl set-ntp true
```

3.修改时区
```
sudo dpkg-reconfigure tzdata
```

4.设置asia/shanghai

