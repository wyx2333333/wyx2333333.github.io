---
title: 解决CleanMyMac X启动扫描时一直要求输入密码
date: 2021-03-11 15:32:41 +0800
categories: [Untitled]
tags: [Untitled]
---

## 终端执行指令

```bash
sudo launchctl load -w /Library/LaunchDaemons/com.macpaw.CleanMyMac4.Agent.plist
```
