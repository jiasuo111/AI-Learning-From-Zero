## 学习目标[¶](#_1)

- 了解Agent智能体的基本概念
- 了解Agent智能体和应用

## 一、智能体的基本概念[¶](#_2)

### 1 Agent智能体[¶](#1-agent)

在以chatgpt为代表的大模型出现以后，模型具备了“思考”能力，能够帮助我们解决一些知识获取和推理等问题。但是因为大模型只能输出内容，能够帮助我们解决的业务问题有限，如果能让大模型具备使用工具的能力，将很大程度上拓宽大模型能力的边界。 于是基于大模型+工具调用这种解决方案，“Agent智能体”这个概念在业界出现并大火。

智能体，又称Agent。是指能够感知环境、分析信息、自主决策并采取行动以实现特定目标的软件实体或系统。它以大模型（如GPT等）为“大脑”，具备理解、规划、决策、记忆和行动的能力。

AI Agent = LLM+ 记忆 + 任务规划 + 工具使用

![img](https://i-blog.csdnimg.cn/direct/b7a97b24074e455fab5f994bbbc1a0c4.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

大模型就像一个“百科全书”，知识渊博，有问必答，但它不会主动去“做”什么事。而智能体则像一个“全能助理”，比如你告诉它“我下周三要去杭州出差，帮我安排好行程”，它就能自主完成查询航班、预订酒店、规划市内交通、提醒天气等一系列操作 。它不仅“有脑”（能思考），还“有手有脚”（能调用工具去执行）。

Agent的目标和理念：让它成为一个能自主思考、能帮你办事的“智能大脑”或“数字员工”。

现阶段，我们的Agent还处于过渡阶段，一般是以一个“流水线”的方式事先规定好业务流程，通过串联多个大模型负责处理不同的任务，从而解决一个复杂的业务场景。

目前实现智能体的技术路径：

| 技术栈          | 优势                                                         | 劣势                                   |
| --------------- | ------------------------------------------------------------ | -------------------------------------- |
| Coze            | 快速实现需求，低代码                                         | 限制较多，无法实现比较定制化的功能     |
| Dify            | 快速实现需求，低代码，能够对接本地模型和服务                 | 本地版插件太少，很多功能需要重新造轮子 |
| Langgraph等框架 | 扩展性极强，基本上不受限制，可以对接本地模型，实现自定义复杂逻辑等 | 从零构建                               |

而Agent的终极形态是不需要开发这些流水线，Agent能够完全自主完成我们的各种需求，这依赖于大模型的能力，目前来看还需要一定的时间。

### 2 智能体开发平台-GPTS[¶](#2-gpts)

2023年11月，OpenAI 为旗下的 ChatGPT 推出了一项名为“GPTs”的服务，允许用户无需写代码就可以根据特定需求创建“属于自己的 ChatGPT 版本”，也就是基于 ChatGPT 创建一个Agent。

GPT Store访问地址：https://chat.openai.com/gpts，注意需要科学上网，以及当前只针对plus用户开通了使用权限。

![img](https://i-blog.csdnimg.cn/direct/9ef32e4b458f4e31a025048c6858d9fd.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

## 二、Coze平台简介[¶](#coze)

### 1 Coze平台介绍[¶](#1-coze)

#### 1.1 Coze介绍[¶](#11-coze_1)

Coze 是由字节跳动推出的一个AI聊天机器人和应用程序编辑开发平台，可以理解为国产版的GPTs. Coze（扣子）分为国内版和国外版：

- 国内版访问地址：https://www.coze.cn/home，背后大模型应用的是字节自研的豆包大模型、通义千问和kimi大模型
- 国外版访问地址：https://www.coze.com/home，背后大模型应用的是GPT-4、Gemini等，但是需要一些科学上网的方法。

Coze还提供了多种插件、知识、工作流、长期记忆和定时任务等功能，来增强聊天机器人的能力和交互性。而且你可以将搭建的 Bot 发布到各类社交平台和通讯软件上，让更多的用户与你搭建的 Bot 聊天。

#### 1.2 Coze注册[¶](#12-coze)

使用地址：[https://www.coze.cn/]，可以使用手机号进行注册，也可以使用抖音或飞书进行注册。

![img](https://i-blog.csdnimg.cn/direct/8a357d8eb5ad4a4ba3d0a315a57656c7.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

注册成功后我们就可以使用coze平台了。

Coze平台在线版的主页如下：

![img](https://i-blog.csdnimg.cn/direct/dfc00589627a4da687d1a22952296007.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

Coze平台的套餐有个人版和企业：

![img](https://i-blog.csdnimg.cn/direct/c26447f5dc6f4f029c767d313f1131fc.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

![img](https://i-blog.csdnimg.cn/direct/9045203dba34430c9967ca235baadc94.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

### 2 Coze平台开源版[¶](#2-coze)

因某些业务场景的数据安全要求较高，要求模型、数据不能暴露到公网。因此，私有化部署成为了部分业务场景的刚需。Coze针对私有化部署场景进行了开源，github地址：[Coze Studio](https://github.com/coze-dev/coze-studio) ，在2025年7月26日正式开源，允许免费商用和本地化部署。‌‌

![img](https://i-blog.csdnimg.cn/direct/65cbb21ac8004d8fa2990abf4d80d239.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

[Coze Studio](https://www.coze.cn/home) 是一站式 AI Agent 开发工具。提供各类最新大模型和工具、多种开发模式和框架，从开发到部署，为你提供最便捷的 AI Agent 开发环境。

- **提供 AI Agent 开发所需的全部核心技术**：Prompt、RAG、Plugin、Workflow，使得开发者可以聚焦创造 AI 核心价值。
- **开箱即用，用最低的成本开发最专业的 AI Agent**：Coze Studio 为开发者提供了健全的应用模板和编排框架，你可以基于它们快速构建各种 AI Agent ，将创意变为现实。

因coze开源版在企业中应用较少，因此在这里我们主要介绍Coze在线版的使用。

私有化部署使用的话，一般使用Dify，原因有以下几点：

- dify是一个更早开源的平台，与coze功能类似，且社区成熟、功能对比coze更加强大，已经占据了较大的市场
- coze对比dify的优势在于丰富的插件库、应用型智能体、以及对于多模态的支持，在开源版这些功能被阉割比较严重