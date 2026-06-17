##  学习目标[¶](#_1)

- 了解什么是多智能体
- 掌握多智能体的创建方式
- 了解单Agent自主规划模式

## 一、 什么是Multi-Agent[¶](#multi-agent)

在单 Agent 模式下处理复杂任务时，你必须编写非常详细和冗长的提示词，而且你可能需要添加各种插件和工作流等，这增加了调试智能体的复杂性。调试时任何一处细节改动，都有可能影响到智能体的整体功能，实际处理用户任务时，处理结果可能与预期效果有较大出入。

为了解决上述问题，扣子提供了多 Agent 模式（也就是multi-agent），该模式下你可以为智能体添加多个 Agent，并连接、配置各个 Agent 节点，通过多节点之间的分工协作来高效解复杂的用户任务。

例如我们要构建一个类似于“小爱同学”的语音助手的智能体来控制各类设备（比如空调、电视、电饭煲等）， 每个设备的操作和交互方式又有很多种， 如果把这些功能全部通过一个agent来进行控制，那么这个agent的复杂度将会非常复杂。这里因为每个设备之间的功能都是独立互不干扰的，我们可以根据设备的不同，拆分成多个agent：

- 一个入口用来判断用户要控制什么设备，并调用对应的具体设备的agent。这个总的agent我们把它叫做 “父agent” 或者 “主agent”，主要作为协调者，充当“总指挥”，负责接收初始任务、进行意图识别和任务分解。
- 具体设备的agent则可以处理这个设备下所有支持的具体的操作和交互，根据设备不同，agent的实现也是不同的。这个具体设备的agent我们把它叫做“子agent”或者叫 “从agent”，职责单一且明确。

![img](https://i-blog.csdnimg.cn/direct/a11d60428e594bb180c336381a64ee49.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

这就是一个经典的**主从模式**或者**父子模式**的多agent结构。

## 二、创建一个Multi-Agent智能体[¶](#multi-agent_1)

接下来我们将通过一个“多语言翻译”案例来演示如何创建一个Multi-Agent智能体。

### 1 创建一个Multi-Agent智能体[¶](#1-multi-agent)

首先，我们创建一个智能体，命名和介绍如下：

![img](https://i-blog.csdnimg.cn/direct/c9a0a01d95444a0a808b3b672820e775.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

在这里，我们需要切换到多Agents模式。

![img](https://i-blog.csdnimg.cn/direct/afa18ddc726346ef8b8c3d17526d1acc.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

可以看到，多Agent的执行过程有点类似于工作流，接下来我们查看一下在多Agents模式下支持哪些节点类型，添加“添加节点”

![img](https://i-blog.csdnimg.cn/direct/9a326bbead8d4ffcbe9a2171b64d1716.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

分为一下3类节点：

1. Agent：Agent 节点是可以独立执行任务的智能实体。默认情况下，智能体内添加了使用智能体名称的 Agent。功能相对比较简单，仅支持提示词和技能两个功能。没法实现类似工作流的功能，较难实现比较复杂的业务流程，因此更适合用在对话类的业务上。
2. 工作空间智能体：将已发布的、可以执行特定任务的单 Agent 智能体添加为节点。 我们可以把已经集成了各类插件、业务逻辑的复杂工作流的智能体作为其中一个Agent。在实际工作中，需要使用到多Agents的时候往往是功能非常复杂的时候， 将已经发布到工作空间的多个复杂功能Agent结合一个父Agent做路由，往往是更常见的做法。
3. 全局跳转条件：适用于所有 Agent 的全局条件。只要用户输入满足该节点的条件，则会立即跳转到 Agent。用于实现复杂Agent的流程控制。

### 2 拆分Agent和实现Agent[¶](#2-agentagent)

在思考如何规划和拆分Agent之前，我们先需要知道当前的业务场景是怎么样的，对于“多语言翻译”，它的业务逻辑如下： 用输入一段文本，并指定要翻译的目标语言，智能体最终返回用户要求的目标语言的译文，支持中文、汉语、日语三种。

案例业务逻辑比较简单，单LLM+提示词即可实现较好效果，达不到需要使用multi-agent的场景。这里不必纠结，我们的目的是基于这个案例，学会如何使用multi-agent功能。

我们可以通过父子结构拆分，父Agent负责分发翻译任务，每个语言的翻译都有一个对应的Agent进行翻译：

- 父Agent：分发翻译任务，将用户输入翻译为目标语言，
- 中文Agent：如果目标语言是中文，则调用此Agent实现翻译
- 韩语Agent：如果目标语言是韩语，则调用此Agent实现翻译
- 日语Agent：如果目标语言是日语，则调用此Agent实现翻译

如下图：

![img](https://i-blog.csdnimg.cn/direct/04a80aae3cca4b89ad2cacab612482d3.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

调试Agent，输入以下内容：

```
"Good morning" in Chinese
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

执行结果如下图：

![img](https://i-blog.csdnimg.cn/direct/91cdebc0c0824540875480060a651385.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑