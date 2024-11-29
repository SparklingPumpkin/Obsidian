
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

### 2. 

narratives can be mapped to each other in terms of the high-level message they strive to convey. These high-level messages can be related to traditions, common knowledge, or moral principles. We call this mapping analogical mapping. Which one of the two narratives (1, 2) can create a better analogical mapping with the query narrative? Answer in the template: {{narrative x, because narrative x and query narrative are ...}}
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