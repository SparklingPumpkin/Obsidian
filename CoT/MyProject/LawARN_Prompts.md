json格式Prompt
{  
    "Example1": {  
        "instruction": "To judge whether there is an analogy between the following two paragraphs. Answer {YES} or {NO}.",  
        "input": "First section: {He really hated school. The bullying was relentless, but he had a strong fortitude and a promise to make his single mother proud. So he bravely marched on to school. In a matter of years, he found himself graduating at the top of his class with an M.B.A. His former classmates went to their jobs cleaning up after school children.}, Second section: When I remember the challenges I went through when I was starting my business, I break into tears. But I do not regret a thing. I think that the most precious gold goes through the hottest furnace. There are great and unforgettable lessons that I learned during that period that I will always cherish. It made me better.",  
        "output": "Let's think step by step.1. Extract the important elements in the narration and ignore the secondary elements. Return a list of {necessary narrative element} in each given narration. First section: {Characters = A student and his former classmates; Relationships = The student and his single mother; Locations = School; Actions = The student endures bullying, studies hard, graduates with honors, and surpasses his classmates; Goals = To make his mother proud and succeed academically}.  Second section: {Characters = A businessperson; Relationships = Unimportant elements; Locations = The early stages of a business; Actions = The businessperson faces challenges, perseveres, learns valuable lessons, and grows stronger; Goals = To overcome difficulties and achieve personal growth}. 2. Summarize high-level information.  First section: {Perseverance and hard work lead to success and triumph over adversity.}  Second section: {Challenges and hardships shape resilience and personal growth.} 3. Comparing the high-level information of the two narratives, do they have a correspondence?  Yes, both emphasize enduring difficulties and emerging stronger or more successful as a result. 4. According to the previous inference, can we conclude whether there is an analogy between the two narratives?  The answer is {YES}."  
    },  
    "Example2": {  
        "instruction": "To judge whether there is an analogy between the following two paragraphs. Answer {YES} or {NO}.",  
        "input": "First section: {Johnny was having a very hard time at work. He had too many projects and too many short deadlines, and he was stressed. He kept working as hard as he could to finish everything, and it paid off. His boss noticed how hard he was working and offered him a raise, as well as his choice in future projects.}, Second section: {The man had been in the job for ten years and had never had a payrise, despite his diligent hard work. One day he plucked up the courage to ask his boss for a rage and was curtly refused. That was it! He decided. No more working his hardest. He was going to slack as much as he could and not take any further pride in his work.}",  
        "output": "Let's think step by step. 1.  Extract the important elements in the narration and ignore the secondary elements. Return a list of {necessary narrative element} in each given narration. First section: {Characters = A worker and a boss; Relationships =The relationship between boss and worker; Locations = workplace; actions = The worker works hard, and the boss promotes the worker; Goals = The worker completes the work},  Second section: {Characters = A worker and a boss; Relationships = The relationship between boss and worker; Locations = workplace; Actions = The worker works hard, is refused a raise, and decides to stop working diligently; Goals = The worker stops working hard} 2. Summarize high-level information. First section: {No pain, no gain.}, Second section:{Hard work does not always pay off.} 3. Comparing the high-level information of the two narratives, do they have a correspondence? They have no corresponding relationship. 4. According to the previous inference, can we conclude whether there is an analogy between the two narratives ? The answer is {NO}"  
    },  
    "Question": {  
        "instruction": "To judge whether there is an analogy between the following two paragraphs. Answer {YES} or {NO}.",  
        "input": "First section: {query_narrative}, Second section: {first_choice}",  
        "output": ""  
    }  
}

下面的是格式没调整的版本
Prompt = f"{
Example1 = {
	instruction: 'To judge whether there is an analogy between the following two paragraphs, only answer yes or no.', 
	input: 'First section: {He really hated school. The bullying was relentless, but he had a strong fortitude and a promise to make his single mother proud. So he bravely marched on to school. In a matter of years, he found himself graduating at the top of his class with an M.B.A. His former classmates went to their jobs cleaning up after school children.}, Second section: When I remember the challenges I went through when I was starting my business, I break into tears. But I do not regret a thing. I think that the most precious gold goes through the hottest furnace. There are great and unforgettable lessons that I learned during that period that I will always cherish. It made me better.}', 
	output: 'Let's think step by step.1. Extract the important elements in the narration and ignore the secondary elements. Return a list of {necessary narrative element} in each given narration. First section: {Characters = A student and his former classmates; Relationships = The student and his single mother; Locations = School; Actions = The student endures bullying, studies hard, graduates with honors, and surpasses his classmates; Goals = To make his mother proud and succeed academically}.  Second section: {Characters = A businessperson; Relationships = Unspecified; Locations = The early stages of a business; Actions = The businessperson faces challenges, perseveres, learns valuable lessons, and grows stronger; Goals = To overcome difficulties and achieve personal growth}. 2. Summarize high-level information.  First section: {Perseverance and hard work lead to success and triumph over adversity.}  Second section: {Challenges and hardships shape resilience and personal growth.} 3. Comparing the high-level information of the two narratives, do they have a correspondence?  Yes, both emphasize enduring difficulties and emerging stronger or more successful as a result. 4. According to the previous inference, can we conclude whether there is an analogy between the two narratives?  The answer is {YES}.'
	}, 
Example2 = {
	instruction: 'To judge whether there is an analogy between the following two paragraphs, only answer yes or no.', 
	input: 'First section: {Johnny was having a very hard time at work. He had too many projects and too many short deadlines, and he was stressed. He kept working as hard as he could to finish everything, and it paid off. His boss noticed how hard he was working and offered him a raise, as well as his choice in future projects.}, Second section: {The man had been in the job for ten years and had never had a payrise, despite his diligent hard work. One day he plucked up the courage to ask his boss for a rage and was curtly refused. That was it! He decided. No more working his hardest. He was going to slack as much as he could and not take any further pride in his work.}', 
	output: 'Let's think step by step. 1.  Extract the important elements in the narration and ignore the secondary elements. Return a list of {necessary narrative element} in each given narration. First section: {Characters = A worker and a boss; Relationships =The relationship between boss and worker; Locations = workplace; actions = The worker works hard, and the boss promotes the worker; Goals = The worker completes the work},  Second section: {Characters = A worker and a boss; Relationships = The relationship between boss and worker; Locations = workplace; Actions = The worker works hard, is refused a raise, and decides to stop working diligently; Goals = The worker stops working hard} 2. Summarize high-level information. First section: {No pain, no gain.}, Second section:{Hard work does not always pay off.} 3. Comparing the high-level information of the two narratives, do they have a correspondence? They have no corresponding relationship. 4. According to the previous inference, can we conclude whether there is an analogy between the two narratives ? The answer is {NO}'
	}, 
Question = {
	instruction: 'To judge whether there is an analogy between the following two paragraphs, only answer yes or no.', 
	input: 'First: {row['query_narrative']}, Second: {row['first_choice']}', 
	output: ''
	}
}"


----
原始prompts
### 1. 提取叙事元素

Return a list of {element} in the given narrative.

附有3个随机解答的演示文稿，并附有作者手工提取的叙述性及其提取的元素。

### 2. 对Llama 2、GPT3.5和GPT4.0模型给出的主要零样本和少样本实验

对Llama 2、GPT3.5和GPT4.0模型给出的主要零样本和少样本实验的提示由多个部分组成：1 .定义了任务所在的组件，并提供了模型返回答案应该使用的模板的一些细节；2 .提示(显示为较浅的颜色)中包含演示文稿的成分，该成分在零拍设置中不存在，在小拍设置中根据指定的演示文稿数量重复多次。3 .表示模型要解决的问题的组件：

Narratives can be mapped to each other in terms of the high-level message they strive to convey. These high-level messages can be related to traditions, common knowledge, or moral principles. We call this mapping analogical mapping. Which one of the two narratives (1, 2) can create a better analogical mapping with the query narrative? Answer in the template: {{narrative x, because narrative x and query narrative are ...}}
--------query narrative: {query narrative demonstration} 
--------narrative 1: {first candidate demonstration} 
--------narrative 2: {second candidate demonstration} 
########## 
{Answer provided with its corresponding explanation} 
--------query narrative: {query narrative} 
--------narrative 1: {first candidate} 
--------narrative 2: {second candidate} 
########## 
narrative

### 3. 零样本实验中使用的Macaw、Unified QA和Flan T5模型给出的提示

零样本实验中使用的Macaw、Unified QA和Flan T5模型给出的提示与GPT3.5、GPT4.0和Llama 2模型使用的提示不同，这是因为它们在微调时使用了特定的指令模板。提示的内容与另一个提示相同；但是，为了采用Macaw (塔菲约德和Clark , 2021)、Unified QA ( Khashabi et al , 2020)和Flan T5 ( Chung et al , 2024)中讨论的问答设置，我们对提示语做了如下稍微的修改：

Narratives can be mapped to each other in terms of the high-level message they strive to convey. This high-level message can be related to traditions, common knowledge, or moral principles. We call this mapping analogical mapping. Which one of the two narratives (a, b) can create a better analogical mapping with the query narrative? query narrative: {query narrative}. \\n (a) {first candidate} (b) {second candidate}

### 4. 生成近类比

Shortly paraphrase or give a short synonym for {element content} without any explanations.

Generate a random narrative with 4-8 sentences that uses {elements type and their content}. THE NARRATIVE MUST HAVE THE SAME CONCLUSION AS THE PROVERB: {given proverb}. Only return the narrative without any explanation, and also DO NOT mention the proverb.



