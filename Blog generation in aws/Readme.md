# AWS Bedrock Blog Generator

This project demonstrates how to generate a blog using **AWS Bedrock** and the **Meta Llama2 model** with Python.

The script sends a prompt to the Bedrock foundation model and returns a generated blog based on the provided topic.

---

## 🚀 Features

* Uses **AWS Bedrock Runtime API**
* Generates blogs using **Meta Llama2 13B Chat Model**
* Configurable generation parameters
* Error handling for failed API calls
* Easy integration with Python applications

---

## 🧠 How It Works

1. The user provides a **blog topic**.
2. A prompt is created using the topic.
3. The prompt is sent to **AWS Bedrock Llama2 model**.
4. The model generates a response.
5. The response is parsed and returned as a blog.

---

## 📂 Project Structure

```
bedrock-blog-generator/
│
├── app.py
├── requirements.txt
└── README.md
```

---

## ⚙️ Requirements

* Python 3.8+
* AWS Account
* AWS Bedrock access enabled
* AWS CLI configured

---

## 📦 Install Dependencies

Create a virtual environment and install required packages.

```bash
pip install boto3 botocore
```

Or create a **requirements.txt**

```
boto3
botocore
```

Install with:

```bash
pip install -r requirements.txt
```

---

## 🔑 Configure AWS Credentials

Make sure your AWS credentials are configured.

```bash
aws configure
```

Provide:

```
AWS Access Key
AWS Secret Key
Region (example: us-east-1)
```

---

## 🧩 Code Example

```python
def blog_generate_using_bedrock(blogtopic:str)-> str:
    prompt=f"""<s>[INST]Human: Write a 200 words blog on the topic {blogtopic}
    Assistant:[/INST]
    """

    body={
        "prompt":prompt,
        "max_gen_len":512,
        "temperature":0.5,
        "top_p":0.9
    }

    try:
        bedrock=boto3.client("bedrock-runtime",region_name="us-east-1",
                             config=botocore.config.Config(read_timeout=300,retries={'max_attempts':3}))
        response=bedrock.invoke_model(body=json.dumps(body),modelId="meta.llama2-13b-chat-v1")

        response_content=response.get('body').read()
        response_data=json.loads(response_content)

        blog_details=response_data['generation']
        return blog_details

    except Exception as e:
        print(f"Error generating the blog:{e}")
        return ""
```

---

## ▶️ Example Usage

```python
topic = "Artificial Intelligence in Healthcare"

blog = blog_generate_using_bedrock(topic)

print(blog)
```

Example Output:

```
Artificial Intelligence is transforming healthcare by enabling faster diagnosis, 
personalized treatment plans, and improved patient outcomes...
```

---

## ⚙️ Model Parameters

| Parameter   | Description                         |
| ----------- | ----------------------------------- |
| prompt      | Input instruction sent to the model |
| max_gen_len | Maximum tokens generated            |
| temperature | Controls creativity of response     |
| top_p       | Controls probability sampling       |

---

## 🧱 AWS Services Used

* **AWS Bedrock** – Access foundation models
* **Meta Llama2** – Text generation model
* **Boto3** – AWS SDK for Python

---

## 📚 References

* AWS Bedrock Documentation
  https://docs.aws.amazon.com/bedrock/

* Boto3 Documentation
  https://boto3.amazonaws.com/

---

## 👨‍💻 Author

Developed as part of learning **AWS Bedrock and Generative AI integration using Python**.
