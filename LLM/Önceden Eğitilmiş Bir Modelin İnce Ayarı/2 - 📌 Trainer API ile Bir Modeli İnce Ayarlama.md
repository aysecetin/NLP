

# 📌 Modeli Trainer API ile İnce Ayar (Fine-Tuning)

🤗 Transformers, önceden eğitilmiş modelleri modern yöntemlerle kolayca ince ayar yapabilmenizi sağlayan `Trainer` adlı bir sınıf sunar.

## 🔧 Gerekli Hazırlıklar

```python
from datasets import load_dataset
from transformers import AutoTokenizer, DataCollatorWithPadding

raw_datasets = load_dataset("glue", "mrpc")
checkpoint = "bert-base-uncased"
tokenizer = AutoTokenizer.from_pretrained(checkpoint)

def tokenize_function(example):
    return tokenizer(example["sentence1"], example["sentence2"], truncation=True)

tokenized_datasets = raw_datasets.map(tokenize_function, batched=True)
data_collator = DataCollatorWithPadding(tokenizer=tokenizer)
```

## ⚙️ Eğitim Ayarları

```python
from transformers import TrainingArguments

training_args = TrainingArguments("test-trainer")
```

GPU kullanmıyorsan eğitim çok yavaş olabilir. Google Colab veya benzeri platformlarda ücretsiz GPU'larla çalışabilirsin.

## 🧠 Model Tanımı

```python
from transformers import AutoModelForSequenceClassification

model = AutoModelForSequenceClassification.from_pretrained(checkpoint, num_labels=2)
```

Modelin baş kısmı görevimize uygun hale getirilir; bazı ağırlıklar rastgele başlatılır.

## 🏋️ Trainer Sınıfını Oluşturma

```python
from transformers import Trainer

trainer = Trainer(
    model,
    training_args,
    train_dataset=tokenized_datasets["train"],
    eval_dataset=tokenized_datasets["validation"],
    data_collator=data_collator,
    processing_class=tokenizer,
)
```

`processing_class` ile hangi tokenizer'ın kullanılacağı belirtilir.

## 🚀 Eğitimi Başlatmak

```python
trainer.train()
```

Eğitim sırasında sadece kayıp (loss) değeri raporlanır. Doğruluk (accuracy) gibi metrikler için ayrıca `compute_metrics()` tanımlamak gerekir.

## 📊 Değerlendirme Metrikleri

```python
import evaluate
import numpy as np

metric = evaluate.load("glue", "mrpc")

def compute_metrics(eval_preds):
    logits, labels = eval_preds
    predictions = np.argmax(logits, axis=-1)
    return metric.compute(predictions=predictions, references=labels)
```

Trainer’a yeniden bu fonksiyonu tanıt:

```python
training_args = TrainingArguments("test-trainer", eval_strategy="epoch")

trainer = Trainer(
    model,
    training_args,
    train_dataset=tokenized_datasets["train"],
    eval_dataset=tokenized_datasets["validation"],
    data_collator=data_collator,
    processing_class=tokenizer,
    compute_metrics=compute_metrics,
)
trainer.train()
```

## ⚡️ Gelişmiş Özellikler

- **Mixed Precision (fp16):** Bellek kullanımını azaltır, eğitimi hızlandırır.
- **Gradient Accumulation:** Küçük batch’lerle büyük batch etkisi sağlar.
- **Learning Rate Scheduling:** Öğrenme oranı zamanla azaltılabilir (`lr_scheduler_type`).



**Soru-Cevap:**

1. `processing_class` ne işe yarar?  
   → Tokenizer’ı belirtmek için kullanılır.

2. `eval_strategy` neyi kontrol eder?  
   → Değerlendirmenin sıklığını belirler (adım adım veya epoch sonunda).

3. `fp16=True` ne sağlar?  
   → Mixed precision ile daha az bellek, daha hızlı eğitim.

4. `compute_metrics()` fonksiyonu ne işe yarar?  
   → Logit'leri tahmine dönüştürür, doğruluk ve F1 gibi metrikleri hesaplar.

5. `eval_dataset` verilmezse ne olur?  
   → Eğitim yapılır ama değerlendirme metrikleri alınmaz.

6. Gradient accumulation nedir?  
   → Birkaç batch'in gradyanlarını biriktirip sonra ağırlıkları günceller.


Trainer API, tüm bu süreci yüksek seviyede kontrol etmeni sağlayarak hızlı ve güvenilir model ince ayarı sağlar.

## 💡 Önemli Noktalar:

- API Trainer, çoğu eğitim karmaşıklığını ele alan üst düzey bir arayüz sağlar
- processing_classUygun veri işleme için belirteçleyicinizi belirtmek için kullanın
- TrainingArgumentseğitimin tüm yönlerini kontrol eder: öğrenme hızı, parti büyüklüğü, değerlendirme stratejisi ve optimizasyonlar
-  compute_metricsyalnızca eğitim kaybının ötesinde özel değerlendirme ölçümlerini mümkün kılar
- Karma hassasiyet ( ) ve gradyan birikimi gibi modern özellikler, fp16=Trueeğitim verimliliğini önemli ölçüde artırabilir
