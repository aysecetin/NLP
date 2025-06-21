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
```

Bu kodun çıktısı:

```python

[{'label': 'POSITIVE', 'score': 0.9598},
 {'label': 'NEGATIVE', 'score': 0.9994}]
```

Bu basit pipelineaslında üç aşamayı kapsar:

- Preprocessing (Ön işleme)

- Model Inference (Modelden geçirme)

- Postprocessing (Son işleme)

