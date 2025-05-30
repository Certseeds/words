---
author: "Certseeds"
date: "2025-03-27"
lastmod: "2025-03-27"
title: "记录ollama最近的使用体验和几个模型的感受"
description: "ollama and a few models"
tags: ["notes", "experience"]
---

# Ollama 和几个模型

> 这篇是短文

## 任务描述

近期主要有两个依赖于llm的任务, 一个是从译文中执行人名提取, 用产物来辅助校对; 另外一个是视觉识别, 输入手写或者是印刷体的题干, 选项, 图片等, 将其提取为markdown+latex, 主要目标是缩减pdf体积.

### 人名提取

人名提取要在效率和效果之间平衡, 年初到现在用qwen2.5的模型来测试, 0.5b, 1.5b, 3b的模型干这事虽然快的飞起, 但是效果很差, 非常容易遗漏人名, 将人名和前后的形容词动词连接起来, 又或者是出现完全不相关的事情.

随后使用了`qwen2.5:32b`的模型, ollama确实能将这个体积就比显存大的模型跑起来, 但是速度实在是太慢了, 一个20万字的任务, 用6-7token/s的速度要跑几个小时, 有这时间我都能把书正着读一遍再反着读一遍. 这个速度几乎和只能执行一次相捆绑, 违背了校对过程中可能存在的多次迭代.

之后尝试了下`qwen2.5:14b`, 大小适中, 在人名提取这个小context的背景下正适合, 速度也能接受, 虽然还是会粘一些的,得,地以及奇怪的字在名字里, 但是出现的概率很少了, 可以完成任务.

在叠加上ollama对json format的支持, 省去了在输出中再次处理的时间, 优势++.

然而可能是ollama对推理模型支持的问题, 或者是推理模型得参数过小, 这项任务对`deepseek-r1:14b`, `gemma3:12b`都会出现明显的过度推理现象, 即使要求不加思考, 仍然会成倍地输出结果; ollama的 json format 对其也失去了约束力, 输出不是none就是一串怪东西.

本任务现在使用 `qwen2.5:14b`

**UPDATE**

近期将人名提取任务升级到了qwen3 on ollama, 问题如下

1. 没有显式的no_think模式的api启动开关, api中只能将 `/no_think` 拼接到prompt中.
2. 在使用 `/no_think`的情况下会在出人意料的短上下文的情况下, 输出几乎无限的token.

``` log
>>> hey, 请阅读下面用三个 at 符号括住的一段话, 使用/no_think模式, 这摘录自一篇战锤30k荷鲁斯叛乱系列小说中. 希望你在读完之后, 从中提取出人名, 并以json格式返回, 要求逐个放入names这个列表中, 只需要人名, 不需要'他','她','它','您'等第三人称代词, 不需要'兄弟', '姐妹'这样的亲属称谓, 不需要'大人', '指挥官', '队长', '大师', '总管'之类的职位称呼, 不需要A的B这样的连词(至少要将其拆分为A和B), 不需要车辆, 飞行器的名称(比如xx号), 不要输出代码, 也不需要输出额外的前缀、后缀. 如果没有人名则输出'WU-REN-MING'。@@@>@@@
<think>

</think>

```json
{"names": ["荷鲁斯", "罗格·斯图加特", "马格努斯", "安格隆", "费鲁斯·马格努斯", "鲁莽者", "玛莎", "克里昂", "阿斯塔特", "阿比多斯", "阿德里安", "亚历山大", "阿德里安·阿比多斯", "阿德里安·亚历山大", "阿德里安·阿比多斯·亚历山大", "阿德里安·亚历山大·阿比多斯", "阿德里安·亚历山大·阿比多斯·阿德里安", "阿德里安·亚历山大·阿比多斯·阿德里安·亚历山大", "阿德里安·亚历山大·阿比多斯·阿德里安·亚历山大·阿比多斯", "阿德里安·亚历山大·阿比多斯·阿德里安·亚历山大·阿比多斯·阿德里安", "阿德里安·亚历山大·阿比多斯·阿德里安·亚历山大·阿比多斯·阿德里安·亚历山大", "阿德里安·亚历山大·阿比多斯·阿德里安·亚历山大·阿比多斯·阿德里安·亚历山大·阿比多斯", "阿德里安·亚历山大·阿比多斯·阿德里安·亚历山大·阿比多斯·阿德里安·亚历山大·阿比多斯·阿德里安", "阿德里安·亚历山大·阿比多斯·阿德里安·亚历山大·阿比多斯·阿德里安·亚历山大·阿比多斯·阿德里安·亚历山大", "阿德里安·亚历山大·阿比多斯·阿德里安·亚历山大·阿比多斯·阿德里安·亚历山大·阿比多斯·阿德里安·亚历山大·阿比多斯", "阿德里安·亚历山大·阿比多斯·阿德里安·亚历山大·阿比多斯·阿德里安·亚历山大·阿比多斯·阿德里安·亚历山大·阿比多斯·阿德里安", "阿德里安·亚历山大·阿比多斯·阿德里安·亚历山大·阿比多斯·阿德里安·亚历山大·阿比多斯·阿德里安·亚历山大·阿比多斯·阿德里安·亚历山大", "阿德里安·亚历山大·阿比多斯·阿德里安·亚历山大·阿比多斯·阿德里安·亚历山大·阿比多斯·阿德里安·亚历山大·阿比多斯·阿德里安·亚历山大·阿比多斯", "阿德里安·亚历山大·阿比多斯·阿德里安·亚历山大·阿比多斯·阿德里安·亚历山大·阿比多斯·阿德里安·亚历山大·阿比多斯·阿德里安·亚历山大·阿比多斯·阿德里安", "阿德里安·亚历山大·阿比多斯·阿德里安·亚历山大·阿比多斯·阿德里安·亚历山大·阿比多斯·阿德里安·亚历山大·阿比多斯·阿德里安·亚历山大·阿比多斯·阿德里安·亚历山大", "阿德里安·亚历山大·阿比多斯·阿德里安·亚历山大·阿比多斯·阿德里安·亚历山大·阿比多斯·阿德里安·亚历山大·阿比多斯·阿德里安·亚历山大·阿比多斯·阿德里安·亚历山大·阿比多斯"
......
```

居然会在嵌入一个">"的情况下, 输出无限制的, 无意义, 越来越长的的重复字符串. 在手动停止时已经输出了超过6700个字符

经过几次重复, 这类问题常见的输出模式包括但不限于单个名字重复, 多个名字称周期性出现, 多个名字在序列中随机分布出现等, 花样很多, 但是全部会导致服务不可用.

并且这样的输出, 在使用json-format-output时, `temperature = 0`时都会出现, 难以相信会出现这种劣质问题, 这个问题在qwen2.5上完全不会出现

**UPDATE 2**

发现不仅是">", 就连一些没有人名的句子输入进去也会产生无限输出.

### 视觉识别

> 现在存在<https://github.com/iamarunbrahma/vision-parse>这种库

对`gemma3:12b`来说, 看上去模型是存在基础识别图像的能力的, 并且将其转换到markdown+latex的能力也是有的, 但是当图片和较长(100字左右)的prompt一起请求时, 返回就会出现严重的幻觉, 要求 "请将输入中存在的数学, 物理, 化学等公式识别为markdown+latex"输出, 就能在思考中发现 "虽然没有发现, 但是可能有以下公式"的这种奇怪设定; 当输入只有题干时还能给生成出完全不相关的内容.

并且`gemme3:12b`在这项任务中同样存在对指令执行能力差的现象, 要求在最后 `<md+latex></md+latex>`中输出内容, 最后的code-block中不能有推理, 输出中就完全没有推理; 同样对json format的约束执行的很差, 到头来最合适的prompt居然是 `hey, 请将图中的题目转换为markdown+latex`

即使用了上面的prompt还是会随机蹦出来一些乱七八糟的 `Explanation of the LaTeX`, 感觉gemma被训练成了不会好好使用prompt的样子, 这种输出可能还是要过一遍`qwen2.5:14b`这种模型, 让`gemme3:12b`负责vision部分, `qwen2.5:14b`负责将其净化为可用的格式.

本任务现在使用 `gemma3:12b` + `qwen2.5:14b`
