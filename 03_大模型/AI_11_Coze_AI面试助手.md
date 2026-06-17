# Dify平台介绍[¶](#11-dify)

## 学习目标[¶](#_1)

- 了解什么是Dify以及和Coze的区别
- 掌握Dify基于docker的安装方式
- 掌握Dify添加大模型
- 实现第一个Dify案例

## 一、Dify平台介绍[¶](#dify)

**Dify** 是一款开源的大语言模型(LLM) 应用开发平台。它融合了后端即服务（Backend as Service, Baas） 的理念，使开发者可以快速搭建生产级的生成式 AI 应用。即使你是非技术人员，也能参与到 AI 应用的定义和数据运营过程中。

由于 Dify 内置了构建 LLM 应用所需的关键技术栈，包括对数百个模型的支持、直观的 Prompt 编排界面、高质量的 RAG 引擎、稳健的 Agent 框架、灵活的流程编排，并同时提供了一套易用的界面和 API。这为开发者节省了许多重复造轮子的时间，使其可以专注在创新和业务需求上。

Dify也是一个基于“工作流”编排的框架，能够以比较低的成本实现一个复杂的Agent。 但在实际工作中，Coze和Dify的定位有所不同，具体细节我在本章的第四小节再进行对比。

![img](https://i-blog.csdnimg.cn/direct/ca28273d8f644de7a16534baec51d124.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

## 二、Dify的安装[¶](#dify_1)

在安装 Dify 之前，请确保您的机器满足以下最低系统要求：

- CPU >= 2 Core
- RAM >= 4 GiB

同时注意，在安装的过程中，需要严格按照教案的顺序执行，确保每个过程都正常执行。否则dify将无法正常运行。

### 1 安装 WSL（Windows Subsystem for Linux）[¶](#1-wslwindows-subsystem-for-linux)

什么是WSL？

> WSL（Windows Subsystem for Linux）是微软推出的一项功能，它允许你在 Windows 系统上直接运行原生的 Linux 可执行文件，无需配置传统的虚拟机或采用双系统启动方式
>
> 因为Docker只能在linux上运行，所以需要先安装WSL。我们需要安装的是WSL2(**推荐且默认的版本**)。

参考地址：[WSL安装](https://link.juejin.cn?target=https%3A%2F%2Flearn.microsoft.com%2Fzh-cn%2Fwindows%2Fwsl%2Finstall)

**使用命令行安装WSL** 打开具有管理员权限的 PowerShell，运行以下命令以安装 WSL，使用默认的：

```
wsl --install
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

或运行：

```
wsl.exe --install -d ubuntu
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

安装完成后，重启电脑以完成配置。 如果提示需要你注册账户， 注册一个自己能记得住的账号和密码即可，后续基本不会使用到。

常见问题：

1. 403： 使用windows的应用商店搜索 "linux"， 下载ubuntu 22.0.4 TSL
2. 网络连接超时：更换更好的网络环境 ， 挂梯子
3. 提示内容包含”虚拟化“相关内容：确认自己的机器是否有虚拟化功能，以及进入BIOS系统进行开启
4. 下载速度非常慢：同超时解决办法，但是如果在30min以内能下载完成，建议等待。切换网络重新下载中或许引入其他问题

------

### 2 安装 Docker Desktop[¶](#2-docker-desktop)

注意：这一步需要确保WSL已经安装完毕。

1. **下载 Docker Desktop** 前往 [Docker 官方文档](https://link.juejin.cn?target=https%3A%2F%2Fdocs.docker.com%2Fdesktop%2Fsetup%2Finstall%2Fwindows-install%2F%23wsl-2-backend) 下载适用于 Windows 的 Docker Desktop。
2. **安装并配置 Docker** 根据安装向导完成 Docker 的安装，并确保启用了 WSL 2 后端支持。这将允许 Docker 在 WSL 环境中无缝运行。

安装完毕，进入设置页面（右上角齿轮）

![img](https://i-blog.csdnimg.cn/direct/4b8ac5e0247f482a8684beff792ee412.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

------

修改配置，如下：

```
"registry-mirrors": [
    "https://docker.m.daocloud.io",
    "https://hub-mirror.c.163.com",
    "https://mirror.baidubce.com",
    "https://ccr.ccs.tencentyun.com"
  ]
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

这一步的操作主要是为了给docker增加国内的镜像源，以便加快拉取镜像的速度。同时也可以不连外网实现镜像拉取

### 3 安装dify环境[¶](#3-dify)

#### 3.1下载dify项目压缩包[¶](#31dify)

直接下载

![img](https://i-blog.csdnimg.cn/direct/e007b5ccae7046eb9414e29586eacc16.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

网络比较差的同学可以直接使用课程提供的下载好的代码。

#### 3.2 进入项目根目录找到docker文件夹[¶](#32-docker)

![img](https://i-blog.csdnimg.cn/direct/f04a1a2684e34b69b3c45b8597738936.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

#### 3.3 env文件重命名[¶](#33-env)

![img](https://i-blog.csdnimg.cn/direct/b6e733eed80d4f318b01997741bfe5a9.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

#### 3.4 右键打开命令行[¶](#34)

![img](https://i-blog.csdnimg.cn/direct/7ed7164aa3c7447d8acdf7c2000e9979.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

#### 3.5 运行docker环境[¶](#35-docker)

```
docker compose up -d
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://i-blog.csdnimg.cn/direct/598ee7bbae9d4a4695bfd3d2705b970b.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

这里对网络有一定的要求，需要先拉取镜像再启动， 一次不成功的可以多尝试几次。

docker compose up -d 命令的原理如下：

![img](https://i-blog.csdnimg.cn/direct/2c0bf8ae4a6e46fa8f55b51fccc4ac6c.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

### 4 启动dify[¶](#4-dify)

在浏览器地址栏输入即可安装：

```
http://127.0.0.1/install
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

首次登录需进行注册：

![img](https://i-blog.csdnimg.cn/direct/5e927784f70d4b3cafcf60d02cc2e955.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

dify的主界面如下所示：

![img](https://i-blog.csdnimg.cn/direct/4d4e7ba979784114be54a3b833dcc80e.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

## 三、第一个Dify案例[¶](#dify_2)

### 1 大模型接入[¶](#1)

在 Dify 中，我们按模型的使用场景将模型分为以下3类：

1. **系统推理模型**。 在创建的应用中，用的是该类型的模型。智聊、对话名称生成、下一步问题建议用的也是推理模型。
2. **Embedding 模型**。在知识库中，将分段过的文档做 Embedding 用的是该类型的模型。在使用了知识库的应用中，将用户的提问做 Embedding 处理也是用的该类型的模型。
3. **语音转文字模型**。将对话型应用中，将语音转文字用的是该类型的模型。

在 Dify 的 `设置 > 模型供应商` 中设置要接入的模型。

![img](https://i-blog.csdnimg.cn/direct/8105894ec6a14c1e8b6186fa38b65569.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

添加大模型：

![img](https://i-blog.csdnimg.cn/direct/4a92c3ea45c145e881631b0e7eee8982.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

设置默认的大模型：

![img](https://i-blog.csdnimg.cn/direct/ae1ef8c2d61841dfa2803912b136e57f.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

### 2 创建一个应用[¶](#2)

在“工作室”页面创建一个空白应用，如下：

![img](https://i-blog.csdnimg.cn/direct/73265c2b23664940a92b4105435f7a9b.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

在这里我们可以看到，Dify中“应用”的概念和Coze不同， 这里的应用是工作流或者是对话流， 而不是Coze中完整的智能体或者应用。

搭建一个简单的工作流：

![img](https://i-blog.csdnimg.cn/direct/9bda079cc51c4d8a9fb2309e4ce9da62.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

首先，第一步，在“开始”工作流增加一个query字段，如下：

![img](https://i-blog.csdnimg.cn/direct/eaba4fb05f434d56aad70cbd932ad21f.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

添加一个LLM节点，如下：

![img](https://i-blog.csdnimg.cn/direct/c3c3b7b020aa452ab2dd311164b7b24b.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

设置LLM节点中各变量：

![img](https://i-blog.csdnimg.cn/direct/10a774fdd8984430b7832a00b34e43e4.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

系统提示词：

```
你是一个无所不能的助手，能够根据用户输入的问题返回对应的答案
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

添加一个结束节点，并接收LLM节点的输出作为输入：

![img](https://i-blog.csdnimg.cn/direct/294be4d2979d492faa0b560719352a54.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

### 3 试运行工作流[¶](#3)

点击试运行，输入

```
西安有什么好吃的
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

如下图：

![img](https://i-blog.csdnimg.cn/direct/7b44ce1915d544e9a1827b7fd85612a4.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

结果如下：

![img](https://i-blog.csdnimg.cn/direct/84449c6525c94d7a88f31e28052e73cf.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

## 四、 Dify和Coze的对比[¶](#difycoze)

虽然Dify和Coze看起来都像是基于“工作流”，在实际工作中，我们该如何选择？

在以下情况下，果断选择 Coze：

- 身份是新手、运营或内容创作者，想零代码在几分钟内搭建一个AI聊天机器人。
- 核心需求是快速原型验证，或者为抖音、飞书等平台制作一个营销助手、客服机器人。
- 不关心底层技术细节，追求“开箱即用”的流畅体验。

在以下情况下，Dify 是你的更优选择：

- 身份是开发者或企业技术决策者，需要构建复杂、定制化的生产级AI应用（如企业知识库、智能问答系统）。
- 对数据隐私、安全性和合规性有高要求，必须进行私有化部署。
- 希望拥有对模型、工作流和数据的**完全控制权**，避免被单一生态绑定。
- 技术团队有能力进行二次开发：团队中有开发者，希望基于开源代码进行定制开发，或将AI能力通过API集成到现有业务系统中

简单来说，**求快、求简单、个人/轻量级项目，选Coze；求强、求复杂、企业级/定制化需求，选Dify**

