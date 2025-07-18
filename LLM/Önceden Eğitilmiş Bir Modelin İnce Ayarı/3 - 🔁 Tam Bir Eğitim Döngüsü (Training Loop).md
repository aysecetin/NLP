
# 🔁 Tam Bir Eğitim Döngüsü (Training Loop)

Bu bölümde `Trainer` sınıfı olmadan, PyTorch ile sıfırdan bir eğitim döngüsü kurarak aynı sonuca ulaşmayı hedefliyoruz.



## 📦 Veri ve Tokenizer Hazırlığı

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

---

## 🧹 Ön İşleme

```python
tokenized_datasets = tokenized_datasets.remove_columns(["sentence1", "sentence2", "idx"])
tokenized_datasets = tokenized_datasets.rename_column("label", "labels")
tokenized_datasets.set_format("torch")
```

---

## 🧱 DataLoader Tanımı

```python
from torch.utils.data import DataLoader

train_dataloader = DataLoader(
    tokenized_datasets["train"], shuffle=True, batch_size=8, collate_fn=data_collator
)
eval_dataloader = DataLoader(
    tokenized_datasets["validation"], batch_size=8, collate_fn=data_collator
)
```

---

## 🧠 Model Kurulumu

```python
from transformers import AutoModelForSequenceClassification

model = AutoModelForSequenceClassification.from_pretrained(checkpoint, num_labels=2)
```

---

## 🛠️ Optimizasyon

```python
from torch.optim import AdamW
optimizer = AdamW(model.parameters(), lr=5e-5)
```

---

## 📉 Learning Rate Scheduler

```python
from transformers import get_scheduler

num_epochs = 3
num_training_steps = num_epochs * len(train_dataloader)
lr_scheduler = get_scheduler(
    "linear",
    optimizer=optimizer,
    num_warmup_steps=0,
    num_training_steps=num_training_steps,
)
```

---

## ⚡ Cihaz Kullanımı

```python
import torch

device = torch.device("cuda") if torch.cuda.is_available() else torch.device("cpu")
model.to(device)
```

---

## 🔁 Eğitim Döngüsü

```python
from tqdm.auto import tqdm

progress_bar = tqdm(range(num_training_steps))

model.train()
for epoch in range(num_epochs):
    for batch in train_dataloader:
        batch = {k: v.to(device) for k, v in batch.items()}
        outputs = model(**batch)
        loss = outputs.loss
        loss.backward()

        optimizer.step()
        lr_scheduler.step()
        optimizer.zero_grad()
        progress_bar.update(1)
```

---

## 🧪 Değerlendirme

```python
import evaluate

metric = evaluate.load("glue", "mrpc")
model.eval()
for batch in eval_dataloader:
    batch = {k: v.to(device) for k, v in batch.items()}
    with torch.no_grad():
        outputs = model(**batch)

    logits = outputs.logits
    predictions = torch.argmax(logits, dim=-1)
    metric.add_batch(predictions=predictions, references=batch["labels"])

metric.compute()
```

---

## 🚀 Accelerate ile Dağıtık Eğitim

```python
from accelerate import Accelerator
from torch.optim import AdamW
from transformers import AutoModelForSequenceClassification, get_scheduler

accelerator = Accelerator()
model = AutoModelForSequenceClassification.from_pretrained(checkpoint, num_labels=2)
optimizer = AdamW(model.parameters(), lr=3e-5)

train_dl, eval_dl, model, optimizer = accelerator.prepare(
    train_dataloader, eval_dataloader, model, optimizer
)

num_epochs = 3
num_training_steps = num_epochs * len(train_dl)
lr_scheduler = get_scheduler(
    "linear", optimizer=optimizer, num_warmup_steps=0, num_training_steps=num_training_steps
)

progress_bar = tqdm(range(num_training_steps))
model.train()
for epoch in range(num_epochs):
    for batch in train_dl:
        outputs = model(**batch)
        loss = outputs.loss
        accelerator.backward(loss)

        optimizer.step()
        lr_scheduler.step()
        optimizer.zero_grad()
        progress_bar.update(1)
```

---

## 🧠 Quiz Bilgileri

- **AdamW**: Adam ile aynı, ama ağırlık çürümesi (weight decay) için daha uygun.
- **Eğitim akışı**: forward → backward → optimizer.step → scheduler.step → zero_grad
- **Accelerate**: Dağıtık eğitimi kolaylaştırır.
- **model.eval()**: Dropout gibi katmanların davranışını değiştirir.
- **torch.no_grad()**: Bellekten tasarruf sağlar, gradyan hesaplamayı kapatır.
- **accelerator.prepare()**: Model, optimizer ve dataloader’ı uygun yapıya sokar.
