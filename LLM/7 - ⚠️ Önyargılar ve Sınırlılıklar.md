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
### 📋 Çıktılar:

- **"This man works as a [MASK]."** → `['lawyer', 'carpenter', 'doctor', 'waiter', 'mechanic']`
- **"This woman works as a [MASK]."** → `['nurse', 'waitress', 'teacher', 'maid', 'prostitute']`

Bu örnekte model, kadınlar için hem **daha az çeşitlilik gösteren hem de toplumsal cinsiyet kalıplarını yansıtan** işler öneriyor.  
Üstelik `"prostitute"` (fahişe) gibi hassas bir meslek de öneriliyor.  
Erkek için verilen meslekler genellikle daha “prestijli” kabul edilen roller.

İronik olarak, bu örnekte kullanılan **BERT modeli**, pek çok modern LLM’in aksine rastgele internet verileriyle değil;  
**Wikipedia ve BookCorpus** gibi daha “nötr” veri kaynaklarıyla eğitilmiştir.  
**Buna rağmen önyargılar hâlâ mevcut.**

---

## 🧠 Gerçek: Modeller Önyargı Taşır

- LLM’ler sadece **istatistiksel örüntüleri öğrenir**.
- Eğitim verilerindeki **cinsiyetçilik, ırkçılık, homofobi** gibi önyargılar, modelin çıktısına da yansır.
- Modeli kendi verinle **fine-tune** etmek, bu temel önyargıları **tamamen ortadan kaldırmaz**.

---

## 🚨 Ne Yapmalı?

- **Modellerin tarafsız olmadığını** her zaman akılda tut.
- Hassas uygulamalarda model çıktılarında:
  - **Filtreleme**
  - **İzleme (monitoring)** stratejileri kullanılmalı.
- Gerçek kullanıcıya yönelik sistemlerde, **önyargı farkındalığı olan tasarımlar** geliştirilmeli.

> 💡 *Modeller güçlü olabilir ama etik sorumlulukla kullanıldıklarında güvenilir hale gelirler.*
