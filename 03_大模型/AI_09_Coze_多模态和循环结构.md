## 学习目标[¶](#_1)

- 掌握知识库的创建和使用
- 了解知识库检索相关的核心原理
- 掌握数据库的创建和使用
- 掌握在工作流中使用数据库进行增删改查

## 一、概述[¶](#_2)

在前面的内容中，我们已经学习了如何使用工作流搭建一个功能复杂的agent，以及工作流调用插件 ，使工作流具备处理文件、联网搜索等能力。但是在某些业务场景下，我们还需要让agent具备访问私有数据库的能力：

- 比如，我们有一些医疗知识文档，想通过和agent对话的方式，直接去询问医疗知识文档中的知识，这需要让agent具备访问”知识库“的能力；再比如，我想让agent作为一个签到代理，能够记录和更新用户的签到信息，同时我还可以查询有哪些用户已经签到，这需要让agent具备读写”数据库“的能力。

什么时候使用知识库，什么时候使用数据库：

- 知识库：知识库可以理解为是一个支持快速检索的文件夹，里面存放的都是一些文档，类比word文档、pdf文档等。适合存储文字内容
- 数据库：数据库可以理解为是一张巨大的表格，里面存放的是一些结构化的数据，类比excel表格数据。

## 二、 在工作流中使用知识库[¶](#_3)

知识库功能包含两个能力，一是存储和管理外部数据的能力，二是增强检索的能力。

- 数据管理与存储
- 扣子开发平台支持从多种数据源渠道上传文本和表格数据，例如本地文档、在线数据、Notion、飞书文档等。
- 上传后，扣子可将知识内容自动切分为一个个内容片段进行存储，同时支持用户自定义内容分片规则，例如通过分段标识符、字符长度等方式进行内容分割。
- 增强检索：
- 扣子开发平台的知识功能还提供了多种检索方式来对存储的内容片段进行检索，例如使用全文检索通过关键词进行内容片段检索和召回。
- 大模型会根据召回的内容片段生成最终的回复内容。

### 1 创建一个知识库[¶](#1)

#### 1.1 新建一个知识库资源[¶](#11)

在资源库页面点击右上角的”+资源“，选择知识库

![img](https://i-blog.csdnimg.cn/direct/d24c8f68f0e34cf3bea846bd99985423.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

然后，选择”创建扣子知识库“：

![img](https://i-blog.csdnimg.cn/direct/f72387517eab4f55985bfc2ebd9e16c8.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

扣子支持使用扣子知识库和火山知识库，两者对应的分类方式不同。

使用知识库功能的第一步就是上传知识内容。上传知识内容又分为两步，首先选择要上传的知识类型和上传方式，然后对上传的内容进行切分。合理的内容分片可以提升召回内容的相关性，从而提升大模型回复问题的准确性。

在上传知识前，建议先了解不同的知识类型的使用场景和导入方式，以便更好地管理知识内容。

**扣子知识库：** 适用于轻量检索，低代码，轻量级应用的场景

**火山知识库：** 适用于企业用户，以及大规模的查询场景。

最后，选择“文本格式”以及“本地文档”。

![img](https://i-blog.csdnimg.cn/direct/2d5978ed87d041799d4b4d2fa8f3ae40.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

#### 1.2 解析文件和分段[¶](#12)

将物料 **03-大模型发展史.docx** 上传到知识库：

![img](https://i-blog.csdnimg.cn/direct/98dcb383f55241478535c5d4003f4565.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

点击下一步，进到“创建设置”页面：

![img](https://i-blog.csdnimg.cn/direct/809a528884c34ebeb313e2e4ce0fe172.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

支持精准解析和快速解析：

- **精准解析**：支持从文档中提取图片元素、扫描件（OCR）、表格元素；支持设置过滤策略，以文档页的粒度过滤掉当前文档中不需要导入的内容。精准解析需要耗时更长的时间。
- **快速解析**：不支持从文档中提取图像、表格等元素，适用于纯文本。

上传本地文档时，支持以自动分段与清洗、自定义分段和层级分段这三种方式对文本内容进行分段处理。

![img](https://i-blog.csdnimg.cn/direct/57d16f306fb349ce8c1ce9ea3fb47600.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

然后是配置存储，这里我们使用平台共享存储即可。如果应用对性能要求比较高，可以再火山引擎上购买独立的存储服务，使用云搜索服务。

![img](https://i-blog.csdnimg.cn/direct/f356e4b62254475d9c48d90cdd9d05fc.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

最后需要给同学们强调的是：文档分段的好坏直接影响检索的效果，如何选择分段的策略非常重要。但因这部分内容原理比较复杂，不适宜在coze阶段给同学们深度讲解，所以这部分知识作为拓展内容，同学们可根据个人的情况自行学习，详见 **1.3 拓展：分段详细解读部分**。 **如觉得晦涩难懂，可在学习完RAG阶段后复习时再来学习。**

#### 1.3 分段预览和数据处理[¶](#13)

**分段预览：**

- 在使用不同的解析方式时，会有分段预览这个阶段，让用户查看分段以后的效果大概是什么样的，以便确认分段是否符合预期。如果不符合预期，可以选择上一步，重新选择。需要注意的是，在这一步处理的时候，数据并没有存储到知识库中，当确认无误，点击下一步时，才会执行数据处理

![img](https://i-blog.csdnimg.cn/direct/3123ca64daf44503a626d04d358ae458.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

**数据处理：**

点击下一步后，等待服务器处理完成数据。点击确认后，知识库就完成了数据写入。

![img](https://i-blog.csdnimg.cn/direct/afc9ad562f804d1b8a646ae3ceb91b5f.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

将文档写到知识库以后，我们可以看到左侧的文档列表，代表已经有哪些文档被写到了里面，它和上传时的文件名保持一致。点击文档名以后，可以看到具体的分段的信息。 这里如何分段，

![img](https://i-blog.csdnimg.cn/direct/72341459127040f28e0d8fda827e14c9.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

### 2 在工作流中使用知识库[¶](#2)

在前面的课程中，我们已经建好了知识库，并且把数据导入到了知识库中，接下来我们将在工作流中使用我们已经建好的知识库，完成一个知识库检索的demo。

#### 2.1 创建一个知识库检索节点[¶](#21_1)

首先，我们创建一个新的工作流，并添加一个“知识库检索”节点：

![img](https://i-blog.csdnimg.cn/direct/2fc594ea65b84c55b4379973fa1b3224.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

给知识库检索节点赋值， 把开始节点的输入传如给知识库检索， 目标知识库选择我们前面刚建好的

- 输入：只有一个query字段，且必须是str类型。 就是要去知识库检索时的查询，执行检索时，这个query会被和知识库中已有的文档段落进行匹配，并返回给用户匹配度最高的（1-20条，默认1条）
- 输出：outputList，数组类型，匹配到几个文档，就返回几条数据![img](https://i-blog.csdnimg.cn/direct/c0504041cd964a13a6ae06878cb893f3.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

细心的同学可能会注意到检索策略下面会有一堆参数，如下图。这些参数是知识库检索的核心参数，除了分段的策略以外，这些参数同样影响着知识库检索的效果，这里我们先使用默认参数，稍后将给同学们介绍参数的原理和如何设置。![img](https://i-blog.csdnimg.cn/direct/e37530ef9a7847ceb529a3ddd733373d.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

#### 2.2 使用大模型润色检索结果和兜底[¶](#22)

有时候我们检索知识库的时候结果可能是空的，或者检索出来一些无关的段落，以及话术比较生硬等情况。所以我们在工作流中使用知识库时，不会直接把知识库检索出来的结果返回给用户，而是使用大模型做一些润色和兜底的工作，再把结果给到用户。结合知识库创建，到此位置，这就是一个简单的RAG的流程。如果是在做一个轻量级的agent，就可以考虑使用这种方式，快速实现。

接下来，我们在工作流中添加一个LLM节点，把query、查询结果都给到它，并赋予联网搜索能力

![img](https://i-blog.csdnimg.cn/direct/eacf51987e684f2594d959b9b77a09b1.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

prompt如下：

```
你是一个知识库查询结果润色和兜底的助手，能够根据用户输入的问题对查询结果做润色。也可以在检索结果为空时，负责调用搜索插件，在互联网检索相关内容并返回给用户。  你的职责如下：
1.  如果检索结果不为空，根据用户输入的问题进行优化，并使用比较大方和专业的话术回复用户。这个过程不需要联网搜索。
2. 如果检索结果为空，则调用工具，在互联网检索对应的内容，并根据用户输入的问题进行优化，并使用比较大方和专业的话术回复用户
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

输入query:

```
什么是深度学习
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

调用大模型过程略，结果如下：

知识库检索结果：

![img](https://i-blog.csdnimg.cn/direct/598f551d17274224b81067cb1807bbf5.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

润色以后的结果：

![img](https://i-blog.csdnimg.cn/direct/cef48078212848ad84166d8d761d6f8a.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

可以看到，经过大模型的润色，结果的可读性更好，更专业。

## 三、在工作流中使用数据库[¶](#_4)

扣子的数据库功能适用于组织和管理结构化数据，例如客户信息、产品列表、订单记录等。目前，扣子支持使用扣子官方数据库和火山数据库。

接下来，我们将以一个“绩效录入和查询agent”的一个简单的demo，帮助同学们理解怎么在工作流中使用数据库。 这个agent支持绩效数据的查询、修改、增加、删除，我们将通过意图识别节点，实现这个agent的功能。

### 1 创建一个数据库[¶](#1_1)

在资源库中创建一个数据库，如下图：

![img](https://i-blog.csdnimg.cn/direct/45c4a123e2e84181b7166ea49f1e1975.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

对比如下：

- 扣子数据库：扣子官方数据库，提供了类似传统软件开发中数据库的功能，允许用户以表格结构存储数据。
- 火山数据库：火山数据库是指[云数据库 MySQL 版](https://www.volcengine.com/docs/6313/74502)，它是火山引擎基于开源数据库 MySQL 打造的弹性、可靠的在线关系型数据库服务。MySQL 实例使用云原生方式部署，结合本地 SSD 存储类型，提供高性能读写能力；完全兼容 MySQL 引擎，并提供实例管理、备份恢复、日志管理、监控告警、数据迁移等全套解决方案，帮助企业简化繁杂的数据库管理和运维任务，使企业有更多的时间与资源聚焦于自己的核心业务。

在这里我们使用扣子数据库。对于使用coze大部分场景来讲，扣子数据库足够应对。

![img](https://i-blog.csdnimg.cn/direct/d8ff98e96f814f15a6479d153e5e4efe.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

#### 1.1 设置表结构[¶](#11_1)

创建数据库表以后，选择单用户模式（默认选项），这里需要注意：

- 单用户模式：开发者和用户都可以添加记录，但仅能读/修改/删除自己创建的来自同渠道的数据。
- 多用户模式：开发者和用户都可读/写/修改/删除表中来自同渠道的任何数据，由业务逻辑控制读写权限。

![img](https://i-blog.csdnimg.cn/direct/f47691bea0ee4511bb01d8cf0eb67e42.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑



新建一个数据表，结构如下图：

![img](https://i-blog.csdnimg.cn/direct/d31f051203a341d8be910e2aa9ca2b09.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

这里需要注意：id、sys_platform、uuid、bstudio_create_time 字段为系统内置。work_code、name、performance_level 字段为我们这个案例中所使用的字段，分别代表工号、人名、绩效等级。

点击保存按钮，就完成表的创建。

#### 1.2 导入数据[¶](#12_1)

有了表结构以后，接下来我们导入数据。在 测试数据中，点击批量导入，并把我们的素材 **04-绩效数据.xlsx** 文件上传。这里需要注意，数据库分测试数据和线上数据两个环境，这两个环境相互隔离：

- 测试数据：用于在发布之前调试使用， 和线上数据互相不影响
- 线上数据：发布后，从应用商店、api等各种渠道调用时使用。和测试数据相互隔离

![img](https://i-blog.csdnimg.cn/direct/fb1bfd10cde74e27bdb9eaf4516146a4.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

配置表结构，表头选择第1行、数据起始行选择第2行

![img](https://i-blog.csdnimg.cn/direct/de56620c686347ad86bcdba144bed5da.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑对应原文件：

![img](https://i-blog.csdnimg.cn/direct/a6f91eef606e4facaedc3fb4ffbcd2dc.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

预览数据，无误后点击下一步即可。![img](https://i-blog.csdnimg.cn/direct/02aee51074984dbd94b4b4d0b7f7f450.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

确认数据已经完全写入：

![img](https://i-blog.csdnimg.cn/direct/6f334acfc2b8415cb38bdd02e647a5f8.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑![img](https://i-blog.csdnimg.cn/direct/8727686420fc4eb29fcaccf11452b212.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑![img](https://i-blog.csdnimg.cn/direct/bf9f5da04e59428eb33c97257012d915.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

###  在工作流中使用数据库[¶](#2_1)

接下来，我们新建工作流，然后新建一个意图识别节点，如下图：

![img](https://i-blog.csdnimg.cn/direct/c468638774bf4a1a974f3caf2e9a0416.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

分为以下几个意图：

- 查询数据
- 查询某个人的绩效数据
- 新增数据
- 新增某个人的绩效数据
- 更新数据
- 更新某个人的绩效数据
- 删除数据
- 删除某个人的绩效数据

整体业务流程如下图：

![img](https://i-blog.csdnimg.cn/direct/b98274c8aca14d83856d4dfb23a1036a.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

#### 2.1 使用LLM提取命名实体[¶](#21-llm)

在实现增删改查数据之前，我们需要先研究一下数据库节点的特性，首先数据库节点分为增删改查和自定义SQL5种类型。因为我们还没有学过SQL，所以这种使用方法我们先不关注，大家知道支持这种用法即可。

![img](https://i-blog.csdnimg.cn/direct/0cbc19d5b8c34fd18cd1dc4ef2c17e86.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

查看更新数据节点，如下图，在这里我们可以发现一个问题，我们查询数据需要根据： 字段名 = 某个值，这种方式去实现查询数据。但是实际用户和agent交互时往往说的是自然语言，比如：

> 用户输入： 帮我查一下张三的绩效是多少
>
> 或者 ：帮我查一下绩效，张三的

这里就会涉及到一个问题， 帮我查一下张三的绩效是多少 -> name = '张三' 这一步怎么转化，也就是说，我**们需要把“帮我查一下张三的绩效是多少”这句话以及相同语义不同表达方式的语句中的人名提取出来**。 这一步如何完成？

![img](https://i-blog.csdnimg.cn/direct/1189b99c17f147cda09c1bda354f24d9.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

这种场景在生活中其实非常常见，比如我们在快递程序中输入一串人名、地址、手机号的一段话，点击智能识别，快递程序就可以帮助我们把这些“实体”进行解析，并填到对应的表格中。 这里就涉及到了一种自然语言处理算法中常见的方法，叫做“命名实体识别”，也可以叫做“实体提取”，这里我们了解就行，后续会有其他课程进行深入讲解。

在这里，我们可以使用大模型的能力实现这个功能。比如我们要从用户输入的input中提取人名，执行以下两步：

- 编写提示词： 说明要抽取人名实体，当返回结果只有一个字段时，不需要指定变量名；多个字段时，需要说明哪个实体对应哪个变量名。
- 指定输出：在输出的变量中，指定变量名

![img](https://i-blog.csdnimg.cn/direct/24a2a648912849be937fe2f4fd174d3c.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

![img](https://i-blog.csdnimg.cn/direct/628fb903fe1f46c7bb076e0b5df5aa11.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

#### 2.2 查询数据[¶](#22_1)

查询某个人的绩效数据，需要添加一个人名提取节点，和一个数据库查询节点 ， 这两个节点和上个知识点中的是一致的，不做赘述。为了结果可读性更好，还需增加一个结果整理的节点，整体如下图：

![img](https://i-blog.csdnimg.cn/direct/8b52771ad99f40ae994fcac1d1ba391b.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

结果整理提示词：

```
查询结果整理助手，能够根据用输入的问题和数据库中查询到的结果进行整合，并给到用户一个专业、得体的答复。
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

输入提示词

```
查询王大锤的绩效
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

试运行后，结果如下：

![img](https://i-blog.csdnimg.cn/direct/7d5a7fef31e347f58f2590820380a9c9.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

#### 2.3 新增数据[¶](#23)

对于新增数据，我们需要把用户输入的内容中人名、工号、绩效3个字段全部提取出来，然后再通过新增数据节点，把数据更新到数据库中。整体流程如下

![img](https://i-blog.csdnimg.cn/direct/e5a94eaae04645199537ba03601048b0.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

如果我们要通过LLM同时提取多个实体，则需要这样操作：

![img](https://i-blog.csdnimg.cn/direct/9d110e72d8b948658034acf7e0ec81c7.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

对应的提示词如下：

```
你是一个人人名提取助手，能够根据用户输入的内容，提取出来里面的人名{name}、工号{work_code}、绩效{performance_level}，以json格式返回
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

设置新增数据节点中的变量，把3个字段需要新增的值填入

![img](https://i-blog.csdnimg.cn/direct/bebe2a3391894637806134a63ad6171c.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

输入以下内容，试运行流水线：

```
新增绩效记录， 老邢，工号1050，绩效S
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

去数据库中查看数据是否成功写入：

![img](https://i-blog.csdnimg.cn/direct/f53795f556bc48c7a7747ce05e9ec484.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

#### 2.4 更新数据[¶](#24)

更新数据和新增数据流程类似，区别在于更新数据需要先根据更新条件找到对应的数据，再更新这条数据中的内容。

![img](https://i-blog.csdnimg.cn/direct/d96ba90d8ab944f3a99d69e62e39dac7.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

试运行该工作流，输入提示词

```
更新绩效记录， 老邢，工号1050，绩效A
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

查看数据库中的数据，可以看到数据没有新增，而绩效字段确实是由S改为了A，说明了功能生效。

![img](https://i-blog.csdnimg.cn/direct/ed796384efc54f758e37560d530edba5.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

#### 2.5 删除数据[¶](#25)

删除数据只需要删除条件定位到这条数据，即可完成。 对于我们当前的业务场景，一般是工号+人名确定唯一一条数据，所以这里我们需要提取出来name和work_code两个字段，并形成过滤条件，

![img](https://i-blog.csdnimg.cn/direct/eb75160a32ba4d0e81b2bae589437f0e.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

试运行该工作流，输入提示词

```
删除老邢的记录，工号1050
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

查看数据库，发现“老邢”的数据已经被删除：

![img](https://i-blog.csdnimg.cn/direct/fa6cc527fd5d443990f57d564e477294.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

## 四、总结[¶](#_5)

本章节介绍了两种访问私有数据的功能，知识库和数据库：

- 知识库：适用于知识检索，数据基本不写入，只做检索，且进行语义匹配的检索方式。
- 数据库：使用实现业务处理的Agent，对数据库进行增删改查，不支持基于语义的检索方式。

在实际工作中，需要基于不同的场景做不同的技术选型。