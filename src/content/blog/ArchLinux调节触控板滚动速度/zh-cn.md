---
title: ArchLinux调节触控板滚动速度
pubDate: '2026-08-25'
description: ''
image: ''
draft: false
slugId: ArchLinux调节触控板滚动速度
category: 日常
pinTop: 0
---

# 触控板滚动速度调整记

> 一篇关于「触控板滑一下滚太多」的折腾笔记。

## 起因

触控板上下滑动时，页面一次滚动太多，想把它调小一点。

## 经过

### 1. 发现 GNOME 没这个设置

我的环境是 **GNOME + Wayland**。翻了一圈设置，发现 GNOME 原生**根本没有**"滚动手速/一次滚动距离"的调节选项，让我十分的纳闷，因为我觉得这是一个非常应该有的功能，结果原生不支持，那么只有寻找第三方来救赎了。。。

### 2. 找到救星：WSF

搜了下，社区有个专门解决这个的工具 **Wayland Scroll Factor（WSF）**，正好支持 GNOME Wayland。直接在 AUR 装：

```bash
paru -S wayland-scroll-factor # 我用包管理器是paru
```

装好设成 `0.5`，注销重登。

### 3. 原生应用有效，浏览器没用？

测了一下：GNOME 里的原生应用确实变缓了，但**浏览器一点变化都没有**。

排查发现：我使用的 Helium 浏览器（Chromium 系）之前跑在 XWayland 上，WSF 拦不到。于是给 Helium 加了原生 Wayland 参数：

```bash
echo '--ozone-platform-hint=auto' > ~/.config/helium-browser-flags.conf
```

重启 Helium，确认它已经用 `--ozone-platform=wayland` 跑起来了。

### 4. 真相大白

可是浏览器滚动还是没感觉变慢。继续查，才发现**不是 WSF 没生效，是 Chromium 本身就太快**：

Chromium 系在 Wayland 后端给滚动值加了个固定的 **×5.3 乘数**，GTK 用的常数小得多。所以同一因子下，Chromium 天生比原生应用快约 **5 倍**。WSF 对所有应用其实是均匀生效的，只是 Chromium 太快了，好舒服呀，5.3倍耍起嘛。

## 结局

因为 WSF 只能设一个全局因子，没法单独给浏览器调，要想浏览器也慢就得把值调得很小、代价是原生应用会跟着更慢，我也没招了。
