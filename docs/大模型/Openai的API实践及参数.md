---
layout: default
title: Openai的API实践及参数
nav_order: 3
has_children: true
parent: 大模型相关
permalink: /docs/LLM/openaiapi
---


## **示例**
在通过程序调用OpenAI的API进行基于提示(prompt)对话任务时，通常是通过Completion接口作为提示的回应，Completion是一个通用的自然语言生成接口，支持生成各种类型的文本任务。
```
import os
import openai

openai.api_key = os.environ.get("API_KEY")

prompts ="Write a promotion for a food brand"   
response = openai.Completion.create(model="gpt-3.5-turbo",
                                           prompt=prompts, 
                                           temperature=0.5,
                                           n=1,
                                           best_of=1,
                                           max_tokens=6,
                                           (top_p=0.1),
                                           frequency_penalty=0,
                                           presence_penalty=2
                                           )   

print(response['choices'][0]['text'].strip())  
```
包含在`openai.Completion.create`中间的各种信息则作为参数，影响输出的结果与质量。

## **实践**
```
import os
import openai
openai.api_key = os.environ.get("API_KEY")

#打开提示文件读取每行内容
with open("prompts.txt") as f:
    contents = f.readlines()
    if (len(contents)>0):
      if contents[-1]==("\n") :
        contents.pop()

#如果提示文件内没有内容则产生错误消息
if contents==[] :
    print("Please write a prompt")
else:
    #提示文件的内容是以行列表的形式存在，把行连起来形成完整提示
    prompts = "".join(contents)
    response = openai.Completion.create(model="gpt-3.5-turbo",
                                            prompt=prompts, 
                                            temperature=0.5,
                                            n=1,
                                            best_of=1,
                                            max_tokens=6,
                                            (top_p=0.1),
                                            frequency_penalty=0,
                                            presence_penalty=2
                                            ) 
    txt_response=response['choices'][0]['text'].strip()
    print(txt_response)
```
在这段脚本中，用于response的参数里采用了davinci模型，设置了最大token数值为256。response打印的结果txt_response做了格式清洗。

我们通过在prompts.txt 文件里输入文本内容，然后调用 prompts.py 来输出结果。

### **1. 问答**
```
Q：爱情公寓中的角色都有哪些？
A:《爱情公寓》是一部中国的电视剧，剧中有以下主要角色：
艾薇（陈赫饰）：一个外表冷酷但内心温暖的大男子主义者。
曾小贤（田亮饰）：一个喜欢兼职、经济实惠、擅长修电器的小聪明。
吕子乔（罗志祥饰）：一个机灵、小聪明、自带台湾腔调的股票投资高手。
陈美嘉（陈意涵饰）：一个漂亮但有点傻气的女孩，喜欢小贤。
秦羽墨（邓家佳饰）：一个冷艳、优雅和知性的女人，喜欢子乔。
张伟（孙艺洲饰）：一个善良但有时不太聪明的男人，一直追求美嘉。
吕子乔的弟弟（黄晓明饰）：外貌与子乔相似，是个花花公子，常常与子乔混淆。
王伟：患有种种疾病，富二代，为了给他的孩子们找到合适的妈妈而参加爱情公寓。
这些是《爱情公寓》中一些主要的角色，他们的故事情节和关系在剧集中有着复杂而有趣的发展。

这里的最后两位没有印象...
```
### **2. 分类**
```
Q:将这些角色进行分类
A:好的，我将《爱情公寓》中的角色进行一些简单的分类：
1. 大男子主义者：
- 艾薇（陈赫饰）
2. 小聪明型角色：
- 曾小贤（田亮饰）
- 吕子乔（罗志祥饰）
3. 傻气可爱型角色：
- 陈美嘉（陈意涵饰）
- 张伟（孙艺洲饰）
4. 冷艳优雅型角色：
- 秦羽墨（邓家佳饰）
5. 其他角色：
- 王伟
这只是基于角色性格等方面的简单分类，每个角色都有独特的特点和故事线。《爱情公寓》中的角色关系错综复杂，通过各种情节发展，呈现出欢乐、搞笑和温馨的故事。
```

### **3. 生成内容**
```
Q:编写一个简单的剧本，主题是旅行，包含所有的角色，并且符合所有人物的性格
A:
角色：
吕子乔 (Lv Ziqiao): 冷艳优雅型角色，聪明机智，喜欢时尚和奢华。
曾小贤 (Zeng Xiaoxian): 小聪明型角色，狡黠机智，常常耍小聪明来解决问题。
张伟 (Zhang Wei): 傻气可爱型角色，单纯天真，总是给大家带来欢乐。
艾薇 (Ai Wei): 大男子主义者，自信阳光，追求刺激与冒险。
陈美嘉 (Chen Meijia): 傻气可爱型角色，天真活泼，爱好摄影和记录生活。
秦羽墨 (Qin Yumo): 冷艳优雅型角色，神秘高傲，对旅行有浓厚的兴趣。
王伟 (Wang Wei): 配角，善良可靠，是大家的朋友。

剧本：
场景一：住宿预订中心
（吕子乔、曾小贤、张伟、艾薇和陈美嘉在住宿预订中心见面）
吕子乔：大家好！我听说我们要一起去旅行了。我已经为我们预订了一个豪华度假别墅。
曾小贤：哼，别墅？那有什么意思！我有一个秘密小屋，里面藏着宝藏！而且离景点更近。
张伟：宝藏？太棒了！我要和小贤一起去探险！
艾薇：嘿，别墅还是小屋，无所谓啦。我的目标是寻找刺激，让我们去尝试一些冒险项目。
陈美嘉：好主意！我会为我们记录下每一个难忘的瞬间。

场景二：旅行途中
（一路上他们经历了各种有趣的事情，包括迷路、遇到奇怪的人和发现了隐藏的美景）
张伟：哎呀，我们好像迷路了！
吕子乔：别怕，我们有我。让我来带路！
曾小贤：我有一个主意，我们问问路边那位可爱的老奶奶。
（老奶奶指引他们到达目的地）
艾薇：旅行真是一次充满惊喜的过程，让我们继续探险吧！

场景三：抵达目的地
（他们终于抵达豪华度假别墅，开始享受旅行）
曾小贤：看吧！这是我的秘密小屋！我们可以一起玩寻宝游戏。
张伟：好刺激！我准备好了，让我们开始冒险！
（他们在小屋探索，最终找到宝藏）
吕子乔：不错，这次小贤的小聪明发挥了作用。
艾薇：冒险真是太刺激了！我已经充满了活力！

场景四：回忆与告别
（大家在旅行结束时回顾这段美好的经历）
陈美嘉：看，这是我们一起照的照片，记录下了我们每一个快乐的瞬间。
吕子乔：这次旅行让我们更加了解彼此，也证明了我们是最好的朋友。
曾小贤：没错！我们的友谊因为这次旅行而更加坚固。
张伟：对！虽然我有时候傻傻的，但是你们从来没有笑话过我
```

## **参数详解**

### **1. model 参数**：
通过输入不同的 model ID 选择使用不同的大型语言模型，该参数是必选参数。例如 text-davinci-002、text-davinci-003 。

### **2. prompt 参数**：
输入给模型的提示，这是可选参数，可以不设置。如果不设置默认是 <|endoftext|>。<|endoftext|> 是 OpenAI 定义的文本生成终止符号，当模型预测输出这个 token 时，文本生成将终止。

### **3. temperature 参数**：
采样的温度（temperature）参数，默认值为 1 ，可选择范围是 [0,2] 之间，较高的值（如1.5）将使输出更加随机，而较低的值（例如0.2）将使其更加集中和确定。

### **4. n 参数**：
表示要生成多少次文本。默认值是 1 。例如当 $n=3$ 时，ChatGPT 会根据你的输入生成 3 次。这 3 次一般是不相同的，因为采样是随机的。$n>1$ 比较适合于同一个问题生成多个候选答案的场景。

### **5. best_of 参数**：
best_of关键参数在n个Completion后并从中选择最好的结果返回。n 需要小于或等于 best_of，而且其实不需要 n 也可以单独使用 best_of。

### **6. top_p 参数**：
核采样参数，默认值为 1， 可选择范围是 (0,1] 之间。需 top_p 是 temperature 采样的替代方案，也被称为nucleus采样，一般只修改其中的一个。

### **7. presence_penalty 和 frequency_penalty 参数**：
这两个参数的范围都在 [-2,2] 之间。值大于 0 表示文本生成将尽量减少出现重复的 token。
- presence_penalty 是通过前文 token 是否出现了来控制重复 token 的出现。
- frequency_penalty 是通过前文 token 出现的频率来控制重复 token 的出现。
它们使用如下公式来控制：
```
mu[j] -> mu[j] - c[j] * frequency_penalty - float(c[j] > 0) * presence_penalty
1. 其中 mu[j] 表示现在生成的 token 选择 j 编号 token 的概率值。
2. c[j] 表示 j 编号 token 在当前位置之前出现（被采样）的频率。
3. float(c[j] > 0) 表示 j 编号 token 是否在当前位置之前出现过。
```
很明显，当 frequency_penalty 和 presence_penalty 均为 0 时，文本生成的过程与前面讲述的相同。当它们均大于 0 时，模型倾向于尽量减少重复的 token，反之，则倾向于尽量增加重复的 token。



