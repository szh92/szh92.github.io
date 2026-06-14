---
layout: default
title: "WSL和Docker"
date: 2026-06-14
excerpt_separator: <!--more-->
---



可以把它理解成 **“在 Windows 里面套了一层 Linux，再在 Linux 里面放很多独立的小盒子”**。

## 1. WSL 是干什么的？

WSL（Windows Subsystem for Linux）本质上是：

> **让 Windows 可以直接运行 Linux。**

以前想用 Linux，要么双系统，要么虚拟机。WSL 相当于在 Windows 里面装了一个轻量级 Linux（比如 Ubuntu）。

所以：

```
Windows
└── WSL（Ubuntu）
```

这样你就能在 Windows 上使用 Linux 命令、运行 Python、安装各种 AI 工具。

### 为什么很多 AI 都喜欢用 WSL？

因为：

* 大部分 AI 工具最初都是给 Linux 开发的；
* Linux 环境更稳定；
* 安装依赖更方便；
* 与服务器环境一致。

---

## 2. Docker 是干什么的？

Docker 可以理解成：

> **一个一个独立的小房间（容器）。**

每个容器里面：

* 有自己的程序；
* 有自己的环境；
* 和外面尽量隔离。

例如：

```
Docker容器A
    ├─ Python
    ├─ AI程序
    └─ 各种依赖

Docker容器B
    ├─ Node.js
    └─ 数据库
```

互相不影响。

### 为什么要用 Docker？

因为：

#### ① 防止环境冲突

例如：

AI1 要 Python 3.10

AI2 要 Python 3.12

Docker 可以让它们各用各的。

---

#### ② 方便复制

别人给你一个 Docker 镜像：

```
docker run xxx
```

就能得到一模一样的环境。

不用折腾安装。

---

#### ③ 隔离

AI 在容器里面活动。

如果它删文件：

通常只能删容器里面的东西。

把容器删掉重新创建即可。

所以很多人让 AI 在 Docker 里跑，而不是直接操作电脑。

---

## 3. WSL 和 Docker 的关系

实际上通常是：

```
你的电脑
│
├─ Windows
│
└─ WSL (Debian)
      │
      └─ Docker
            │
            ├─ 容器1（AI）
            ├─ 容器2（数据库）
            └─ 容器3（其他程序）
```

也就是：

### WSL 提供 Linux 环境

↓

### Docker 在 Linux 环境中运行

↓

### AI 在 Docker 容器中运行

---

## 4. 为什么这样比较安全？

如果直接让 AI 操作：

```
Windows
└─ AI
```

AI 有可能：

* rm 删除文件；
* 改系统配置；
* 安装乱七八糟的软件；

影响整个电脑。

而现在：

```
Windows
└─ WSL
      └─ Docker
            └─ AI
```

相当于：

### AI 被关在一个小盒子里。

即使 AI：

* 删文件；
* 装软件；
* 把环境搞崩；

通常只影响那个容器。

把容器删掉：

```bash
docker rm
docker compose up
```

几分钟恢复。

---

## 用房子来比喻

### Windows

整个小区。

### WSL

小区里的一栋楼（Linux）。

### Docker

楼里的一个个房间（容器）。

### AI

住在某个房间里的人。

于是：

```
小区（Windows）
    ↓
一栋楼（WSL）
    ↓
一个房间（Docker）
    ↓
AI
```

AI 再怎么折腾，通常也只是把自己的房间弄乱，不会把整栋楼甚至整个小区拆掉。

---

# 最后说下怎么正确进入claude code

为什么进入 Claude Code 要先 Reopen in WSL，再 Reopen in Container？

目前我的使用流程如下：

Windows
↓
WSL（Linux）
↓
Docker Container
↓
Claude Code
# 第一步：Reopen in WSL

首先在 VS Code 中选择 Reopen in WSL，进入 Linux 环境。

这是因为许多 AI 工具、Python 环境以及依赖管理最初都是基于 Linux 开发的，在 Windows 上直接运行可能会遇到兼容性问题。因此通常会先在 Windows 中进入 WSL（Windows Subsystem for Linux）。

此时环境变成：

Windows
└── WSL(Ubuntu/Debian)
# 第二步：Reopen in Container

进入 WSL 之后，再选择 Reopen in Container，进入 Docker 容器。

容器可以理解为一个隔离的小环境。Claude Code、Python 环境以及各种依赖都运行在容器内部，即使 AI 把环境弄乱，也通常只影响当前容器，而不会影响整个系统。

此时环境变成：

Windows
└── WSL
      └── Docker Container
              └── Claude Code

因此，目前进入 Claude Code 的标准流程可以概括为：

VS Code
    ↓
Reopen in WSL
    ↓
Reopen in Container
    ↓
启动 Claude Code

整个结构实际上是：

Windows
    ↓
WSL（提供 Linux 环境）
    ↓
Docker（提供隔离环境）
    ↓
Claude Code（运行 AI）

# 第三步：退出时连按两边ctrl+D
方便claude code记录历史对话在对应目录的历史对话的session中


