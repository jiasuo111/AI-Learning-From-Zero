# AI_15_Dify_Dify接入外部知识库

##  学习目标[¶](#_1)

- 了解RAGFlow的定位和优势
- 掌握RAGFlow的安装方法
- 掌握RAGFlow知识库创建
- 掌握dify接入RAGFlow的方法

## 一、RAGFlow简介[¶](#ragflow)

### 1 Dify知识库的不足[¶](#1-dify)

Dify中的RAG一直被诟病，它的知识库设置不够丰富和灵活，对于不同形式的文档上传，尤其是pdf扫描版，上传识别效果不好，知识库根本回答不了PDF内的内容。为了解决这这些Dify提供了外部知识库API，这样就可以连接到 Dify 之外的知识库并从中检索知识。

接下来给大家重点介绍Dify连接RAGFlow外部知识库的内容

### 2 什么是RAGFlow[¶](#2-ragflow)

RAGFlow是一款基于**深度文档理解**（deepdoc）构建的开源 RAG引擎。 深度文档理解是RAGFlow对文档解析的一个解决方案，它包含两个组成部分：视觉处理和解析器。其中视觉处理是通过OCR，布局识别，表结构识别来完成图像，PDF，表格的识别的。针对PDF、DOCX、EXCEL和PPT四种文档格式，都有相应的解析器。

能够从各类复杂格式的非结构化数据中提取信息，文本切片过程可视化，还支持手动调整。支持丰富的文件类型，包括 Word 文档、PPT、excel 表格、txt 文件、图片、PDF、影印件、复印件、结构化数据、网页等。

还集成了各种嵌入模型，rerank模型，提供易用的 API，可以轻松集成到各类企业系统。

官网地址：

```
https://RAGFlow.io/
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

安装包镜像完整下载下来，体积非常大，环境要求如下：

- CPU >= 4 核
- 运行内存= 16 GB
- 硬盘 >= 50 GB
- Docker >= 24.0.0 & Docker Compose>= v2.26.1

## 二、RAGFlow的安装方法[¶](#ragflow_1)

### 1 到github下载项目源码[¶](#1-github)

地址如下：

```
https://www.github.com/infiniflow/RAGFlow
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

可以直接下载

![img](https://i-blog.csdnimg.cn/direct/675627f4cccc4885b25c3ebd04515aa3.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

也可以直接使用老师提供的。

### 2 修改配置[¶](#2)

因为dify的原因，接下来我们需要修改两个配置：

- web访问端口
- redis配置

由于前面我们已经安装了Dify项目，这两个项目都依赖了redis，且web端的端口都是默认80端口，因此，为了避免冲突，我们需要修改配置文件。

#### 2.2 修改web访问默认端口[¶](#22-web)

修改文件位置：docker目录下的docker-compose文件

![img](https://i-blog.csdnimg.cn/direct/f57bf4c878c54d8aaf1a3b889c53a0a3.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

#### 2.3 修改redis配置[¶](#23-redis)

修改.env文件

![img](https://i-blog.csdnimg.cn/direct/3d2398b750514f99954d4e3538cb54d7.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

修改docker-compose-base.yml文件

![img](https://i-blog.csdnimg.cn/direct/cd556a094fbb47e49e00b8222aaff6b9.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

### 4 启动服务[¶](#4)

进入docker目录，右键打开命令行：

![img](https://i-blog.csdnimg.cn/direct/fd0235798f8041768ecbb287e5744d75.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

输入命令：

```
docker compose up -d
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://i-blog.csdnimg.cn/direct/6461d9af42af4e4eac56563817361d58.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

时间比较久，需要大家耐心等待。

运行成功：

![img](https://i-blog.csdnimg.cn/direct/760e51aab6be4bd08929f94e907b883f.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

### 5 打开浏览器访问[¶](#5)

由于前面，我们把web端口设置为了8880端口，docker镜像拉取后，等待容器启动完成，在浏览器输入:`127.0.0.1:8880` 即可访问

![img](https://i-blog.csdnimg.cn/direct/65705018667d4ed0a2f9635c598d735e.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

至此，RAGFlow就安装完成了，注册完成后主页面如下：

### 6 模型配置[¶](#6)

![img](https://i-blog.csdnimg.cn/direct/11d504f204f44842baceb9d25ed1f761.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

![img](https://i-blog.csdnimg.cn/direct/e9872b2b35f84cfba4c467305ef76552.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

设置系统默认模型：

![img](https://i-blog.csdnimg.cn/direct/8eefe4dd85ca4c61ab56a83d13d454ca.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

## 三、RAGFlow 创建知识库[¶](#ragflow_2)

### 1 知识库创建[¶](#1)

创建好知识库，点击RAGFlow 创建知识库。

![img](https://i-blog.csdnimg.cn/direct/6fdb6cc904564dcdb889338717d95f7f.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

### 2 上传文件[¶](#2_1)

我们点击新增加文件。上传我们要解析的私有化知识库文档，然后点击解析完成文档向量化。

![img](https://i-blog.csdnimg.cn/direct/ce63cfddd00e462883a4895359d3e9f3.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

可以单个文件解析，也可以批量解析，大家可以根据自己的需要选择使用，解析完成后可进行

### 3 分片方式[¶](#3)

进入模型设置界面 选择好嵌入模型和解析方法

![img](https://i-blog.csdnimg.cn/direct/949cbfa801714f2a8656ef9be79405e7.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

1. **General** 支持的文件格式为DOCX、EXCEL、PPT、IMAGE、PDF、TXT、MD、JSON、EML、HTML。 此方法将简单的方法应用于块文件：系统将使用视觉检测模型将连续文本分割成多个片段。
2. **Q&A** 支持 excel 和 csv/txt 文件格式。 如果文件是 excel 格式，则应由两个列组成 没有标题：一个提出问题，另一个用于答案， 答案列之前的问题列。多张纸是只要列正确结构，就可以接受。 如果文件是 csv/txt 格式 以 UTF-8 编码且用 TAB 作分开问题和答案的定界符。
3. **Resume** 支持的文件格式为DOCX、PDF、TXT。 简历有多种格式，就像一个人的个性一样，但我们经常必须将它们组织成结构化数据，以便于搜索。 我们不是将简历分块，而是将简历解析为结构化数据。 作为HR，你可以扔掉所有的简历， 您只需与'RAGFlow'交谈即可列出所有符合资格的候选人。
4. **Manual** 仅支持PDF。 我们假设手册具有分层部分结构。 我们使用最低的部分标题作为对文档进行切片的枢轴。 因此，同一部分中的图和表不会被分割，并且块大小可能会很大。
5. **Table** 支持EXCEL和CSV/TXT格式文件。
6. **Paper** 仅支持PDF文件。 如果我们的模型运行良好，论文将按其部分进行切片，例如摘要、1.1、1.2等。 这样做的好处是LLM可以更好的概括论文中相关章节的内容， 产生更全面的答案，帮助读者更好地理解论文。 缺点是它增加了 LLM 对话的背景并增加了计算成本， 所以在对话过程中，你可以考虑减少‘topN’的设置。
7. **Book** 支持的文件格式为DOCX、PDF、TXT。 由于一本书很长，并不是所有部分都有用，如果是 PDF， 请为每本书设置页面范围，以消除负面影响并节省分析计算时间。
8. **Laws** 支持的文件格式为DOCX、PDF、TXT。 法律文件有非常严格的书写格式。 我们使用文本特征来检测分割点。 chunk的粒度与'ARTICLE'一致，所有上层文本都会包含在chunk中。

这里需要注意的是， ragflow还支持知识图谱功能，开启后，系统将基于大模型自动提取知识图谱。开启后，可以很好的提关系查询的效果：

![img](https://i-blog.csdnimg.cn/direct/59c713c2da474fc08bf4e744794de5b9.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

## 四、RAGFlow 知识库检索[¶](#ragflow_3)

聊天窗口界面

![img](https://i-blog.csdnimg.cn/direct/5ddfb50ecdd84f64ae0235674483b7cd.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

聊天窗口配置有3个选项面板（助理设置、提示引擎、模型设置）

助理设置，这里最关键就是 填写助理姓名和选择指定的知识库

![img](https://i-blog.csdnimg.cn/direct/b8dfadb9edf84712accfa6c4ba5dd7e7.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

提示引擎 如果大家没有 Rerank模型 可以默认不选。

![img](https://i-blog.csdnimg.cn/direct/6f46af3e25a143228feb5a18047f216c.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

模型设置，这里需要填写选择一个LLM大语言模型作为推理模型使用。

![img](https://i-blog.csdnimg.cn/direct/07f14adfacfe417b80a2cbeb18af3c72.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

以上设置完成后就可以进行聊天对话了。

![img](https://i-blog.csdnimg.cn/direct/bb9e8dbd6f894c679b36af5733e70320.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

以上设置我们完成了RAGFlow相关设置。

## 五、RAGFlow API 设置[¶](#ragflow-api)

接下来我们在dify调用这个RAGFlow ，需要设置一下RAGFlow api key.

### 1 获取API[¶](#1-api)

点击系统右上角，选择 API

![img](https://i-blog.csdnimg.cn/direct/952790a1b8c1477da2c74d51d7aa62a2.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

### 2 获取IP[¶](#2-ip)

API 服务器 显示RAGFlow 对外提供的IP， 我的显示是http://127.0.0.1:8880!

![img](https://i-blog.csdnimg.cn/direct/408d9bd9558a4b2ea001fac3ccea56e9.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

### 3 获取API key[¶](#3-api-key)

点击上面key生成RAGFlow 对外提供的API

![img](https://i-blog.csdnimg.cn/direct/52065d00bde04facb2545a2c92236f7f.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

下面就是RAGFlow 对外提供的HTTP 请求API接口文档

![img](https://i-blog.csdnimg.cn/direct/e4545e53022d49988f5be472b2bda7e9.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

## 六 dify配置外部知识库[¶](#dify)

### 1 dify配置外部知识库[¶](#1-dify_1)

dify 工作流管理界面，点击上面知识库。点击链接外部知识库。

![img](https://i-blog.csdnimg.cn/direct/f9c5306ae7994d36b14aed9a96974b64.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

### 2 配置API[¶](#2-api)

击右边的外部知识库API 先把外部知识库API 配置好。

![img](https://i-blog.csdnimg.cn/direct/6d4bee7d88334ba9b50fc802a88aaf08.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

这里我需要添加3个值：

1. name ：随便写一个名字
2. API Endpoint 这个就是和RAGFlow 整合的地址。RAGFlow 对外提供的是192.168.xx.xx 这里填写http://192.168.XX.XXX:9380/api/v1/dify
3. api key 就是上面RAGFlow-开头的api KEY

![img](https://i-blog.csdnimg.cn/direct/3266557a76be4728967b77e9ea7b8ff5.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

这里的 API Endpoint 的 IP 地址一定是要宿主机网卡上的 IP 地址，不要用 127.0.0.1 或 localhost

![img](https://i-blog.csdnimg.cn/direct/9c296dc750104ecab07f865562e1b9db.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

### 3 连接知识库[¶](#3_1)

![img](https://i-blog.csdnimg.cn/direct/cc62e7dc94d740b8801923c33e511f4a.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

这个外部知识库 ID 如何获取呢？我们回到RAGFlow 知识库页面，从url中获取：

![img](https://i-blog.csdnimg.cn/direct/003e548200814a1da5b3b50a89544205.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

点击链接，完成RAGFlow和dify的链接

![img](https://i-blog.csdnimg.cn/direct/da5079c4b38c4ec3926fb76e0d62d0d1.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

到这dify 和RAGFlow已经连接好了。

![img](https://i-blog.csdnimg.cn/direct/1929bd2c38c1473b8344fa48c833758b.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

## 七、dify ai agent使用外部知识库[¶](#dify-ai-agent)

![img](https://i-blog.csdnimg.cn/direct/9ba04a2784e64a218b2bd1bfd157f5fb.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

进入ai Agent 聊天界面，在上下文添加外部知识库。

![img](https://i-blog.csdnimg.cn/direct/7714ccf27f2242dbb81dec33cfeaf1a6.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

后面就可以进行知识库检索了，检索的是调用通过工具调用知识库。

![img](https://i-blog.csdnimg.cn/direct/23a6265098ce435aaf3616fd847af1fc.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

到这在dify中调用RAGFlow中的知识库了。