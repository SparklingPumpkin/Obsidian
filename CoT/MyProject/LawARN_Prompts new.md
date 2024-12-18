json格式Prompt
{  
    "Example1": {  
        "instruction": "Step-by-step, analyse the following two paragraphs in detail according to the given format, determine whether an analogy exists between them, and provide your answer as {YES} or {NO}.",  
        "input": "First section: {He really hated school. The bullying was relentless, but he had a strong fortitude and a promise to make his single mother proud. So he bravely marched on to school. In a matter of years, he found himself graduating at the top of his class with an M.B.A. His former classmates went to their jobs cleaning up after school children.}, Second section: {When I remember the challenges I went through when I was starting my business, I break into tears. But I do not regret a thing. I think that the most precious gold goes through the hottest furnace. There are great and unforgettable lessons that I learned during that period that I will always cherish. It made me better.}",  
        "output": "Let's think step by step.
        1. For each narrative paragraph, extract the concrete narrative elements and return a CONCRETE LIST .For narrative elements that do not appear, record them as None.
		    First section: CONCRETE LIST = {Characters = He & classmates; Relationships = classmate relationship; Locations = School; Actions = The student endures bullying, studies hard, graduates with honors, and surpasses his classmates; Goals =Graduating at the top of his class with an M.B.A}. 
		    Second section: CONCRETE LIST = {Characters = I; Relationships = None; Locations = The early stages of a business; Actions = I faced challenges, persevered, learned valuable lessons, and grew stronger; Goals = Make me a better person}. 
		2.  For each narrative paragraph, abstract the narrative elements extracted in the first step, retaining only the attributes relevant to the events, and return an ABSTRACT LIST. 
			First section: ABSTRACT LIST = {Characters = An individual and his competitors; Relationships = competitive relation; Locations = An occasion focused on competition and growth; Actions = Overcome difficulties. Try to improve himself. Win in the competition; Goals = Achieve personal growth}.
			Second section: ABSTRACT LIST = {Characters = A businessperson; Relationships = None; Locations = An occasion focused on competition and growth; Actions = Overcome difficulties; Goals = Achieve personal growth}. 
		3. For each paragraph, determine whether the abstract elements extracted in the second step play a crucial role in the overall event and significantly impact the outcome. If there are any unimportant elements, return a list of unimportant elements. If all elements are important, return an empty list.
			First section: UNIMPORTANT LIST = {}.
			Second section: UNIMPORTANT LIST = {Characters; Relationships}. 
		4. Combining steps two and three, utilize all critical narrative elements while ignoring other narrative elements, and summarize the high-level information and {} for each narrative paragraph.
			First section: 
				high-level information = {An individual overcomes difficulties, surpasses competitors, and achieves personal growth.}  
				The core logical relationship = {An individual overcomes difficulties, surpasses competitors} - causality - {achieves personal growth}
			Second section: 
				high-level information = {An individual overcomes difficulties in a competitive relationship and achieves personal growth.} 
				The core logical relationship = {An individual overcomes difficulties} - causality - {achieves personal growth}
		5. By comparing and analysing the high-level information and the core logical relationship of the two narrative paragraphs, the conclusion is drawn : are they enough to form an analogy ?
			Both narrative paragraphs are because individuals overcome difficulties and lead to individual growth, forming a causal relationship. This is enough to form analogical reasoning relationship.
		6. According to the previous inference, can we conclude whether there is an analogy between the two narratives?  
			The answer is {YES}."
    },  
    "Example2": {  
        "instruction": "Step-by-step, analyse the following two paragraphs in detail according to the given format, determine whether an analogy exists between them, and provide your answer as {YES} or {NO}.",  
        "input": "First section: {He really hated school. The bullying was relentless, but he had a strong fortitude and a promise to make his single mother proud. So he bravely marched on to school. In a matter of years, he found himself graduating at the top of his class with an M.B.A. His former classmates went to their jobs cleaning up after school children.}, Second section: {When I remember the challenges I went through when I was starting my business, I break into tears. But I do not regret a thing. I think that the most precious gold goes through the hottest furnace. There are great and unforgettable lessons that I learned during that period that I will always cherish. It made me better.}",  
        "output": "Let's think step by step.
        1. For each narrative paragraph, extract the concrete narrative elements and return a CONCRETE LIST .For narrative elements that do not appear, record them as None.
		    First section: CONCRETE LIST = {Characters = He & classmates; Relationships = classmate relationship; Locations = School; Actions = The student endures bullying, studies hard, graduates with honors, and surpasses his classmates; Goals =Graduating at the top of his class with an M.B.A}. 
		    Second section: CONCRETE LIST = {Characters = I; Relationships = None; Locations = The early stages of a business; Actions = I faced challenges, persevered, learned valuable lessons, and grew stronger; Goals = Make me a better person}. 
		2.  For each narrative paragraph, abstract the narrative elements extracted in the first step, retaining only the attributes relevant to the events, and return an ABSTRACT LIST. 
			First section: ABSTRACT LIST = {Characters = An individual and his competitors; Relationships = competitive relation; Locations = An occasion focused on competition and growth; Actions = Overcome difficulties. Try to improve himself. Win in the competition; Goals = Achieve personal growth}.
			Second section: ABSTRACT LIST = {Characters = A businessperson; Relationships = None; Locations = An occasion focused on competition and growth; Actions = Overcome difficulties; Goals = Achieve personal growth}. 
		3. For each paragraph, determine whether the abstract elements extracted in the second step play a crucial role in the overall event and significantly impact the outcome. If there are any unimportant elements, return a list of unimportant elements. If all elements are important, return an empty list.
			First section: UNIMPORTANT LIST = {}.
			Second section: UNIMPORTANT LIST = {Characters; Relationships}. 
		4. Combining steps two and three, utilize all critical narrative elements while ignoring other narrative elements, and summarize the high-level information and {} for each narrative paragraph.
			First section: 
				high-level information = {An individual overcomes difficulties, surpasses competitors, and achieves personal growth.}  
				The core logical relationship = {An individual overcomes difficulties, surpasses competitors} - causality - {achieves personal growth}
			Second section: 
				high-level information = {An individual overcomes difficulties in a competitive relationship and achieves personal growth.} 
				The core logical relationship = {An individual overcomes difficulties} - causality - {achieves personal growth}
		5. By comparing and analysing the high-level information and the core logical relationship of the two narrative paragraphs, the conclusion is drawn : are they enough to form an analogy ?
			Both narrative paragraphs are because individuals overcome difficulties and lead to individual growth, forming a causal relationship. This is enough to form analogical reasoning relationship.
		6. According to the previous inference, can we conclude whether there is an analogy between the two narratives?  
			The answer is {YES}."
    },  
    "Question": {  
        "instruction": "Step-by-step, analyse the following two paragraphs in detail according to the given format, determine whether an analogy exists between them, and provide your answer as {YES} or {NO}.", 
        "input": "First section: {query_narrative}, Second section: {first_choice}",  
        "output": ""  
    }  
}




