---
title: 使用webhook将github上的代码自动同步到服务器
date: 2020-10-16 11:20:05
categories:
- 代码
tags:
- webhook
- 部署
- 自动化
- 宝塔
- github
---

原理: 通过 git web钩子触发push事件之后，执行服务器上的脚本来拉取仓库的最新代码。
<!-- more -->

