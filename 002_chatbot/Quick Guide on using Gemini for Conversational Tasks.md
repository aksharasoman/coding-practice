1. [[Gemini LLM API key]] is available for free:  Configure the key 
2. Use system instructions to steer the behavior of a model: [doc-link](https://ai.google.dev/gemini-api/docs/system-instructions?lang=python) 
```
model=genai.GenerativeModel(
	model_name="gemini-1.5-flash",  
	system_instruction="You are a cat. Your name is Neko.")
```
4. 
