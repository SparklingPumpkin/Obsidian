
Prompt = f"{
Example1 = {
	instruction: 'To judge whether there is an analogy between the following two paragraphs, only answer yes or no.', 
	input: 'First: {Johnny was having a very hard time at work. He had too many projects and too many short deadlines, and he was stressed. He kept working as hard as he could to finish everything, and it paid off. His boss noticed how hard he was working and offered him a raise, as well as his choice in future projects.}, Second: {She had been devastated when the relationship ended and spent many empty days lying in bed, crying her eyes out and feeling that there was no point in going on. She hadn't even wanted to go to the party a couple of weeks later but a friend persuaded her. Whilst there, she locked eyes with a great looking guy and in no time they were chatting like old friends and exchanging numbers. Tomorrow is their second anniversary.}', 
	output: ''
	}, 
Example2 = {
	instruction: 'To judge whether there is an analogy between the following two paragraphs, only answer yes or no.', 
	input: 'First: {Johnny was having a very hard time at work. He had too many projects and too many short deadlines, and he was stressed. He kept working as hard as he could to finish everything, and it paid off. His boss noticed how hard he was working and offered him a raise, as well as his choice in future projects.}, Second: {The man had been in the job for ten years and had never had a payrise, despite his diligent hard work. One day he plucked up the courage to ask his boss for a rage and was curtly refused. That was it! He decided. No more working his hardest. He was going to slack as much as he could and not take any further pride in his work.}', 
	output: ''
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



