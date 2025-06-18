# 🤖 Transformers: Ne Yapabilirler?

Bu bölümde, Transformer modellerinin neler yapabileceğini ve 🤗 Transformers kütüphanesindeki `pipeline()` fonksiyonunu nasıl kullanabileceğimizi öğreneceğiz.
---

## 🚀 Transformer’lar Nerelerde Kullanılır?

Transformer modelleri metin, görüntü, ses gibi farklı veri türlerinde çok çeşitli görevlerde kullanılabilir:

- Doğal Dil İşleme (NLP)
- Görüntü Sınıflandırma
- Konuşma Tanıma
- Çeviri
- Özetleme
- Multimodal görevler (örneğin: görsel+metin)

🤗 Hugging Face Model Hub, bu görevler için milyonlarca önceden eğitilmiş modeli barındırır.

---

## 🛠️ pipeline() Fonksiyonu ile Kullanım Örnekleri

### Duygu Analizi
```python
from transformers import pipeline

classifier = pipeline("sentiment-analysis")
classifier("I've been waiting for a HuggingFace course my whole life.")


### Zero-shot Sınıflandırma
```python
from transformers import pipeline

classifier = pipeline("zero-shot-classification")
classifier(
    "This is a course about the Transformers library",
    candidate_labels=["education", "politics", "business"]
)
```

### Metin Üretimi
```python

from transformers import pipeline

generator = pipeline("text-generation")
generator("In this course, we will teach you how to")
```

### Belirli Bir Modelle Metin Üretimi
```python

from transformers import pipeline

generator = pipeline("text-generation", model="HuggingFaceTB/SmolLM2-360M")
generator(
    "In this course, we will teach you how to",
    max_length=30,
    num_return_sequences=2
)
```
### Maskeli Kelime Tamamlama

```python
from transformers import pipeline

unmasker = pipeline("fill-mask")
unmasker("This course will teach you all about <mask> models.", top_k=2)
```

### Varlık Tanıma (NER)
```python
from transformers import pipeline

ner = pipeline("ner", grouped_entities=True)
ner("My name is Sylvain and I work at Hugging Face in Brooklyn.")
```

### Soru-Cevaplama

```python
from transformers import pipeline

question_answerer = pipeline("question-answering")
question_answerer(
    question="Where do I work?",
    context="My name is Sylvain and I work at Hugging Face in Brooklyn."
)
```
### Metin Özeti
```python
from transformers import pipeline

summarizer = pipeline("summarization")
summarizer("Uzun bir metin buraya yazılır...")
```

### Çeviri

```python
from transformers import pipeline

translator = pipeline("translation", model="Helsinki-NLP/opus-mt-fr-en")
translator("Ce cours est produit par Hugging Face.")
```

### Görüntü Sınıflandırma

```python
from transformers import pipeline

image_classifier = pipeline(
    task="image-classification",
    model="google/vit-base-patch16-224"
)
image_classifier("https://.../pipeline-cat-chonk.jpeg")

```

### Konuşmayı Yazıya Çevirme
```python

from transformers import pipeline

transcriber = pipeline(
    task="automatic-speech-recognition",
    model="openai/whisper-large-v3"
)
transcriber("https://.../mlk.flac")
```
### 🔗 Çoklu Veri Kullanımı
Transformer modelleri birden fazla kaynaktan (metin, görsel, ses) gelen verileri bir araya getirerek:

- Bilgi arama

- Özetleme

- Akıllı yanıt üretme gibi işlemleri gerçekleştirebilir.

### ✅ Sonuç
pipeline() fonksiyonu sayesinde pek çok farklı görevi sadece birkaç satır Python koduyla gerçekleştirebilirsin.
Model Hub’daki farklı modelleri keşfetmek, özelleştirmek ve denemek oldukça kolaydır.

