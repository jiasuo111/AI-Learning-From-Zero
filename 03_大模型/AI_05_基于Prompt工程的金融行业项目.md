## 学习目标[¶](#_1)

- 了解项目背景
- 了解整体项目任务
- 掌握Few-Shot、Zero-Shot的思想
- 完成课程代码开发

## 一、项目介绍[¶](#_2)

### 1 项目背景[¶](#1)

当前金融领域信息化发展的时代，金融数据大量激增，许多投资者和研究者试图通过对这些数据进行深度分析而获得一些有效的决策和帮助，尽可能减少决策失误带来的损失。所以，针对金融数据的分析方法研究是目前十分有益且热门的话题。

人工智能技术在金融行业动态分析领域的应用主要包括： - 风险评估：通过AI对大数据分析，识别出不同金融风险事件类型、反欺诈行为、信用评分低等风险，帮助金融机构全面评估贷款人的信用状况，从而提高贷款的准确性和风险控制能力。 - 投资决策：通过AI技术对历史数据、财务报表等信息分析，为投资者提供精准的投资决策支持。 - 客户服务：通过AI技术，实现智能客服等功能，进而为客户提供便捷而又准确的答案

因此，本项目主要基于大模型来直接实现在金融领域相关任务的应用。重点在于如何对大模型设计prompt，从而激发大模型的"涌现能力"，进而给出准确的答案。

### 2 项目任务与方法介绍[¶](#2)

项目任务（三大业务场景）： - 金融文本分类：将金融文本分成不同类型。 - 金融文本信息抽取：抽取金融文本中的信息。 - 金融文本匹配：判断两个金融文本是否类似。

大模型选择：Qwen

采用方法：基于Few-Shot+Zero-Shot的思想，设计prompt， 进而应用大模型完成相应的任务。

## 二、LLM实现金融文本分类[¶](#llm)

### 1 需求[¶](#1_1)

下面几段文本来自某平台发布的金融领域文本：

```
1."今日，央行发布公告宣布降低利率，以刺激经济增长。这一降息举措将影响贷款利率，并在未来几个季度内对金融市场产生影响。",
2."ABC公司今日发布公告称，已成功完成对XYZ公司股权的收购交易。本次交易是ABC公司在扩大业务范围、加强市场竞争力方面的重要举措。据悉，此次收购将进一步巩固ABC公司在行业中的地位，并为未来业务发展提供更广阔的发展空间。详情请见公司官方网站公告栏",
3."公司资产负债表显示，公司偿债能力强劲，现金流充足，为未来投资和扩张提供了坚实的财务基础。",
4."最新的分析报告指出，可再生能源行业预计将在未来几年经历持续增长，投资者应该关注这一领域的投资机会",
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

我们的目的是期望模型能够帮助我们识别出这4段话中，每一句话描述的是一个什么类型的报告。因此，我们期望模型输出的结果为：

```
['新闻报道', '公司公告', '财务公告 '分析师报告']
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

- 金融新闻自动分类
- 公司文档智能归档
- 投资研究材料整理
- 金融数据预处理

### 2 项目思路[¶](#2_1)

基于提示词进行模型预测，处理流程：

- 加载模型
- 构建提示词 (描述清楚任务及输出格式)
- 模型预测
- 后处理

### 3 Prompt设计[¶](#3-prompt)

在该任务的 prompt 设计中，我们主要考虑 2 点：

- 需要向模型解释什么叫作「文本分类任务」

- 需要让模型按照我们指定的格式输出

- 为了让模型知道什么叫做「文本分类」，我们借用 few-shot 的方式，先给模型展示几个正确的例子：

  > User: "今日，股市经历了一轮震荡，受到宏观经济数据和全球贸易紧张局势的影响。投资者密切关注美联储可能的政策调整，以适应市场的不确定性。" 是['新闻报道', '公司公告', '财务公告 '分析师报告']里的什么类别？ Bot: 新闻报道
  >
  > User: "本公司年度财务报告显示，去年公司实现了稳步增长的盈利，同时资产负债表呈现强劲的状况。经济环境的稳定和管理层的有效战略执行为公司的健康发展奠定了基础。"是['新闻报道', '公司公告', '财务公告 '分析师报告']里的什么类别？ Bot: 财务报告 `

 其中，`User` 代表我们输入给模型的句子，`Bot` 代表模型的回复内容。

 注意：上述例子中 `Bot` 的部分也是由人工输入的，其目的是希望看到在看到类似 `User` 中的句子时，模型应当做出类似 `Bot` 的回答。

### 4 代码实现[¶](#4)

使用Few-Shot方式，将系统提示和示例写到prompt中

**实现流程**：

**1.初始化阶段**

```
class_examples = {
    '新闻报道': '今日，股市经历了一轮震荡...',
    '财务报告': '本公司年度财务报告显示...', 
    '公司公告': '本公司高兴地宣布成功完成...',
    '分析师报告': '最新的行业分析报告指出...'
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

定义4个金融文本类别，并提供示例，每个类别对应一个典型文本样例

**2.构建Prompt**

```
def init_prompts(class_examples):
    pre_prompt = '你是一个金融专家，需要对输入的金融领域文本进行分析...'
    # 添加示例演示
    for class_type, example in class_examples.items():
        pre_prompt = pre_prompt + f'"""{example}是{class_list}里的什么类别？"""' + f'"""{class_type}。"""'
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

将模型设定为金融专家，明确分类任务和输出格式要求，通过few-shot learning展示正确的分类方式

**生成的Prompt示例**：

```
你是一个金融专家，需要对输入的金融领域文本进行分析，将类型归类到['新闻报道', '财务报告', '公司公告', '分析师报告']...
"""今日，股市经历了一轮震荡...是['新闻报道', '财务报告', '公司公告', '分析师报告']里的什么类别？"""
"""新闻报道。"""
[其他3个示例...]
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**3.模型调用**

```
def model_chat(content: str) -> str:
    # 调用通义千问API
    client = openai.OpenAI(api_key=os.environ['DASHSCOPE_API_KEY'])
    response = client.chat.completions.create(model='qwen-plus', ...)
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**4. 模型预测+后处理**

```
sentences = [
    "今日，央行发布公告宣布降低利率...",  # 新闻报道
    "ABC公司今日发布公告称...",           # 公司公告  
    "公司资产负债表显示...",              # 财务报告
    "最新的分析报告指出..."              # 分析师报告
]

for sentence in sentences:
    content = prompts_info['pre_prompt'] + f'"""{sentence}是...什么类别？"""'
    resp = model_chat(content)
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

拼接完整Prompt并对4个测试句子依次分类

**代码实现：**

```
# 0 导入必备的工具包
import openai
import os

# 1.所有类别以及每个类别下的样例
class_examples = {
    '新闻报道': '今日，股市经历了一轮震荡，受到宏观经济数据和全球贸易紧张局势的影响。投资者密切关注美联储可能的政策调整，以适应市场的不确定性。',
    '财务报告': '本公司年度财务报告显示，去年公司实现了稳步增长的盈利，同时资产负债表呈现强劲的状况。经济环境的稳定和管理层的有效战略执行为公司的健康发展奠定了基础。',
    '公司公告': '本公司高兴地宣布成功完成最新一轮并购交易，收购了一家在人工智能领域领先的公司。这一战略举措将有助于扩大我们的业务领域，提高市场竞争力',
    '分析师报告': '最新的行业分析报告指出，科技公司的创新将成为未来增长的主要推动力。云计算、人工智能和数字化转型被认为是引领行业发展的关键因素，投资者应关注这些趋势'}


# 2 构建函数，进行prompt设计（描述清楚任务及输出格式）
def init_prompts(class_examples):
    '''
     初始化前置prompt，便于模型做 incontext learning。
    :param class_examples:
    :return:
    '''
    class_list = list(class_examples.keys())
    pre_prompt = f'你是一个金融专家，需要对输入的金融领域文本进行分析，将类型归类到{class_list}，对于不在这四类中的数据，输出‘不清楚类型’。以下是几个示例分类：'
    for class_type, example in class_examples.items():
        pre_prompt = pre_prompt + f'"""{example}是{class_list}里的什么类别？"""' + f'"""{class_type}。"""'
    return {'class_list': class_list, 'pre_prompt': pre_prompt}


# 3 构建推理函数
def model_chat(content: str) -> str:
    """
    调用大模型对话接口
    :param messages: 输入内容
    :param model: 模型名称
    :return: 大模型输出内容 str
    """
    client = openai.OpenAI(
        api_key=os.environ['DASHSCOPE_API_KEY'],
        base_url='https://dashscope.aliyuncs.com/compatible-mode/v1'
    )
    messages = [{"role": "user", "content": content}]
    response = client.chat.completions.create(
        model='qwen-plus',
        messages=messages,
        stream=False,
    )
    text = response.choices[0].message.content
    return text


# 5 调用推理+后处理():
prompts_info = init_prompts(class_examples)

sentences = [
    "今日，央行发布公告宣布降低利率，以刺激经济增长。这一降息举措将影响贷款利率，并在未来几个季度内对金融市场产生影响。",
    "ABC公司今日发布公告称，已成功完成对XYZ公司股权的收购交易。本次交易是ABC公司在扩大业务范围、加强市场竞争力方面的重要举措。据悉，此次收购将进一步巩固ABC公司在行业中的地位，并为未来业务发展提供更广阔的发展空间。详情请见公司官方网站公告栏",
    "公司资产负债表显示，公司偿债能力强劲，现金流充足，为未来投资和扩张提供了坚实的财务基础。",
    "最新的分析报告指出，可再生能源行业预计将在未来几年经历持续增长，投资者应该关注这一领域的投资机会",
]

for sentence in sentences:
    content = f"{prompts_info['pre_prompt']}" + f'"""{sentence}是 {prompts_info["class_list"]} 里的什么类别？"""'
    print("content: ", content)
    resp = model_chat(content)
    print("class result: ", resp)
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## 三、LLM实现金融文本信息抽取[¶](#llm_1)

### 1 需求[¶](#1_2)

下面几段文本来自某平台发布的股票信息：

```
1.'2023-02-15，寓意吉祥的节日，股票佰笃[BD]美股开盘价10美元，虽然经历了波动，但最终以13美元收盘，成交量微幅增加至460,000，投资者情绪较为平稳。',

2.'2023-04-05，市场迎来轻松氛围，股票盘古(0021)开盘价23元，尽管经历了波动，但最终以26美元收盘，成交量缩小至310,000，投资者保持观望态度。',
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

我们的目的是期望模型能够帮助我们识别出这两段话中的SPO三元组信息。这里定义的信息抽取Schema如下：

```
# 不同实体下的具备属性
schema = {
    '金融': ['日期', '股票名称', '开盘价', '收盘价', '成交量'],
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

期望抽取的结果如下：

```
{"日期": ["2023-02-15"], "股票名称": ["佰笃[BD]美股"], "开盘价": ["10美元"], "收盘价": ["13美元"], "成交量": ["460,000"]}
{"日期": ["2023-04-05"], "股票名称": ["盘古(0021)"], "开盘价": ["23元"], "收盘价": ["26美元"], "成交量": ["310,000"]}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

### 2 项目思路[¶](#2_2)

基于提示词进行模型预测，处理流程：

- 加载模型
- 构建提示词 (描述清楚任务及输出格式)
- 模型预测
- 后处理

### 3 Prompt设计[¶](#3-prompt_1)

在该任务的 prompt 设计中，我们主要考虑 2 点：

- 需要向模型解释什么叫作「信息抽取任务」
- 需要让模型按照我们指定的格式（json）输出

为了让模型知道什么叫做「信息抽取」，我们借用 In-context Learning 的方式，先给模型展示几个正确的例子：

> User:'2023-01-10，股市震荡。股票古哥-D[EOOE]美股今日开盘价100美元，一度飙升至105美元，随后回落至98美元，最终以102美元收盘，成交量达到520000。'。提取上述句子中“金融”('日期', '股票名称', '开盘价', '收盘价', '成交量')类型的实体，并按照JSON格式输出，上述句子中没有的信息用['原文中未提及']来表示，多个值之间用','分隔。 Bot: {'日期': ['2023-01-10'],'股票名称': ['古哥-D[EOOE]美股'],'开盘价': ['100美元'], '收盘价': ['102美元'],成交量': ['520000']}

 其中，`User` 代表我们输入给模型的句子，`Bot` 代表模型的回复内容。

 注意：上述例子中 `Bot` 的部分也是由人工输入的，其目的是希望看到在看到类似 `User` 中的句子时，模型应当做出类似 `Bot` 的回答。

### 4 代码实现[¶](#4_1)

使用history的方式，添加提示词示例。

实现流程：

**1.构建示例**

```
ie_examples = [
    {
        'content': '2023-01-10，股市震荡。股票古哥-D[EOOE]美股今日开盘价100美元...',
        'answers': {
            '日期': ['2023-01-10'],
            '股票名称': ['古哥-D[EOOE]美股'],
            '开盘价': ['100美元'],
            '收盘价': ['102美元'],
            '成交量': ['520000'],
        }
    }
]
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

提供一个完整的示例，展示如何从金融文本中提取信息，并且定义需要提取的实体类型：日期、股票名称、开盘价、收盘价、成交量

**2、构建对话历史，作为提示词样例**

```
def build_prompt(ie_examples):
    history_list = [{'role': 'system', 'content': "你是信息提取专家..."}]

    for example in ie_examples:
        # 用户提问：提取句子中的实体
        history_list.append({'role': 'user', 'content': IE_PATTERN.format(sentence)})
        # 助手回答：正确的提取结果
        history_list.append({'role': 'assistant', 'content': json.dumps(example['answers'])})
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

指定角色信息提取专家，通过对话历史教AI如何提取信息，并要求输出JSON格式，未提及的信息用特定标记。

**3. 模型调用**

```
def model_chat(content: str, history=[]) -> str:
    # 调用通义千问API
    client = openai.OpenAI(api_key=os.environ['DASHSCOPE_API_KEY'])
    response = client.chat.completions.create(...)
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

配置大模型API，进行对话

**4. 模型推理**

```
for sentence in sentences:
    result = model_chat(sentence, prompt_dict['history_list'])
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

输入新的金融文本句子，模型基于学习到的模式进行信息提取，并输出结构化的实体信息

代码如下所示：

```
# 0.导入必备的工具包
import openai
import json
import os


# 1.提供一些例子供模型参考
ie_examples = [
    {
        'content': '2023-01-10，股市震荡。股票古哥-D[EOOE]美股今日开盘价100美元，一度飙升至105美元，随后回落至98美元，最终以102美元收盘，成交量达到520000。',
        'answers': {
            '日期': ['2023-01-10'],
            '股票名称': ['古哥-D[EOOE]美股'],
            '开盘价': ['100美元'],
            '收盘价': ['102美元'],
            '成交量': ['520000'],
        }
    }
]

# 2 构建函数，进行prompt设计（描述清楚任务及输出格式）
IE_PATTERN = "{}\n\n提取上述句子中的实体，并输出，上述句子中不存在的信息用['原文中未提及']来表示。"


def build_prompt(ie_examples):
    history_list = [{'role': 'system',
                     'content': "你是信息提取专家，需要需要完成信息抽取任务。"
                                "我会给你一个句子，你需要提取句子中的实体，并输出，如果句子中有不存在的信息用['原文中未提及']来表示。"}]

    # 遍历示例，将样本和实体 添加到history_list中
    for example in ie_examples:  # 遍历每个样本
        sentence = example['content']
        # 获取到金融类型需要抽取的实体
        history_list.append({'role': 'user', 'content': IE_PATTERN.format(sentence)})
        history_list.append({'role': 'assistant', 'content': json.dumps(example['answers'],ensure_ascii=False)})

    return {'history_list': history_list}


# 3 构建推理函数
def model_chat(content: str, history=[]) -> str:
    """
    调用大模型对话接口
    :param messages: 输入内容
    :param model: 模型名称
    :return: 大模型输出内容 str
    """
    client = openai.OpenAI(
        api_key=os.environ['DASHSCOPE_API_KEY'],
        base_url='https://dashscope.aliyuncs.com/compatible-mode/v1'
    )
    messages = [{"role": "user", "content": content}]
    response = client.chat.completions.create(
        model='qwen-plus',
        messages=history + messages,
        stream=False,
    )
    text = response.choices[0].message.content
    return text


# 4 调用推理
sentences = [
    '2025-02-15，寓意吉祥的节日，股票佰笃[BD]美股开盘价10美元，虽然经历了波动，但最终以13美元收盘，成交量微幅增加至460,000，投资者情绪较为平稳。',
    '2025-04-05，市场迎来轻松氛围，股票盘古(0021)开盘价23元，尽管经历了波动，但最终以26美元收盘，成交量缩小至310,000，投资者保持观望态度。',
]

prompt_dict = build_prompt(ie_examples)
for sentence in sentences:
    print(f'sentence-->{sentence}')
    result = model_chat(sentence, prompt_dict['history_list'])
    print(result)
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## 四、LLM实现金融文本匹配[¶](#llm_2)

### 1 需求[¶](#1_3)

- 下面是几个金融方面的短文本对：

```
1.('股票市场今日大涨，投资者乐观。', '持续上涨的市场让投资者感到满意。'),
2.('油价大幅下跌，能源公司面临挑战。', '未来智能城市的建设趋势愈发明显。'),
3.('利率上升，影响房地产市场。', '高利率对房地产有一定冲击。'),
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

- 我们期望模型能够帮我们识别出这 3 对句子中，哪几对描述的是相似的语言。

  即期望模型输出的结果为：

```
['相似', '不相似', '相似']
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

### 2 项目思路[¶](#2_3)

基于提示词进行模型预测，处理流程：

- 加载模型
- 构建提示词 (描述清楚任务及输出格式)
- 模型预测
- 后处理

### 3 Prompt设计[¶](#3-prompt_2)

在该任务的 prompt 设计中，我们主要考虑 2 点：

- 需要向模型解释什么叫作「文本匹配任务」

- 需要让模型按照我们指定的格式输出

- 为了让模型知道什么叫做「文本匹配任务」，我们借用 In-context Learning 的方式，先给模型展示几个正确的例子：

  > User: 句子一: 公司ABC发布了季度财报，显示盈利增长。\n句子二: 财报披露，公司ABC利润上升 Bot: 是
  >
  > User:句子一: 黄金价格下跌，投资者抛售。\n句子二: 外汇市场交易额创下新高 Bot: 不是 ...

 其中，`User` 代表我们输入给模型的句子，`Bot` 代表模型的回复内容。

 注意：上述例子中 `Bot` 的部分也是由人工输入的，其目的是希望看到在看到类似 `User` 中的句子时，模型应当做出类似 `Bot` 的回答。

### 4 代码实现[¶](#4_2)

使用history的方式，添加提示词示例。

实现流程：

**0. 构建示例**

```
examples = {
    '是': [
        ('公司ABC发布了季度财报，显示盈利增长。', '财报披露，公司ABC利润上升。'),
    ],
    '不是': [
        ('黄金价格下跌，投资者抛售。', '外汇市场交易额创下新高。'),
        ('央行降息，刺激经济增长。', '新能源技术的创新。')
    ]
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

通过相似和不相似示例使AI理解"语义相似"的概念

**2. 构建对话历史，提供样例**

```
def init_prompts(examples):
    pre_history = [
        {
            "role": "system",
            "content": '现在你需要帮助我完成文本匹配任务...只需要回答是否相似，不要做多余的回答。'
        }
    ]

    # 添加所有教学示例到对话历史
    for key, sentence_pairs in examples.items():
        for sentence_pair in sentence_pairs:
            sentence1, sentence2 = sentence_pair
            # 用户提问
            pre_history.append({'role': 'user', 'content': f'句子一: {sentence1}\n句子二: {sentence2}...'})
            # AI回答（是/不是）
            pre_history.append({'role': 'assistant', 'content': key})
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

明确任务要求和输出格式，通过完整的对话记录教AI如何判断，并要求只回答"是"或"不是"

**3.模型调用**

```
def model_chat(content: str, history=[]):
    # 调用大模型API
    client = openai.OpenAI(api_key=os.environ['DASHSCOPE_API_KEY'])
    response = client.chat.completions.create(...)
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**4. 模型推理，进行语义匹配**

```
for sentence_pair in sentence_pairs:
    sentence1, sentence2 = sentence_pair
    sentence_with_prompt = f'句子一: {sentence1}\n句子二: {sentence2}\n上面两句话是相似的语义吗？'
    result = model_chat(sentence_with_prompt, history=prompts_info['pre_history'])
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

输入新的句子对，使用统一的问题格式提问，最后输出判断结果（是/不是）

代码实现：

```
# 0. 导入必备的工具包
import openai
import os

# 1 提供相似，不相似的语义匹配例子
examples = {
    '是': [
        ('公司ABC发布了季度财报，显示盈利增长。', '财报披露，公司ABC利润上升。'),
    ],
    '不是': [
        ('黄金价格下跌，投资者抛售。', '外汇市场交易额创下新高。'),
        ('央行降息，刺激经济增长。', '新能源技术的创新。')
    ]
}


# 2 构建函数，进行prompt设计（描述清楚任务及输出格式）
def init_prompts(examples):
    """
    初始化前置prompt，便于模型推理。
    """
    pre_history = [
        {
            "role": "system",
            "content": '现在你需要帮助我完成文本匹配任务，当我给你两个句子时，你需要回答我这两句话语义是否相似。'
                       '只需要回答是否相似，不要做多余的回答。'
        }
    ]

    for key, sentence_pairs in examples.items():
        for sentence_pair in sentence_pairs:
            sentence1, sentence2 = sentence_pair
            pre_history.append({
                "role": 'user',
                "content": f'句子一: {sentence1}\n句子二: {sentence2}\n上面两句话是相似的语义吗？'
            })
            pre_history.append({
                "role": 'assistant',
                "content": key
            })

    return {'pre_history': pre_history}


# 3 构建推理函数
def model_chat(content: str, history=[]):
    """
    调用大模型对话接口
    :param messages: 输入内容
    :param model: 模型名称
    :return: 大模型输出内容 str
    """
    client = openai.OpenAI(
        api_key=os.environ['DASHSCOPE_API_KEY'],
        base_url='https://dashscope.aliyuncs.com/compatible-mode/v1'
    )
    messages = [{"role": "user", "content": content}]
    response = client.chat.completions.create(
        model='qwen-plus',
        messages=history + messages
    )
    text = response.choices[0].message.content
    return text


# 4 调用推理
prompts_info = init_prompts(examples)
sentence_pairs = [
    ('股票市场今日大涨，投资者乐观。', '持续上涨的市场让投资者感到满意。'),
    ('油价大幅下跌，能源公司面临挑战。', '未来智能城市的建设趋势愈发明显。'),
    ('利率上升，影响房地产市场。', '高利率对房地产有一定冲击。'),
]
for sentence_pair in sentence_pairs:
    sentence1, sentence2 = sentence_pair
    sentence_with_prompt = f'句子一: {sentence1}\n句子二: {sentence2}\n上面两句话是相似的语义吗？'
    result = model_chat(sentence_with_prompt, history=prompts_info['pre_history'])
    print(f'sentence_pair-->{sentence_pair}')
    print(f'result-->{result}')
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## 五、本章小结[¶](#_3)

本章节主要介绍了项目开发的背景及意义，采用了Zero-Shot/Few-Shot学习方式，完成了金融文本分类、金融信息抽取以及近似文本匹配。