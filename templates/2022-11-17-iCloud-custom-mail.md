---
layout: post
title: 在 iCloud+ 中配置自定义域名邮箱
subtitle: 自讨苦吃
author: Cain Minami
categories: 技术
tags: 班门弄斧
sidebar: [article-menu]
---

2021年，Apple 开始正式向所有付费购买了 iCloud 存储空间的用户[^1]（iCloud+）提供自定义域名邮箱的托管服务。鉴于我开了 200G 的空间，正好最近在折腾域名，就打算把这个送的功能利用起来。


## 部署
在设置界面，Apple 集成了 Cloudflare（CF）的域名注册服务，估计可以供没有域名的人「一键」搭建邮箱，应该也就没有手动配置 DNS 的那些烦恼了。

## 问题

### SPF
`"v=spf1 include:icloud.com ~all"` 

## 体验
### iCloud
配置好自定义域名邮箱以后，这个域名最多可以给包括你在内的六个人使用，每个人最多登记3个不同的地址

只能在 iCloud

既然是托管在 iCloud 服务器上的，这个邮箱的体验与 iCloud 邮件服务一致，没什么特别好的地方，在这里也不过多赘述。

### Cloudflare
由于 CF 也提供免费的域名邮箱转发服务，这两者肯定会被拿来做比较的。CF 服务中登记的邮箱地址只能用于接收邮件，无法发送邮件。但是与提供有限数量的 iCloud 服务相比，CF 提供未登记邮箱地址的邮件转发。既然 CF 的服务只能提供邮件的接收和转发，开启这个功能以后，在个人使用的一般情况下，已经没有多大必要去登记邮箱地址了，这样也就获得了无上限的自定义邮箱地址。这时候登记地址的主要意义大概是 drop 掉发往特定地址的邮件了吧。


最终考虑到体验与应用的平衡，我把 `*@mail.corn.moe` 托管到 iCloud，主要用于邮件的发送与回复；而 `*@corn.moe` 则托管到 Cloudflare，用于接收邮件。

### 温馨提示
如果你也想这么干，记得先设置 iCloud 邮箱的 DNS，再设置 Cloudflare 的 DNS，要是顺序反了 Cloudflare 会锁定 MX 记录的修改权限，这样就没办法添加 iCloud 的有关记录了。

![Cloudflare 邮件 DNS 设置页面](assets/images/posts/221117_iCloud_CloudflareMX.png "如图中红框所示，如果设置好 Cloudflare DNS 后将无法修改 MX 记录")
<center>如图中红框所示，如果设置好 Cloudflare DNS 后将无法修改 MX 记录</center>

CF 的邮件转发还提供了简单的邮件数据统计。发往未登记地址的邮件可以被成功转发，CF 提供的邮件分析统计也可以正确统计。

### Apple WTF
在 iCloud 的 [QA 界面](https://support.apple.com/zh-tw/HT212514) 中，苹果要求托管在 iCloud 的自定义域名邮箱不能作为 Apple ID 的登录凭证，必须更换其他邮箱。但在这其中，还有这么一句话：

> 如果刪除該 Apple ID（而没有在删除前变更邮箱[^2]），你仍將無法（将这个邮箱）加入個人化電子郵件地址。

所以你们注销账号之后账号数据是不会删除永久保存的吗？

---
[^1]: 在正式版发布后，国区 Apple ID 取消了该功能，[有人](https://discussionschinese.apple.com/thread/253176687) 致电苹果客户支持并确认了这一变更。此外，无论设备在何处购买，当设备所在位置为中国时，Private relay 亦不可用。
[^2]: 引文中括号内的内容为方便读者理解上下文而添加，下同。