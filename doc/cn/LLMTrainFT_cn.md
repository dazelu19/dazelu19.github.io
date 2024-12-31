## 大模型训练和微调探究

### 1 大模型训练的难点



### x 模型权重类型及其转换
**1 权重格式介绍**
- safetensor:
- bin
- kpet

**2 格式转换**

### x 模型使用
1 如何将transformer模型部署在服务器然后本地调用
使用flask搭建服务，此处以bert为例。

服务器代码(`server.py`)：
```python
from flask import Flask, request, jsonify
from transformers import AutoTokenizer, AutoModelForSequenceClassification

app = Flask(__name__)

# 加载模型和分词器
MODEL_NAME = "bert-base-uncased"  # 替换为你的模型名称
tokenizer = AutoTokenizer.from_pretrained(MODEL_NAME)
model = AutoModelForSequenceClassification.from_pretrained(MODEL_NAME)

@app.route("/predict", methods=["POST"])
def predict():
    data = request.json
    text = data.get("text", "")
    if not text:
        return jsonify({"error": "No text provided"}), 400

    # 模型推理
    inputs = tokenizer(text, return_tensors="pt", truncation=True, padding=True)
    outputs = model(**inputs)
    predictions = outputs.logits.argmax(dim=-1).item()

    return jsonify({"prediction": predictions})

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)  # 监听所有接口

```

客户端代码：
```python
import requests

# 替换为服务器的 IP 地址
SERVER_URL = "http://localhost:5000/predict"

# 输入文本
text = "This is a test sentence."

# 发送请求
response = requests.post(SERVER_URL, json={"text": text})

# 检查响应
if response.status_code == 200:
    print("Prediction:", response.json())
else:
    print("Error:", response.text)

```


