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

### ✂️ 1. Ön İşleme (Tokenizer ile)
- Transformer modelleri ham metni anlayamaz. Önce bu metinleri sayılara (token’lara) dönüştürmemiz gerekir.

**Tokenizer Ne Yapar?**
- Metni kelime, alt-kelime veya sembollere böler

- Her token’a bir sayı atar (input_ids)

- Ek bilgiler (örneğin attention_mask) ekler

```python

from transformers import AutoTokenizer

checkpoint = "distilbert-base-uncased-finetuned-sst-2-english"
tokenizer = AutoTokenizer.from_pretrained(checkpoint)

raw_inputs = [
    "I've been waiting for a HuggingFace course my whole life.",
    "I hate this so much!",
]


inputs = tokenizer(raw_inputs, padding=True, truncation=True, return_tensors="pt")
```
Dönen sonuç:

```python
{
  'input_ids': tensor([[...], [...]]),
  'attention_mask': tensor([[1, 1, ..., 1], [1, 1, ..., 0]])
}
```
- input_ids: Cümledeki token’ların sayısal temsili

- attention_mask: Modelin hangi tokenlara dikkat etmesi gerektiğini belirtir

### 🧠 2. Modelden Geçirme (Inference)
Modeli yüklüyoruz:

```python

from transformers import AutoModel

model = AutoModel.from_pretrained(checkpoint)
```
Modelin çıktısı hidden states adı verilen gizli temsillerdir:

```python

outputs = model(**inputs)
print(outputs.last_hidden_state.shape)  # torch.Size([2, 16, 768])
```
- 2 cümle → 2 örnek (batch size)

- 16 token → sequence length

- 768 → her token için vektör boyutu (hidden size)

### 🎯 3. Model Head: Sınıflandırma
Duygu analizi için sınıflandırma başlığı olan bir model gereklidir:

```python

from transformers import AutoModelForSequenceClassification

model = AutoModelForSequenceClassification.from_pretrained(checkpoint)
outputs = model(**inputs)
print(outputs.logits.shape)  # torch.Size([2, 2])
```
Her cümle için 2 skor (logit) döner: NEGATIVE ve POSITIVE.

### 🔄 4. Son İşleme (Postprocessing)
Modelin çıktısı logit yani normalize edilmemiş skorlardır:

```python

print(outputs.logits)
# tensor([[-1.5607,  1.6123],
#         [ 4.1692, -3.3464]])
```
Bu skorları Softmax fonksiyonu ile olasılığa çeviriyoruz:

```python

import torch

predictions = torch.nn.functional.softmax(outputs.logits, dim=-1)
print(predictions)
# tensor([[0.0402, 0.9598],
#         [0.9995, 0.0005]])
```
Etiketleri almak için:

```python

print(model.config.id2label)
# {0: 'NEGATIVE', 1: 'POSITIVE'}
```
Sonuç:

- İlk cümle → %95.98 olasılıkla POZİTİF

- İkinci cümle → %99.95 olasılıkla NEGATİF

### ✅ Özet: NLP Pipeline’ın 3 Temel Aşaması
- Preprocessing → Tokenizer ile metni sayılara çevir

- Model Inference → Model üzerinden tahmin yap

- Postprocessing → Sonuçları olasılığa çevir ve etiketle eşleştir

