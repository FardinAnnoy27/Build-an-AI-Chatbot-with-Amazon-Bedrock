⚡️ 30 Second Summary

Every time you chat with an AI assistant, there is a powerful cloud service working behind the scenes. But how does that actually work under the hood?

In this project, you will build an AI-powered chatbot using Amazon Bedrock and Python, starting from exploring foundation models in the AWS Console, making your first API call with boto3, and extending a simple script into a multi-turn chatbot with conversation history, system prompts, and tunable inference parameters.

What You'll Build

You'll build a Python chatbot in AWS CloudShell that uses Amazon Bedrock's Converse API to chat with the Amazon Nova 2 Lite foundation model, maintaining conversation history across multiple messages.

<img width="1176" height="369" alt="" src="https://learn.nextwork.org/projects/static/aws-genai-bedrock-chatbot/screenshot-63.webp" />

By the end of this project, you'll have:
🧠 A hands-on exploration of the Amazon Bedrock Chat playground to interact with AI foundation models.
🐍 A Python script that calls the Bedrock Converse API from AWS CloudShell.
💬 A multi-turn chatbot that maintains conversation history across messages.
🎭 A custom system prompt that gives your chatbot a specific personality and role.
💎 Secret Mission: Add Guardrails for Responsible AI to filter harmful content and enforce safety policies. 

Meet Amazon Bedrock

You're about to build an AI-powered chatbot, but before writing any code, you need to understand the service that powers it.  gives you access to over 100  from companies like Amazon, Anthropic, and Meta through a single . No servers to manage, no infrastructure to provision.

In this step, you'll sign in to the , explore the models available in Bedrock, and send your first prompt to an AI model using the built-in Chat playground.


In this step, get ready to:

Navigate to  in the AWS Console.
Explore foundation models in the Model catalog.
Chat with  in the playground.
Sign In and Navigate to Amazon Bedrock
Navigate to the AWS Management Console in your browser.
<img width="1176" height="369" alt="" src="https://learn.nextwork.org/projects/static/aws-genai-bedrock-chatbot/step1/screenshot-2.webp" />
Check the region selector in the top right corner of the console.
Select US East (N. Virginia) us-east-1 if it is not already selected.

Click the search bar at the top of the console.
Type Bedrock.
Select Amazon Bedrock from the search results.
<img width="1176" height="369" alt="" src="https://learn.nextwork.org/projects/static/aws-genai-bedrock-chatbot/step1/screenshot-4.webp" />
Check that you see the Amazon Bedrock overview page.

<img width="1176" height="369" alt="" src="https://learn.nextwork.org/projects/static/aws-genai-bedrock-chatbot/step1/screenshot-5.webp" />
Explore the Model Catalog
In the left sidebar, click Model catalog (under Discover).
<img width="1176" height="369" alt="" src="https://learn.nextwork.org/projects/static/aws-genai-bedrock-chatbot/step1/screenshot-6.webp" />
You should see a list of models from different providers, each with different strengths, pricing, and capabilities.
<img width="1176" height="369" alt="" src="https://learn.nextwork.org/projects/static/aws-genai-bedrock-chatbot/step1/screenshot-7.webp" />
Chat with an AI Model
In the left sidebar, click Chat / Text playground (under Test).
<img width="1176" height="369" alt="" src="https://learn.nextwork.org/projects/static/aws-genai-bedrock-chatbot/step1/screenshot-8.webp" />
Click Select model.
<img width="1176" height="369" alt="" src="https://learn.nextwork.org/projects/static/aws-genai-bedrock-chatbot/step1/screenshot-9.webp" />
Under Categories, select Amazon.
Select Nova 2 Lite.
Click Apply.
<img width="1176" height="369" alt="" src="https://learn.nextwork.org/projects/static/aws-genai-bedrock-chatbot/step1/screenshot-10.webp" />
In the prompt box at the bottom, type the following:Explain cloud computing in 3 sentences
Click the Run button.
<img width="1176" height="369" alt="" src="https://learn.nextwork.org/projects/static/aws-genai-bedrock-chatbot/step1/screenshot-11.webp" />
The model will generate a response based on your prompt.
<img width="1176" height="369" alt="" src="https://learn.nextwork.org/projects/static/aws-genai-bedrock-chatbot/step1/screenshot-12.webp" />
Your First AI API Call

Now it is time to write code. You will use  to write a short  script that sends a question to  and prints the answer.


In this step, get ready to:

Open AWS Cloudshell and verify your environment.
Write a Python script that calls the Bedrock Converse API .
Run the script and see an AI-generated response.

Open AWS CloudShell
Click the terminal icon in the top navigation bar of the AWS Console.
<img width="1176" height="369" alt="" src="https://learn.nextwork.org/projects/static/aws-genai-bedrock-chatbot/step2/screenshot-13.webp" />
Wait for AWS CloudShell to initialize until you see the terminal prompt.
<img width="1176" height="369" alt="" src="https://learn.nextwork.org/projects/static/aws-genai-bedrock-chatbot/step2/screenshot-14.webp" />
Verify Python is installed by running:python3 --version
Verify boto3 is available by running:python3 -c "import boto3; print(boto3.__version__)"
Write Your Bedrock Script
In your CloudShell terminal, create a new file:nano bedrock_chat.py
This opens nano, a text editor that runs directly in your terminal. Think of it like Notepad, but inside the command line. You type and edit code here, then save and exit back to the terminal.

Copy and paste the following setup code into nano.
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
<img width="1176" height="369" alt="" src="https://learn.nextwork.org/projects/static/aws-genai-bedrock-chatbot/step2/screenshot-22.webp" />

Still in nano, add the following code below your messages list to call the API and print the response:
# 4. Send the message and get a response
response = client.converse(modelId=model_id, messages=messages)

# 5. Extract the text and print it
response_text = response["output"]["message"]["content"][0]["text"]
print(response_text)
<img width="1176" height="369" alt="" src="https://learn.nextwork.org/projects/static/aws-genai-bedrock-chatbot/step2/screenshot-23.webp" />
Save the file:
Press Ctrl+O, then Enter to confirm the filename.
Exit nano:
Press Ctrl+X.

Run Your Script
Back in CloudShell, run the script:
```bash
python3 bedrock_chat.py

```

Create Your Chatbot Script

Your previous script (bedrock_chat.py) is finished. You will now create a brand new script for the chatbot.

In your CloudShell terminal, create a new file for your chatbot:

```bash
nano bedrock_chat_revised.py

```

Copy and paste the following setup code into nano.

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
Still in nano, add the conversation loop below the setup code:
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

    print(f"Bot: {assistant_message['content'][0]['text']}")


Save the file:





Press Ctrl+O, then Enter to confirm.



Press Ctrl+X to exit nano.
Test Your Chatbot





Back in CloudShell, run your chatbot:

```bash
python3 bedrock_chat_revised.py

```
ow we're going to test multi-turn conversation by asking a question, then a follow-up that references the previous answer.





Type the following prompt:

```prompt
What is an S3 bucket?

```





Then send the same follow up prompt:

```prompt
Can you give me an analogy for that?

```

The chatbot should reference its previous answer about S3 in the analogy. This works because your script sends the full messages list on every call, giving the model context to understand follow-up questions.

Experiment with Temperature

Your chatbot currently uses a temperature of 0.7. What happens if you turn that down?





Type quit to exit your chatbot if it is still running.



Open the script again:

```bash
nano bedrock_chat_revised.py

```
<img width="1176" height="369" alt="" src="https://learn.nextwork.org/projects/static/aws-genai-bedrock-chatbot/step3/screenshot-31.webp" />
Use the arrow keys to find the inferenceConfig section in your code. Look for the line that says "temperature": 0.7.



Change 0.7 to 0.1.
<img width="1176" height="369" alt="" src="https://learn.nextwork.org/projects/static/aws-genai-bedrock-chatbot/step3/screenshot-32.webp" />
Save with Ctrl+O, press Enter, and exit with Ctrl+X.



Run the chatbot again:

```bash
python3 bedrock_chat_revised.py

```





Ask the same question as before:

```prompt
What is an S3 bucket?

```
Type quit to exit the chatbot when you are finished testing.




