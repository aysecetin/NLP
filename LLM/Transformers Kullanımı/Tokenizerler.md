
# 🔤 Tokenizer Nedir ve Nasıl Çalışır?

Transformer modelleri yalnızca **sayısal veriyle** çalışabilir. Tokenizer’lar, metni bu sayısal forma çeviren araçlardır.

---

## 📌 Temel Amaç

Ham metin:
```
Jim Henson was a puppeteer
```

Model bunu anlayamaz. Tokenizer bu metni anlamlı ve minimum bilgi kaybıyla **sayılara çevirir**.

---

## 🧱 Tokenizer Türleri

### 1. **Kelime Tabanlı (Word-based) Tokenizer**
```python
"Jim Henson was a puppeteer".split()
# ['Jim', 'Henson', 'was', 'a', 'puppeteer']
```

- Her kelimeye bir ID atanır.
- "dog" ≠ "dogs", "run" ≠ "running"
- Çok büyük vocabulary gerekir.
- Bilinmeyen kelimeler için `[UNK]` token’ı kullanılır.

---

### 2. **Karakter Tabanlı (Character-based) Tokenizer**

```text
"puppeteer" → ['p', 'u', 'p', 'p', 'e', 't', 'e', 'e', 'r']
```

- Küçük vocabulary
- `[UNK]` yok denecek kadar az
- Ancak uzunluk artar ve her karakter anlamsızdır

---

### 3. **Alt-kelime Tabanlı (Subword-based) Tokenizer**

Örnek:
- `"annoyingly"` → `"annoying"` + `"ly"`
- `"tokenization"` → `"token"` + `"ization"`

Avantajları:
- Anlamlı alt token’lar
- Küçük vocabulary ile geniş kapsama
- Türkçe gibi eklemeli dillerde idealdir

---

### 4. **Popüler Yöntemler**

- **BPE (Byte Pair Encoding)** – GPT-2
- **WordPiece** – BERT
- **SentencePiece / Unigram** – Multilingual modeller

---

## 💾 Tokenizer Yükleme ve Kaydetme

```python
from transformers import AutoTokenizer

tokenizer = AutoTokenizer.from_pretrained("bert-base-cased")
tokenizer.save_pretrained("klasor_adi")
```

---

## 🔁 Encoding (Metni Sayıya Çevirme)

### 1. Tokenization
```python
tokens = tokenizer.tokenize("Using a Transformer network is simple")
# ['Using', 'a', 'transform', '##er', 'network', 'is', 'simple']
```

### 2. Token ID’lerine Dönüştürme
```python
ids = tokenizer.convert_tokens_to_ids(tokens)
# [7993, 170, 11303, 1200, 2443, 1110, 3014]
```

---

## 🔁 Decoding (Sayılardan Metne Geri Dönüş)

```python
decoded = tokenizer.decode([7993, 170, 11303, 1200, 2443, 1110, 3014])
# 'Using a Transformer network is simple'
```

---

## 🧪 Uygulama Önerisi

Şu cümleleri tokenize et ve ID’lere çevir:
- "I've been waiting for a HuggingFace course my whole life."
- "I hate this so much!"

Sonuçları önceki örneklerle karşılaştır.

---

## 🔚 Özet

Tokenizer’lar:
- Metni token’lara böler,
- Bunları sayılara çevirir,
- Gerekirse geri çevirir,
- Modelin anlayabileceği formatı oluşturur.
