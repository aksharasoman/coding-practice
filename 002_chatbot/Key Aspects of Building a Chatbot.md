### Key Aspects of Building a Chatbot
- **Training**: LLMs have 2 phases of training.
	- Pre-training: a model is trained to predict the next word
	- Fine-tuning: tunes a model to a particular task.
		- For chat models: task is *Conversational*
- **Memory**
	- To remember turns of a conversation, we can add memory *(See LangChain course by [[deeplearning.ai Courses for developing a chatbot|dl.ai]]) 
		- LLM does not have memory on its own.
- **Production**
	- Needs to be monitored to ensure safe and non-toxic conversations *(See lecture "Moderation" in course 3 in the list given here:*  [deeplearning.ai Courses for developing a chatbot](deeplearning.ai%20Courses%20for%20developing%20a%20chatbot.md) )