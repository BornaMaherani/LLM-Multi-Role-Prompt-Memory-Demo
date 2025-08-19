#  LLM Multi‑Role Prompt & Memory Demo

> A mini‑project showcasing how to work with Large Language Model (LLM) APIs, send dynamic prompts for different AI roles & models, and implement conversation memory through summarization.

##  Project Overview
This project demonstrates:
- Sending prompts to LLM APIs with different **roles** and **models**.
- Automating prompt generation for 4 predefined AI personas.
- Reading an AI‑generated email from a text file and processing it.
- Implementing **conversational memory** using summarization.
- Running interactive multi‑turn chats while preserving important context.

The notebook is structured in **two main parts**:

### **Part 1 — Multi‑Role Prompting**
You can send one piece of content to different roles such as:
-  Detective — e.g., counting words starting with a certain letter.
-  Sarcastic Persona — making witty guesses about content origins.
-  Literary Scholar — rating and reviewing.
-  Doctor — inferring possible health traits from writing.

The project:
1. Reads an AI‑generated email (`email.txt`).
2. Creates a `role_action_map` linking each role to a specific task prompt.
3. Sends role‑specific prompts to chosen models via the OpenRouter API.
4. Prints model responses that highlight the differences between personas.

### **Part 2 — Conversational Memory Implementation**
Demonstrates how to preserve context in multi‑turn conversations by **summarizing prior chat history** when it grows too long.

Features:
- Uses a `summary_threshold` to switch from raw history to a summarized memory.
- Summarization prompt requests Persian output (customizable).
- Loops through chat turns until the user enters `EXIT`.
- Shows how memory compression allows long chatting without hitting token limits.

##  Repository Structure
```
├── sample.ipynb          # Main Jupyter Notebook with code & execution examples
├── email.txt             # AI‑generated sample email
├── summarized_response.txt # Example summarization output from roles
└── README.md             # Project documentation (this file)
```

##  Getting Started

### 1 Prerequisites
Make sure you have:
- Python 3.9+
- [pip](https://pip.pypa.io/en/stable/)
- An API key from [OpenRouter](https://openrouter.ai/)

### 2️ Install Dependencies
```bash
pip install openai python-dotenv
```

### 3️ Environment Setup
Create a `.env` file with your API key:
```bash
OPENROUTER_API_KEY=your_api_key_here
```

Or set it directly in `sample.ipynb` ( not recommended for public repos):
```python
client = OpenAI(
    base_url="https://openrouter.ai/api/v1",
    api_key="sk-or-v1-...",
)
```

### 4️ Run the Notebook
Open `sample.ipynb` in Jupyter and run all cells.
```bash
jupyter notebook sample.ipynb
```

---

##  How It Works

### **Prompt Sending Function**
```python
def SendPrompt(system_role, prompt, model_name):
    completion = client.chat.completions.create(
        model=model_name,
        messages=[
            {"role": "system", "content": f"You are {system_role} AI assistant."},
            {"role": "user", "content": prompt},
        ],
    )
    return completion.choices[0].message.content
```
This wraps the API call, prepping a system prompt + user prompt combo.

### **Conversational Memory Flow**
- Tracks conversation as a `conversation_history` list.
- After `summary_threshold` turns, calls `get_summary()` to condense history.
- Continues chatting with the summary as the base context.

---

##  Example Run
### Role‑based Outputs (Sample)
- **Detective:** Counts words starting with "e".
- **Sarcastic Persona:** Guesses which AI model wrote the email.
- **Literary Scholar:** Gives numeric and qualitative review.
- **Doctor:** Suggests possible personality traits.

---

##  Technologies Used
- **Python 3.9+**
- **Jupyter Notebook**
- **OpenAI / OpenRouter API** for LLM calls

---

##  Contributing
Pull requests are welcome! Please:
1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

##  License
This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---
