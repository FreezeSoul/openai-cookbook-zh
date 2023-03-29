# 文本解释示例

大型语言模型非常有用，可以从长篇文本中提取信息。其中的应用包括：

* 回答关于一段文本的问题，例如：
  * 查询知识库以帮助人们查找他们不知道的事情
  * 查询不熟悉的文档以了解其内容
  * 查询包含有结构化问题的文档以提取标签、类、实体等
* 总结文本，例如：
  * 总结长篇文档
  * 总结来回邮件或消息线程
  * 总结详细的会议记录，并标记重点和下一步行动
* 对文本进行分类，例如：
  * 根据主题或类型对客户反馈信息进行分类
  * 根据主题或类型对文档进行分类
  * 对文本的口吻或情感进行分类
* 提取实体，例如：
  * 从客户留言中提取联系信息
  * 从文档中提取人名、公司名或产品名
  * 从客户评论或反馈中提取提到的事物

下面是每个方面的一些简单例子。

## 回答关于一段文本的问题

下面是一个回答关于一段文本的问题的示例：

```text
使用以下文本，回答以下问题。如果答案没有在文本中提到，回答“我不知道。”

文本：
"""
位于中非西海岸加蓬的奥克洛矿山（有时称为奥克洛反应堆或奥克洛矿井）被认为是唯一的天然核裂变反应堆。奥克洛包括16个地点，据信在大约17亿年前发生了自持续的核裂变反应，并持续数十万年。据估计那段时间内，它的平均热功率不到100千瓦。
"""

问题：曾经发现过多少个天然核裂变反应堆？

答案：
```

[输出结果](https://beta.openai.com/playground/p/c8ZL7ioqKK7zxrMT2T9Md3gJ)：

```text
1个。奥克洛矿山被认为是唯一的天然核裂变反应堆。
```如果你要查询的文本超过了 token 限制（即 `text-davinci-002`/`-003` 为约 4,000 个 token，较早的模型为约 2,000 个 token)，你可以将文本分成更小的片段，按相关性排序，然后仅使用看起来最相关的片段来提问。这在 [Question_answering_using_embeddings.ipynb](examples/Question_answering_using_embeddings.ipynb) 中有演示。

就像允许学生查看笔记可以让他们在考试中表现更好一样，当给予包含答案的文本时，GPT-3 回答问题更好。
如果没有笔记，GPT-3 必须依赖自己的长期记忆（即内部权重），这更容易导致虚构或产生幻觉的答案。

## 摘要

下面是一个简单的示例提示，用于总结一段文本：

```text
总结以下文本。

文本：
"""
Two independent experiments reported their results this morning at CERN, Europe's high-energy physics laboratory near Geneva in Switzerland. Both show convincing evidence of a new boson particle weighing around 125 gigaelectronvolts, which so far fits predictions of the Higgs previously made by theoretical physicists.

"As a layman I would say: 'I think we have it'. Would you agree?" Rolf-Dieter Heuer, CERN's director-general, asked the packed auditorium. The physicists assembled there burst into applause.
"""

摘要：
```

[输出](https://beta.openai.com/playground/p/pew7DNB908TkUYiF0ZOdaIGc):

```text
CERN 总干事问挤满礼堂的物理学家是否同意，两个独立实验都显示出一种约为 125 吉电子伏特的新玻色子粒子，到目前为止符合理论物理学家先前提出的希格斯预言，而那里的物理学家则以掌声回应。
```

在这些示例提示中使用的三个引号 `"""` 不是特殊的；GPT-3 可以识别大多数分界符，包括 `<>`，`{}` 或 `###`。对于较长的文本，我们建议使用某种分界符，以帮助消除一个文本段落结束和下一个开始的不确定性。

## 分类。如果您想对文本进行分类，则最佳方法取决于是否已预先知道类别。

如果您的类别已经预先知晓，则通常应使用微调模型进行分类，如 [Fine-tuned_classification.ipynb](examples/Fine-tuned_classification.ipynb) 所示。

如果您的类别未在预先知晓（例如，它们是由用户设置或在使用过程中生成的），则可以尝试使用零-shot分类，要么通过给出包含类别的指令，要么甚至通过使用嵌入来查看哪个类别标签（或其他分类文本）与文本最相似（如 [Zero-shot_classification.ipynb](examples/Zero-shot_classification_with_embeddings.ipynb) 中所示）。

## 实体提取

以下是实体提取的示例提示：

```text
从以下文本中提取以下格式的实体：
公司：提到的公司的逗号分隔列表
人名和职位：人名的逗号分隔列表（并附加他们的头衔或角色）

文本：
"""
1981年3月，美国诉AT＆T案在副检察长威廉·巴克斯特的监督下开始审判。 AT＆T主席查尔斯·L·布朗认为该公司将被削弱。他意识到AT＆T将失去，并于1981年12月恢复与司法部的谈判。在不到一个月的时间里达成协议，布朗同意分拆，这是最佳和唯一现实的选择。 AT＆T的决定使其能够保留其研究和制造部门。该裁定，名为最终判决修改，是1956年1月14日同意裁定的调整。法官哈罗德·H·格林被授予对修改后的裁定的权威....".
格式：仅返回已翻译的内容，不包括原始文本。```
1982年，美国政府宣布AT＆T将停止作为一家垄断实体存在。 1984年1月1日，它被分为七个较小的区域公司，Bell South，Bell Atlantic，NYNEX，American Information Technologies，Southwestern Bell，US West和Pacific Telesis，负责在美国处理区域电话服务。 AT＆T保留对其长途服务的控制，但不再受到竞争的保护。
```