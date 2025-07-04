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
### 2. Modeli Kaydetme ve Yükleme
```python
model.save_pretrained("klasor_adi")
```

Bu klasöre iki dosya kaydedilir:

- config.json: Model mimarisi ve metadata bilgisi.

- pytorch_model.bin: Modelin ağırlıkları (state dict).

Tekrar yüklemek için:

```python
model = AutoModel.from_pretrained("klasor_adi")
```

3. Modeli Hugging Face Hub’a Yükleme
Giriş yap:

```python
from huggingface_hub import notebook_login
notebook_login()
```
ya da terminalden:

```bash
huggingface-cli login
```

Modeli yükle:


```python
model.push_to_hub("benim-harika-modelim")
```

Diğer kullanıcılar da artık şu şekilde modeli kullanabilir:


```python
model = AutoModel.from_pretrained("kullaniciadi/benim-harika-modelim")
```
