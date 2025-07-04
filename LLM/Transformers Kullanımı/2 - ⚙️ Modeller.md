# 🚀 Transformer Modeli Oluşturma ve Kullanma (Hugging Face Transformers)

### 1. AutoModel ile Model Yükleme


```python
from transformers import AutoModel
model = AutoModel.from_pretrained("bert-base-cased")
```

- from_pretrained() metodu modeli Hugging Face Hub üzerinden indirir ve önbelleğe alır.

- bert-base-cased: 12 katmanlı, 768 boyutlu gizli vektör, 12 attention head içeren, büyük-küçük harf ayrımı yapan bir BERT modelidir.

- AutoModel: Hangi model türü olduğunu otomatik belirler ve uygun sınıfı yükler.

- Alternatif olarak, doğrudan modeli de çağırabilirsin:

```python
from transformers import BertModel
model = BertModel.from_pretrained("bert-base-cased")
```
