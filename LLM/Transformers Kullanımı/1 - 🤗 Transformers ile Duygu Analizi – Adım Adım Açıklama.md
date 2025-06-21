# 🤗 Transformers ile Duygu Analizi – Adım Adım Açıklama

### 📌 Başlangıç

İlk bölümde kullandığımız kod şu şekildeydi:

```python
from transformers import pipeline

classifier = pipeline("sentiment-analysis")
classifier([
    "I've been waiting for a HuggingFace course my whole life.",
    "I hate this so much!",
])
