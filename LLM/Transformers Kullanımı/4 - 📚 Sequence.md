
# 📚 Birden Fazla Dizi (Sequence) ile Çalışmak

Önceki bölümde yalnızca **tek bir kısa cümle** ile nasıl çıkarım (inference) yapılacağını gördük. Şimdi şu sorular ortaya çıkıyor:

- Birden fazla cümleyi nasıl işleriz?
- Uzunlukları farklı olan cümlelerle ne yaparız?
- Sadece `input_ids` yeterli mi?
- Uzunluk çok fazla olursa ne olur?



### 🧾 Modeller Toplu (Batch) Giriş Bekler

Aşağıdaki gibi bir cümleyi doğrudan modele verirsek hata alırız:

```python
import torch
from transformers import AutoTokenizer, AutoModelForSequenceClassification

checkpoint = "distilbert-base-uncased-finetuned-sst-2-english"
tokenizer = AutoTokenizer.from_pretrained(checkpoint)
model = AutoModelForSequenceClassification.from_pretrained(checkpoint)

sequence = "I've been waiting for a HuggingFace course my whole life."
tokens = tokenizer.tokenize(sequence)
ids = tokenizer.convert_tokens_to_ids(tokens)
input_ids = torch.tensor(ids)

# HATA: Tensor boyutu eksik
model(input_ids)
```

### ✅ Doğru kullanım:

```python
input_ids = torch.tensor([ids])  # Yeni bir boyut ekleniyor
output = model(input_ids)
print(output.logits)
```

Bu çıktılar "logits"tir — modelin tahmin skorlarıdır.



### 📦 Toplu İşlem (Batching)

Aynı cümleyi iki kez gönderelim:
```python
batched_ids = [ids, ids]
```

Bu artık 2 cümlelik bir batch!

> ✏️ Uygulama: Bu listeyi tensöre çevirip modelden çıktıyı al. Logit’lerin aynı olduğunu göreceksin.



### 🔲 Pad Etme (Padding)

Farklı uzunluktaki diziler doğrudan tensöre çevrilemez. Örneğin:

```python
batched_ids = [
    [200, 200, 200],
    [200, 200]
]
```

Bunu düzeltmek için `padding` yapılır:

```python
padding_id = tokenizer.pad_token_id

batched_ids = [
    [200, 200, 200],
    [200, 200, padding_id]
]
```

Ama bu haliyle model çıktıları **bozulur**, çünkü Transformer’lar tüm token’lara dikkat eder (pad’ler dahil!).



### 🧠 Dikkat Maskesi (Attention Mask)

Pad edilen token’ları **yok saymak** için attention maskesi kullanılır:

```python
attention_mask = [
    [1, 1, 1],
    [1, 1, 0]
]

outputs = model(
    torch.tensor(batched_ids),
    attention_mask=torch.tensor(attention_mask)
)
print(outputs.logits)
```

Bu sayede pad’li sonuç ile tekli sonuç **aynı çıkar**.

### ✏️ Uygulama Önerisi

Şu iki cümleyi elle tokenize edip modele ver:

- "I've been waiting for a HuggingFace course my whole life."
- "I hate this so much!"

Önce tek tek ver, sonra pad’leyip batch halinde ver. Aynı sonuçları aldığını gör.



### 📏 Uzun Sekanslar Ne Olur?

Çoğu Transformer modeli **maksimum 512 veya 1024 token**'ı destekler. Daha uzun diziler hata verir.

**2 çözüm:**

1. **Uzun dizilerle başa çıkan modeller** kullan (örneğin: Longformer, LED)
2. **Kısaltma (Truncation)** uygula:

```python
sequence = sequence[:max_sequence_length]
```



Transformer modellerinde batching, padding, ve attention maskeleri doğru kullanmak performans ve doğruluk için kritik öneme sahiptir.
