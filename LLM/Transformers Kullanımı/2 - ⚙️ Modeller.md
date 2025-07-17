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

### 3. Modeli Hugging Face Hub’a Yükleme
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


### ✍️ **Metni Sayılara Çevirme (`Tokenizer`)**

```python
from transformers import AutoTokenizer

tokenizer = AutoTokenizer.from_pretrained("bert-base-cased")

encoded = tokenizer("Merhaba, bu bir cümledir!")
print(encoded)
```

Bu işlem 3 şey döner:

- `input_ids`: Kelimelerin sayısal karşılıkları  
- `token_type_ids`: Hangi kelime hangi cümleye ait (çift cümlelerde işe yarar)  
- `attention_mask`: Hangi token’lara dikkat edilsin (örneğin padding olanlara edilmez)  

Geri çevirmek için:

```python
tokenizer.decode(encoded["input_ids"])
```

Örnek çıktı:  
`[CLS] Merhaba, bu bir cümledir! [SEP]`

---

### 🧱 **Çoklu Cümleleri İşlemek**

```python
encoded = tokenizer(
    ["Nasılsın?", "İyiyim, teşekkürler!"],
    return_tensors="pt",
    padding=True
)
```

- Farklı uzunluktaki cümleleri eşit boyuta getirmek için `padding=True` kullanılır.  
- `return_tensors="pt"` ile PyTorch tensörleri döner.  
- `truncation=True` ile çok uzun cümleler kesilir.  

---

### 🔠 **Özel Tokenlar (Special Tokens)**

- BERT gibi modeller `[CLS]` ve `[SEP]` gibi özel token'lara ihtiyaç duyar.  
- Tokenizer bu token’ları otomatik olarak ekler.  

---

### 🧪 **Modeli Çalıştırmak**

```python
import torch

inputs = tokenizer("Bir örnek cümle", return_tensors="pt")
outputs = model(**inputs)
```

- Tokenizer çıktısı doğrudan modelin içine verilebilir.  

---

### 💡 Neden Bu Kadar Uğraşıyoruz?

Çünkü:

- Transformer modelleri metinle değil sayılarla çalışır.  
- Sayılar modelin anlayabileceği şekilde hazırlanmalı (tokenize edilmeli, pad/truncate edilmeli, mask’lenmeli).  
- Her modelin beklentisi farklı olabilir (örneğin özel tokenlar).  

