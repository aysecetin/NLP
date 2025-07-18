
# 🔗 Her Şeyi Bir Araya Getirmek

Her adımı manuel yapmaya çalıştık:
- Tokenizer nasıl çalışır?
- Tokenization, input ID'ye çevirme
- Padding, truncation, attention mask

Ama aslında 🤗 Transformers API tüm bu işlemleri bizim yerimize **tek adımda** yapabiliyor.

## ⚙️ Basit Kullanım

```python
from transformers import AutoTokenizer

checkpoint = "distilbert-base-uncased-finetuned-sst-2-english"
tokenizer = AutoTokenizer.from_pretrained(checkpoint)

sequence = "I've been waiting for a HuggingFace course my whole life."
model_inputs = tokenizer(sequence)
```

Burada `model_inputs`, modele verilecek tüm bilgileri içerir:
- `input_ids`
- `attention_mask`
- (Bazı modellerde başka girişler de olabilir)



## 📥 Birden Fazla Cümle ile Kullanım

```python
sequences = [
    "I've been waiting for a HuggingFace course my whole life.",
    "So have I!"
]

model_inputs = tokenizer(sequences)
```



## 📏 Otomatik Padding

### En uzun cümleye göre:
```python
tokenizer(sequences, padding="longest")
```

### Modele göre:
```python
tokenizer(sequences, padding="max_length")
```

### Belirli uzunlukta:
```python
tokenizer(sequences, padding="max_length", max_length=8)
```



## ✂️ Otomatik Truncation

### Modele göre kısalt:
```python
tokenizer(sequences, truncation=True)
```

### Belirli uzunlukta kısalt:
```python
tokenizer(sequences, max_length=8, truncation=True)
```



## 🔄 Tensor Formatı Seçmek

```python
# PyTorch tensörleri
tokenizer(sequences, padding=True, return_tensors="pt")

# NumPy dizileri
tokenizer(sequences, padding=True, return_tensors="np")
```



## 🔠 Özel Token'lar

Tokenizer bazen ek token ID'leri ekler:

```python
tokens = tokenizer.tokenize(sequence)
ids = tokenizer.convert_tokens_to_ids(tokens)

print(ids)
# [1045, 1005, 2310, ..., 2166, 1012]

model_inputs = tokenizer(sequence)
print(model_inputs["input_ids"])
# [101, 1045, 1005, ..., 2166, 1012, 102]
```

### Decode farkı:

```python
tokenizer.decode(model_inputs["input_ids"])
# "[CLS] i've been waiting ... my whole life. [SEP]"

tokenizer.decode(ids)
# "i've been waiting ... my whole life."
```

`[CLS]` ve `[SEP]` gibi token'lar modelin eğitildiği formatla uyumlu çıktılar üretmek için gereklidir. Tokenizer bunu senin yerinde otomatik yapar.



## ✅ Baştan Sona: Tokenizer'dan Modele

```python
import torch
from transformers import AutoTokenizer, AutoModelForSequenceClassification

checkpoint = "distilbert-base-uncased-finetuned-sst-2-english"
tokenizer = AutoTokenizer.from_pretrained(checkpoint)
model = AutoModelForSequenceClassification.from_pretrained(checkpoint)

sequences = [
    "I've been waiting for a HuggingFace course my whole life.",
    "So have I!"
]

tokens = tokenizer(sequences, padding=True, truncation=True, return_tensors="pt")
output = model(**tokens)
```

Bu sayede:
- Cümle sayısı fark etmez
- Uzunluk sınırına göre otomatik kısaltma olur
- Tensor formatı ayarlanabilir
- Modele doğrudan veri verilebilir



Bu yapı, Hugging Face Transformers ile NLP iş akışlarını oldukça kolaylaştırır 🚀
