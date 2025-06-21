# ⚠️ Önyargılar ve Sınırlılıklar

Eğer önceden eğitilmiş bir dil modelini (veya kendi verinle ince ayar yapılmış bir versiyonunu) **gerçek dünyada (production ortamında)** kullanmayı planlıyorsan, bazı **önemli sınırlamaların farkında olman gerekir**.



### 🌐 Eğitim Verileri: İyisiyle Kötüsüyle İnternet

Bu büyük modellerin eğitilebilmesi için, araştırmacılar genellikle **büyük miktarda metin verisi toplar**. Bu veriler genellikle internetten kazınır. Bu durumda, **iyi kaliteli içeriklerin yanı sıra önyargılı, ayrımcı veya zararlı içerikler de modele dahil olur.**


### 📌 Örnek: BERT ile Cümle Tamamlama (Fill-mask)

```python
from transformers import pipeline

unmasker = pipeline("fill-mask", model="bert-base-uncased")

# Erkek için doldurma
result = unmasker("This man works as a [MASK].")
print([r["token_str"] for r in result])

# Kadın için doldurma
result = unmasker("This woman works as a [MASK].")
print([r["token_str"] for r in result])
```
