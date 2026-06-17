# AI_16_Dify_智能法律助手

#  智能法律助手[¶](#31)

## 学习目标[¶](#_1)

- 了解法律助手业务背景
- 掌握对话流的使用
- 掌握dify插件的安装和数据库查询插件的使用
- 实现法律助手智能体

## 一、项目背景[¶](#_2)

合作多年的甲方公司“明德律师事务所”，期望在首页增加一个“法律问答助手”智能机器人，理想中的效果如下：

![img](https://i-blog.csdnimg.cn/direct/098baa27d47a4cd681b30d7dd2d514af.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

期望通过机器人对用户提供：

- 法律条文查询：因事务所职责范围在民事诉讼、企业法律顾问、知识产权等业务，需要支持相关法律条文原文的查询。
- 案情分析：通过用户简要描述的案情，
- 合同审核：上传合同文件，审核对应内容
- 律师费用查询：查询律所律师费用

等功能，增加用户的粘性，从而间接提高转化率。

## 二、技术设计[¶](#_3)

### 1 业务流程[¶](#1)

![img](https://i-blog.csdnimg.cn/direct/d8def9966d1b41ad8bddb2255bf535b5.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

### 2 技术选型[¶](#2)

考虑到资费数据在本地存储业务数据库，以及用户的网站部署在本地，而用户的需求又是把对话机器人嵌入到主页。所以：

- Saas版Coze：无法访问本地业务数据库，不支持对话机器人嵌入到主页，排除
- langgraph：考虑到用户项目的复杂度和预算，使用langgraph性价比较低.
- Dify：可以满足需求

## 三、模块介绍[¶](#_4)

分为以下几个模块：

1. 法律咨询：用户通过和机器人对话，实现法律条文的查询
2. 案件分析：支持用户通过文字描述或者上传文件的方式，基于法律条文分析案件
3. 合同分析：支持合同分析，能够根据用户上传的合同附近判断是否存在风险点
4. 资费查询：咨询和计算律所的律师费价格区间

## 四、具体实现[¶](#_5)

因项目的业务复杂度较低，我们这里直接使用一个“对话流”实现，如下图：

![img](https://i-blog.csdnimg.cn/direct/29e5754b08264e5cb72a147d898a75e2.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

整体入口在“开始”节点，然后通过意图识别节点进行场景的划分：

![img](https://i-blog.csdnimg.cn/direct/b68c30a2424c4c33bb9192e0c344cf6b.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

划分到不同的场景后，进到各自模块的处理逻辑中。

在整个过程中，我们需要构建一个知识库。 我们选择在RAGFlow中构建，过程略：

![img](https://i-blog.csdnimg.cn/direct/eeb26e8f2b6244ac8f8358efe7bf8a34.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

在dify中引入知识库：

![img](https://i-blog.csdnimg.cn/direct/18910aa5f35c4181b675369cf3a9d0fa.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

测试连通性：

![img](https://i-blog.csdnimg.cn/direct/e2efe8d888734d878cf5795a4b9e9469.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

### 1 法律咨询模块[¶](#1_1)

对于法律咨询模块，具体流程如下：

![img](https://i-blog.csdnimg.cn/direct/47378e2cb91245648d2ee79a9c20b2cc.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

#### 1.1 查询改写[¶](#11)

查询改写这一步的目的是为了让用户输入的自然语言更便于进行知识库查询，以便能有更好的效果， 如下：

![img](https://i-blog.csdnimg.cn/direct/6beea82f494f411ab18e465b1435f102.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

系统提示词：

```
你是一个法律条文查询助手，能够根据用户的提问进行优化，转化成更适合查询知识库的问题
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

用户提示词：

```
用户输入：
{{#sys.query#}}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

这里可以开启“记忆”，让查询改写节点拥有更多的上下文，从而生成更好的query。

#### 1.2 法律条文检索[¶](#12)

查询知识库，得到法律条文：

![img](https://i-blog.csdnimg.cn/direct/e5a9fe3c66dd4b9eb73bb724f9acc3f1.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

因为我们这里的场景就是查询法律条文原文并返回给用户，所以不宜返回太多内容。所以在这里我们开启rerank

![img](https://i-blog.csdnimg.cn/direct/34760f0a32604272bf67bbf5abdaf8ff.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

#### 1.3 检索结果优化[¶](#13)

拿到了知识库的内容以后，就可以结合用户的问题进行结果优化了。

![img](https://i-blog.csdnimg.cn/direct/e96263e2e6ee494eb04eb5365aa19bbd.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

系统提示词：

```
你是一个法律咨询助手，能够根据用户的提问和从知识库里查询出来的结果回答用户。需要注意，回答用户问题的时候，先直接给出结论，再给出对应的法律原文依据
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

用户提示词：

```
用户的问题：
{{#sys.query#}}
知识库：
{{#context#}}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

在这里我们可以开启记忆，让效果更好。

#### 1.4 输出法律条文[¶](#14)

![img](https://i-blog.csdnimg.cn/direct/bc5b9402b7e34deb893b3e20043eb2fa.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

具体内容：

```
{{#1763643919240.text#}}

以上分析内容来自律所智能大脑，如果您有进一步的需求，建议您联系律所获取专业的咨询建议，电话：400-33334444
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

### 2 合同分析模块[¶](#2_1)

![img](https://i-blog.csdnimg.cn/direct/418255cbbe6e44bab87e52190b9c2530.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

#### 2.1 合同内容读取[¶](#21)

通过文件读取案件内容

![img](https://i-blog.csdnimg.cn/direct/9f78e0151981438db3beb248c0a3833c.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

#### 2.2 法律条文查询提取[¶](#22)

![img](https://i-blog.csdnimg.cn/direct/ede409b529204bb290ec4e90b6db658b.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

系统提示词:

```
你是一个法律专家，能够根据用户上传的合同内容拆分出对应的法律条文对应的查询条件，接下来我将进行知识库的查询。
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

用户提示词:

```
用户当前输入：
{{#sys.query#}}
合同：
{{#1763389349959.text#}}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

#### 2.3 法律条文检索[¶](#23)

![img](https://i-blog.csdnimg.cn/direct/d508496470644792aca48910927c10e0.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

同样的，召回10条：

![img](https://i-blog.csdnimg.cn/direct/90b294855637403eb3260fbf7c976d72.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

#### 2.4 合同分类[¶](#24)

因为不同的合同关注的点不同，所以在这里，我们需要划分不同的场景。

因为律所擅长的就是劳动合同和技术服务类的，所以这两类单独处理。

![img](https://i-blog.csdnimg.cn/direct/46f8f9dcf8bf409785e3a02f56be4828.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

#### 2.5 合同审核[¶](#25)

![img](https://i-blog.csdnimg.cn/direct/f6d97abb64b14a06aeec020ac19f5eb9.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

对于系统提示词，每个合同审核各不相同：

劳动合同审核:

```
# 角色
你是一名资深劳动法律师，专注于帮助企业审查劳动合同的合规性，规避用工法律风险。

# 审查背景
- **合同类型**：劳动合同
- **我方立场**：[用人单位/劳动者]
- **核心诉求**：[例如：确保合同完全合法，避免劳动争议；或，确保劳动者核心权益得到保障]

# 指令与工作流
1.  **首要步骤**：首先，核对合同是否包含《劳动合同法》第十七条规定的所有必备条款。
2.  **重点审查**：针对以下关键领域进行深入分析，并依据最新《劳动合同法》及相关司法解释给出判断：
    -   **试用期**：期限是否符合第十九条上限规定，工资是否不低于第八十条的标准。
    -   **工作地点与岗位调整**：条款是否过于宽泛，是否赋予了单位单方面无限调整权。
    -   **劳动报酬**：是否明确规定了工资构成、支付时间、加班费计算基数。
    -   **解除条件**：约定的解除条件是否合法，经济补偿金（N）、赔偿金（2N）的计算方式是否清晰。
    -   **保密与竞业限制**：竞业限制补偿金是否有明确约定，标准是否合理。
3.  **缺失项分析**：明确指出缺失的必备条款及其潜在法律风险。

# 输出格式
请按以下结构提供**结构化审查报告**：
-   **一、总体评价**：对合同的合法性、公平性给出初步结论。
-   **二、分项分析与修改建议**：以表格形式列出，包含“问题条款”、“风险等级（高/中/低）”、“法律依据”、“风险分析”、“修改建议文本”。
-   **三、核心风险摘要**：总结最重要的3-5个风险点。
-   **四、谈判策略建议**：根据我方立场，提供谈判要点。

# 约束条件
-   严禁编造不存在的法条。所有判断必须有明确的法律依据，并注明出处，例如“根据《劳动合同法》第XX条”。 
-   若某些条款的合法性存在普遍争议，应予以提示，并建议“此事项建议咨询执业律师以获取最终意见”。
-   以下文提供的法律条文为主
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

技术服务合同审核:

```
# 角色
你是一家科技公司的法务顾问，负责审查技术服务协议，以控制风险、保障项目顺利交付。

# 审查背景
- **合同类型**：技术服务协议
- **我方立场**：[服务委托方/服务提供方]
- **交易背景**：[简要描述项目内容与合作背景]

# 指令与工作流
1.  **范围与交付物**：审查“工作范围”（SOW）和“交付物”是否描述得清晰、具体、可量化、可验证。
2.  **知识产权条款**：这是审查的重中之重。必须明确约定：
    -   背景知识产权和项目执行中产生的知识产权（前景知识产权）的归属。
    -   建议的归属原则（例如，委托方付费开发的，前景知识产权归委托方所有）。
    -   双方的使用许可范围、期限和地域[6](@ref)。
3.  **付款条件**：审查付款节点是否与项目关键里程碑（如需求评审、初验报告、终验报告）强关联，以控制付款风险。
4.  **保密与合规**：审查保密信息的定义是否合理，保密期限是否明确，是否涉及数据安全与个人信息保护的合规要求。
5.  **违约责任与责任限制**：分析违约责任条款是否对等，责任上限（如最高不超过合同总价款的XX%）是否合理。

# 输出格式
请按以下结构提供**结构化审查报告**：
-   **一、总体风险评估**：概括合同的主要风险和友好度。
-   **二、关键条款深度剖析**：重点针对“知识产权”、“付款方式”、“验收标准”、“违约责任”等条款，分析其潜在风险、商业合理性和修改建议。
-   **三、谈判优先级清单**：将风险分为“必须修改”、“建议修改”和“可接受”三类，并提供谈判话术。

# 约束条件
-   分析应基于《民法典》合同编及相关司法解释，但更应侧重商业实践的合理性与风险控制。
-   提供的修改建议应具可操作性，并给出修改后的范例文本。
-   以下文提供的法律条文为主
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

通用合同审核:

```
# 角色
你是一名擅长设计风险隔离方案的法律风险控制专家。

# 审查背景
- **协议类型**：兜底协议
- **我方立场**：[委托方(甲方)/兜底方(乙方)]
- **兜底背景**：[简述需要兜底的业务、事项及存在的核心风险]

# 指令与工作流
1.  **明确兜底范围**：审查“兜底事项”的描述是否绝对清晰、无歧义。避免使用概括性语言，应尽可能列举具体情形
2.  **触发条件**：审查兜底责任触发的条件是否明确、客观、可验证（如“当XX事件发生且经双方书面确认后”）。
3.  **责任限度**：审查兜底方承担的责任是连带责任还是一般补充责任？责任金额是否有上限（如“以XX为限”）？
4.  **权利义务对等**：审查协议是否赋予了兜底方相应的核查权、知情权以及履行兜底责任后的追偿权。
5.  **效力与冲突**：判断本协议的效力，以及当本协议与主合同约定冲突时，以何者为准。

# 输出格式
请按以下结构提供**结构化分析报告**：
-   **一、协议效力评估**：分析本协议在法律上的有效性和可执行性。
-   **二、核心条款风险提示**：逐条分析“兜底事项”、“触发条件”、“责任限制”等条款存在的模糊之处或潜在漏洞。
-   **三、修订建议与文本**：对高风险条款提供具体的修改建议和改写后的文本。

# 约束条件
-   重点提示兜底方可能面临的无限责任风险。
-   强调协议的明确性，避免因约定不明被视为无效。
-   以下文提供的法律条文为主
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

用户提示词，则都是一致的：

```
用户的案情：
{{#1763389349959.text#}}
用户提供的输入：
{{#sys.query#}}
法律条文：
{{#context#}}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

#### 2.6 输出审核结果[¶](#26)

![img](https://i-blog.csdnimg.cn/direct/a26bbfb18a66434a90e48ac8a438bac5.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

内容如下:

```
{{#llm.text#}}{{#17633898771260.text#}}{{#1763396670831.text#}}

以上分析内容来自律所智能大脑，建议您联系律所获取专业的咨询建议，电话：400-33334444
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

### 3 案件分析模块[¶](#3)

整体流程如下：

![img](https://i-blog.csdnimg.cn/direct/cf93dbb84db047e0bf4f00081b91cbd0.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

#### 3.1 案件内容读取[¶](#31_1)

首先判断用户是否是通过文档的形式上传的案件内容，基于文档提取器，获取用户上传的文档

![img](https://i-blog.csdnimg.cn/direct/491075bed7ec48bdb6c2d09a57a16489.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

#### 3.2 代码节点[¶](#32)

代码节点合并案件内容和用户需求：

![img](https://i-blog.csdnimg.cn/direct/884b49c096684ecf891485fad20810d3.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

代码实现：

```
ef main(text: str, query: str):
    if text:
        return {
            "result": text + query,
        }
    return {
        "result": query,
    }
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

#### 2.2 法律条文查询提取[¶](#22_1)

通过大模型，判断案件，获取查询法律条文的query

![img](https://i-blog.csdnimg.cn/direct/11b5569320ec4a8ca439e1a6be59729c.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

系统提示词：

```
你是一个法律专家，能够根据用户上传的案例内容和用户的输入拆分出对应的法律条文对应的查询条件，接下来我将进行知识库的查询。
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

用户输入：

```
用户输入：
{{#sys.query#}}
用户上传的案例：
{{#17639915148840.text#}}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

#### 2.3 法律条文检索[¶](#23_1)

通过知识库检索节点，进行法律条文检索：

![img](https://i-blog.csdnimg.cn/direct/f65ff94a398f409f821b5c30673f1737.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

这里可供参考的内容越多越好：

![img](https://i-blog.csdnimg.cn/direct/5bf554b92bdd4b65becdba28ec381996.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

#### 2.4 案情分析[¶](#24_1)

基于大模型+上下文，分析案情：

![img](https://i-blog.csdnimg.cn/direct/cf1c07d36d654131beb96408d36058d7.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

系统提示词：

```
你是一名专业的案情分析专家，名为“案理”。请遵循以下核心原则处理用户输入的案情：
一、核心定位
∙
角色：专业、中立、严谨的法律分析助手，专注于梳理案件事实、识别争议焦点、结合法律条文进行分析。
∙
目标：为用户提供结构化、有逻辑、有依据的案情分析报告，辅助决策。
∙
底线：
∙
所有分析需严格基于用户提供的案件材料和法律条文，不臆断或虚构。
∙
不替代律师提供正式法律意见，不预测判决结果。
∙
如信息不足或存在矛盾，需明确说明局限性。
二、输入与输出规范
1.
输入来源：
∙
用户查询：用户的具体分析需求（例如“分析本案的争议焦点”）。
∙
案例材料：用户上传的案件全文（如判决书、事实描述等）。
∙
参考法律条文：用户提供的相关法条或背景信息。
2.
输出结构（按此顺序组织报告）：
∙
1. 案件基本信息：提炼当事人、案由、时间、核心事实等关键要素。
∙
2. 争议焦点：归纳案件的核心争议点（如法律适用、证据有效性等）。
∙
3. 事实与证据分析：梳理法律事实，评估证据链的完整性。
∙
4. 法律适用分析：结合法律条文，分析行为性质、法律责任及可能后果。
∙
5. 综合结论与建议：总结分析意见，并提出后续行动方向（如取证重点、诉讼策略等）。
三、分析与表达要求
∙
语言风格：使用专业、清晰的法律语言，避免“我认为”等主观表述。关键结论可加粗突出。
∙
引用规范：法律条文需注明名称和条款序号，分析需注明依据（如“根据案件材料第X段……”）。
∙
风险提示：如发现信息矛盾、法律依据不足或案件存在重大风险，需单独说明。
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

用户提示词：

```
用户输入：
{{#sys.query#}}
用户上传的案例：
{{#17639915148840.text#}}

参考法律条文：
{{#context#}}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

开启记忆能力，提升检索效果：

![img](https://i-blog.csdnimg.cn/direct/1b50e23c2035440d8afca2543bf33b33.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

#### 2.5 输出案情分析结果[¶](#25_1)

输出案情分析的结果：

![img](https://i-blog.csdnimg.cn/direct/3957a567f80e471c9c09391f81641877.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

内容如下：

```
{{#1763994871711.text#}}

以上分析结果来自律所智能大脑，仅供参考，建议您联系律所获取专业的咨询建议，电话：400-33334444
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

### 4 资费查询模块[¶](#4)

一些前置操作:

首先，需要安装数据库查询插件，如下：

![img](https://i-blog.csdnimg.cn/direct/383a3625285042148ab117f7496a19ea.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

我们这里通过手动建表的方式模拟业务数据库。准备好mysql数据库实例，并运行建表语句

```
CREATE DATABASE law_assistant;
USE law_assistant;

CREATE TABLE legal_service_pricing (
    service_id INT AUTO_INCREMENT PRIMARY KEY,
    service_name VARCHAR(255) NOT NULL,
    service_description TEXT,
    price DECIMAL(10, 2) NOT NULL,
    billing_unit ENUM('按小时', '按次', '按项目') NOT NULL,
    lawyer_id INT NOT NULL,
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

插入价格数据：

```
INSERT INTO legal_service_pricing (service_name, service_description, price, billing_unit, lawyer_id) VALUES
('初次法律咨询', '针对一般法律问题的初步分析和建议（每小时）', 800.00, '按小时', 101),
('合同审查（标准）', '对标准商业合同进行合规性与风险审查（按次）', 2500.00, '按次', 102),
('民事诉讼一审代理', '代理普通民事纠纷案件的一审程序（全案）', 30000.00, '按项目', 105),
('商标注册申请', '国内商标查询、材料准备及提交代理服务', 3800.00, '按项目', 104),
('定制协议起草', '根据需求起草专项法律协议（每小时）', 1200.00, '按小时', 101),
('公司股权激励方案设计', '设计并制定员工股权激励计划方案', 25000.00, '按项目', 106),
('律师函出具', '就特定事务起草并发出正式律师函', 1500.00, '按次', 103),
('劳动争议仲裁代理', '代理用人单位或员工参与劳动仲裁程序', 12000.00, '按项目', 107),
('刑事案件侦查阶段辩护', '在公安侦查阶段为犯罪嫌疑人提供辩护', 20000.00, '按项目', 108),
('尽职调查（基础）', '针对中小规模企业的基础法律尽职调查', 18000.00, '按项目', 106),
('个人遗嘱起草与公证', '协助起草个人遗嘱并提供公证指引', 1000.00, '按次', 109),
('知识产权侵权维权', '针对商标、专利等侵权行为的初步维权措施', 9000.00, '按项目', 104),
('债务催收服务', '通过非诉讼方式进行商业债务催收', 4500.00, '按项目', 110),
('法律顾问服务（月度）', '提供月度日常法律咨询及简单文件审阅', 5000.00, '按小时', 102),
('离婚协议协商与起草', '协助协商并起草离婚相关法律协议', 6000.00, '按项目', 109),
('数据合规方案辅导', '帮助企业初步建立数据合规框架', 15000.00, '按项目', 111),
('上诉状起草', '针对一审判决起草民事上诉状', 4000.00, '按次', 105),
('商业秘密保护制度构建', '协助企业制定内部商业秘密保护制度', 22000.00, '按项目', 111),
('房产买卖合约审阅', '审阅商品房或二手房买卖相关合同', 2000.00, '按次', 112),
('投资条款清单（Term Sheet）分析', '对投资条款清单的关键风险点提供分析意见', 3500.00, '按次', 106);
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

#### 4.1 运行数据库查询[¶](#41)

设置如下：

![img](https://i-blog.csdnimg.cn/direct/d984efce2fd340099ea82c43bfade2a9.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

数据库属性：

```
charset=utf8mb4&collation=utf8mb4_0900_ai_ci
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

SQL查询语句：

```
select * from legal_service_pricing where 1=1
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

#### 4.2 资费计算[¶](#42)

资费计算，将业务数据库查询出来的定价内容部作为上下文，让大模型自行计算资费

![img](https://i-blog.csdnimg.cn/direct/407adbf19fcc49de89a0b472bdaa6156.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

系统提示词:

```
你是一名专业的法律助手，负责律所的价格咨询业务，能够根据用户输入的问题，结合律所定价细则，给出一个报价区间
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

用户提示词：

```
用户问题：
{{#sys.query#}}

定价：
{{#context#}}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

#### 4.3 输出报价[¶](#43)

![img](https://i-blog.csdnimg.cn/direct/f62518f577164a4e9c334d6112748f62.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

具体内容:

```
{{#1763996314582.text#}}

具体报价请联系人工客服，电话：400-33334444
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

### 5 编排和部署[¶](#5)

#### 5.1 设置开场白[¶](#51)

![img](https://i-blog.csdnimg.cn/direct/2b885fc71be540ffb2fed700258f4b61.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

开场白：

```
你好，我是“明德同学”智能助手，借助律所的法律智能大脑打造。在这里您可以让我咨询法律法规、 进行案情分析、分析合同，也可以咨询律所的资费。有什么不懂的，请致电：400-33334444
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

#### 5.2 将聊天机器人嵌入前端[¶](#52)

物料中的：index.html，就是我们“明德律师事务所”的前端页面。 在实际工作中，它以前端代码的方式存在于服务器上进行部署，我们这里为简化流程， 使用本地html文件模拟。

首先，对于我们已经发布的工作流，点击左侧按钮，按照下图依次操作，得到嵌入的js代码：

![img](https://i-blog.csdnimg.cn/direct/371a9ddae1fb4dc69cf8301c693ff695.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

打开index.html的源码，并粘贴到对应位置：

![img](https://i-blog.csdnimg.cn/direct/baa0fac9ce3545ab862e6ecd4107752b.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

需要注意，这里的颜色：#2c5282，我们是从html前面的颜色设置中得到的，非默认值：

![img](https://i-blog.csdnimg.cn/direct/f3ccc5b5e19249b5963fa3bf30ddd9f0.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

这样做的目的是让机器人图标的颜色更好的融入到页面中。

### 五、运行项目[¶](#_6)

打开网页，在右下角的机器人中输入问题后，效果如下：

![img](https://i-blog.csdnimg.cn/direct/0b93745576eb44e08944e4af666c90a8.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑