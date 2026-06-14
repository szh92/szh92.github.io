---
title: 'Claude Code的Harness与Skill初探'
date: 2026-06-14
---

最近在朋友帮助下，开始尝试使用 Claude Code，并配合 WSL、Docker、Research Harness 和 Skill 构建一个相对安全、可复现、适合科研工作的 AI 环境。这里记录一下目前的理解。



# Claude Code Research Harness

Research Harness 是一个帮助 Claude 更稳定完成复杂任务的工作流。

## 安装

首先 fork 对应仓库，然后：

```bash
git clone （自己的仓库地址）
cd 仓库名称
```

完成后即可使用。

## Harness 的思想

普通提示词：

```
直接让 AI 完成任务
```

Research Harness：

```
任务
↓
拆分成多个步骤
↓
每一步分别验证
↓
只有上一步正确，才进入下一步
↓
最终汇总结果
```

因此它更像：

> 拆步骤 → 验证步骤 → 出错返回重新执行 → 继续下一步

而不是一次性生成全部内容。

这种方式在科研、代码分析、文献阅读等任务中更加稳定。

# Skill 是什么？

可以把 Skill 理解成：

> 一个 md 文件版本的函数。

普通情况下，我们需要反复输入长提示词：

```
请按照某种格式分析论文……
请先总结贡献……
再分析识别策略……
最后评价稳健性……
```

而 Skill 把这些提示词封装起来。

调用时只需要：

```
/某个skill
```

即可自动执行对应流程。

本质上：

```
Skill ≈ 可复用提示词 + 自动化流程
```

# 安装 Skill

例如：

```text
/plugin marketplace add Imbad0202/academic-research-skills

/plugin install academic-research-skills
```

然后：

```text
/reload-plugins
```

重新加载即可。

# Skill 的使用

Skill 有两种触发方式。

### 1. 手动调用

例如：

```text
/skill-name
```

类似于调用函数。

### 2. Trigger 自动触发

当输入内容满足条件时，Claude 会自动调用对应 Skill。

因此很多时候并不需要手动输入 `/`。

# Skill 不能调用怎么办？

有时候会出现：

* Skill 未加载；
* Plugin 安装失败；
* Trigger 没生效。

通常可以尝试：

```text
/plugin list
```

查看是否已经安装。

然后：

```text
/reload-plugins
```

重新加载。

如果仍然有问题，可以：

```text
/doctor
```

查看具体错误信息。

# 自定义 Skill

Skill 本质上是一个 markdown 文件，因此也可以自己编写。

例如：

```
skills/

    paper-reading.md

    causal-inference.md

    literature-review.md
```

通过编写固定流程，实现自己的自动化提示词。

完成后：

### 注册 marketplace

或者直接通过地址安装：

```text
/plugin install 仓库地址
```

这样就可以在不同项目之间复用自己的 Skill。

# 总结

目前整个环境可以理解成：

```
Windows
↓
WSL
↓
Docker Container
↓
Claude Code
↓
Research Harness
↓
Skill
```

其中：

* WSL 提供 Linux 环境；
* Docker 提供隔离环境；
* Claude Code 负责交互；
* Harness 负责拆分和验证复杂任务；
* Skill 用于封装和复用提示词。

对于科研工作而言，这种结构相比单纯聊天式使用 AI，更稳定，也更容易复现。

