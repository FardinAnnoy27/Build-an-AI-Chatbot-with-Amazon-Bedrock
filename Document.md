# ⚡️ 30 Second Summary

Every time you chat with an AI assistant, there is a powerful cloud service working behind the scenes. But how does that actually work under the hood? 🤔

In this project, you will build an **AI-powered chatbot** using **Amazon Bedrock** and **Python** 🐍. You will start by exploring foundation models in the AWS Console, making your first API call with `boto3`, and extending a simple script into a multi-turn chatbot with conversation history, system prompts, and tunable inference parameters. 🚀

---

## 🛠️ What You'll Build

You'll build a Python chatbot in **AWS CloudShell** that uses Amazon Bedrock's **Converse API** to chat with the **Amazon Nova 2 Lite** foundation model, maintaining conversation history across multiple messages. 💬
<img width="1176" height="369" alt="" src="https://learn.nextwork.org/projects/static/aws-genai-bedrock-chatbot/screenshot-63.webp" />

By the end of this project, you'll have:

* 🧠 **A hands-on exploration** of the Amazon Bedrock Chat playground to interact with AI foundation models.
* 🐍 **A Python script** that calls the Bedrock Converse API from AWS CloudShell.
* 💬 **A multi-turn chatbot** that maintains conversation history across messages.
* 🎭 **A custom system prompt** that gives your chatbot a specific personality and role.
* 💎 **Secret Mission:** Add Guardrails for Responsible AI to filter harmful content and enforce safety policies. 🛡️

---

## 🌐 Meet Amazon Bedrock

You're about to build an AI-powered chatbot, but before writing any code, you need to understand the service that powers it. **Amazon Bedrock** gives you access to over 100 **foundation models (FMs)** from companies like Amazon, Anthropic, and Meta through a single **API**. No servers to manage, no infrastructure to provision! 🎉

In this step, you'll sign in to the **AWS Management Console**, explore the models available in Bedrock, and send your first prompt to an AI model using the built-in Chat playground. 🎨

### 🎯 In this step, get ready to:

* 🧭 Navigate to **Amazon Bedrock** in the AWS Console.
* 🔍 Explore foundation models in the **Model catalog**.
* 💬 Chat with **Nova 2 Lite** in the playground.

---

### 🔑 Sign In and Navigate to Amazon Bedrock

1. Navigate to the **AWS Management Console** in your browser. 💻
2. Check the region selector in the top right corner of the console. 🌍
3. Select **US East (N. Virginia) `us-east-1**` if it is not already selected.
4. Click the search bar at the top of the console. 🔍
5. Type **Bedrock**.
6. Select **Amazon Bedrock** from the search results. 🚀
7. Check that you see the Amazon Bedrock overview page. ✨

---

### 📚 Explore the Model Catalog

1. In the left sidebar, click **Model catalog** (under *Discover*). 📂
2. You should see a list of models from different providers, each with different strengths, pricing, and capabilities. 📊

---

### 💬 Chat with an AI Model

1. In the left sidebar, click **Chat / Text playground** (under *Test*). 🎮
2. Click **Select model**. 🎯
3. Under *Categories*, select **Amazon**.
4. Select **Nova 2 Lite**.
5. Click **Apply**. ✅
6. In the prompt box at the bottom, type the following:
> Explain cloud computing in 3 sentences


7. Click the **Run** button. ⚡
8. The model will generate a response based on your prompt. 🔥

---

## 💻 Your First AI API Call

Now it is time to write code! 🧑‍💻 You will use **Python** to write a short script that sends a question to **Amazon Bedrock** and prints the answer. 📝

### 🎯 In this step, get ready to:

* 🐚 Open **AWS CloudShell** and verify your environment.
* ⚙️ Write a Python script that calls the **Bedrock Converse API**.
* 🏃‍♂️ Run the script and see an AI-generated response.

---

### 🐚 Open AWS CloudShell

1. Click the **terminal icon** in the top navigation bar of the AWS Console. 🖥️
2. Wait for AWS CloudShell to initialize until you see the terminal prompt. ⏳
3. Verify Python is installed by running:
```bash
python3 --version

```


4. Verify `boto3` is available by running:
```bash
python3 -c "import boto3; print(boto3.__version__)"

```



---

### 📝 Write Your Bedrock Script

1. In your CloudShell terminal, create a new file: 📑
```bash
nano bedrock_chat.py

```


> 💡 **Note:** This opens `nano`, a text editor that runs directly in your terminal. Think of it like Notepad, but inside the command line. You type and edit code here, then save and exit back to the terminal.


2. Copy and paste the following setup code into `nano`: 📋

```python
import boto3

# 1. Connect to Amazon Bedrock
client = boto3.client("bedrock-runtime", region_name="us-east-1")

# 2. Choose which AI model to use
model_id = "amazon.nova-lite-v1:0"

# 3. Create your prompt
messages = [
    {
        "role": "user",
        "content": [{"text": "What is cloud computing? Explain in 2 sentences."}]
    }
]

```

3. Still in `nano`, add the following code **below** your `messages` list to call the API and print the response: 👇

```python
# 4. Send the message and get a response
response = client.converse(modelId=model_id, messages=messages)

# 5. Extract the text and print it
response_text = response["output"]["message"]["content"]["text"]
print(response_text)

```

4. 💾 **Save the file:**
* Press `Ctrl+O`, then `Enter` to confirm the filename.


5. 🚪 **Exit nano:**
* Press `Ctrl+X`.



---

### 🏃‍♂️ Run Your Script

Back in CloudShell, run your script: ⚡

```bash
python3 bedrock_chat.py

```

---

## 🤖 Create Your Chatbot Script

Your previous script (`bedrock_chat.py`) is finished. You will now create a brand new script for the chatbot! 🛠️

1. In your CloudShell terminal, create a new file for your chatbot: 📑
```bash
nano bedrock_chat_revised.py

```


2. Copy and paste the following setup code into `nano`: 📋

```python
import boto3

# 1. Connect to Bedrock and choose the model
client = boto3.client("bedrock-runtime", region_name="us-east-1")
model_id = "amazon.nova-lite-v1:0"

# 2. Give the chatbot a personality
system_prompt = [{"text": "You are a friendly cloud computing tutor. Explain concepts simply and use analogies."}]

# 3. Store conversation history
messages = []

```

3. Still in `nano`, add the conversation loop **below** the setup code: 👇

```python
# 4. Start the conversation loop
print("Chatbot ready! Type 'quit' to exit.")

while True:
    user_input = input("You: ")
    if user_input.lower() in ["quit", "exit"]:
        print("Goodbye!")
        break

    # 5. Add the user's message to conversation history
    messages.append({"role": "user", "content": [{"text": user_input}]})

    # 6. Send the full conversation to Bedrock
    response = client.converse(
        modelId=model_id,
        messages=messages,
        system=system_prompt,
        inferenceConfig={
            "temperature": 0.7,   # 0.0 = predictable, 1.0 = creative
            "topP": 0.9,          # 0.0-1.0, filters word choices
            "maxTokens": 512      # max response length
        }
    )

    # 7. Save the response and print it
    assistant_message = response["output"]["message"]
    messages.append(assistant_message)

    print(f"Bot: {assistant_message['content']['text']}")

```

4. 💾 **Save the file:**
* Press `Ctrl+O`, then `Enter` to confirm.


5. 🚪 **Exit nano:**
* Press `Ctrl+X` to exit nano.



---

### 🧪 Test Your Chatbot

Back in CloudShell, run your chatbot: ⚡

```bash
python3 bedrock_chat_revised.py

```

Now we're going to test **multi-turn conversation** by asking a question, then a follow-up that references the previous answer. 🔄

1. Type the following prompt: ✍️
```text
What is an S3 bucket?

```


2. Then send the same follow-up prompt: 🔄
```text
Can you give me an analogy for that?

```



> 💡 **Why this works:** The chatbot should reference its previous answer about S3 in the analogy. This works because your script sends the full `messages` list on every call, giving the model context to understand follow-up questions! 🧠

---

### 🧪 Experiment with Temperature

Your chatbot currently uses a temperature of `0.7`. What happens if you turn that down? 🌡️👇

1. Type `quit` to exit your chatbot if it is still running.🚪
2. Open the script again: 📑
```bash
nano bedrock_chat_revised.py

```


3. Use the arrow keys ⬆️⬇️ to find the `inferenceConfig` section in your code. Look for the line that says `"temperature": 0.7`.
4. Change `0.7` to `0.1`. 📉
5. Save with `Ctrl+O`, press `Enter`, and exit with `Ctrl+X`. 💾
6. Run the chatbot again: ⚡
```bash
python3 bedrock_chat_revised.py

```


7. Ask the same question as before: ✍️
```text
What is an S3 bucket?

```


8. Type `quit` to exit the chatbot when you are finished testing! 🚪🤩
