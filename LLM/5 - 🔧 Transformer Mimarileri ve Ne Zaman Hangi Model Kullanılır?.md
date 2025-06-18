# 🔧 Transformer Mimarileri ve Ne Zaman Hangi Model Kullanılır?

Bu bölümde, üç temel Transformer mimarisini ve bunların hangi görevler için uygun olduğunu inceleyeceğiz.

---

## 🧱 Üç Temel Mimari

### 1. Encoder-Only Modeller
- Sadece encoder kısmı kullanılır.
- Tüm cümleyi aynı anda görebilir (bidirectional attention).
- **Kullanım Alanları:**
  - Metin sınıflandırma
  - Named Entity Recognition (NER)
  - Extractive soru-cevap
- **Örnekler:** BERT, DistilBERT

---

### 2. Decoder-Only Modeller
- Sadece decoder kısmı kullanılır.
- Sadece önceki kelimelere bakar (autoregressive).
- **Kullanım Alanları:**
  - Metin üretimi
  - Kod yazma
  - Chatbot uygulamaları
- **Örnekler:** GPT, LLaMA, SmolLM, Gemma

---

### 3. Encoder-Decoder (Seq2Seq) Modeller
- Hem encoder hem decoder birlikte çalışır.
- Encoder anlam çıkarır, decoder çıktı üretir.
- **Kullanım Alanları:**
  - Çeviri
  - Özetleme
  - Generative soru-cevap
- **Örnekler:** T5, BART, Marian, mBART

---

## 📊 Göreve Göre Mimari Seçimi

| Görev                         | Mimari            | Örnek Modeller     |
|------------------------------|-------------------|--------------------|
| Metin sınıflandırma          | Encoder           | BERT, RoBERTa      |
| Metin üretimi                | Decoder           | GPT, LLaMA         |
| Çeviri                       | Encoder-Decoder   | T5, BART           |
| Özetleme                     | Encoder-Decoder   | T5, BART           |
| Named Entity Recognition     | Encoder           | BERT, DistilBERT   |
| Soru-cevap (extractive)      | Encoder           | BERT               |
| Soru-cevap (generative)      | Encoder-Decoder   | T5, BART           |
| Sohbet (chatbot)             | Decoder           | GPT, LLaMA         |

---

## 🌟 Modern LLM’ler: Özellikler ve Aşamalar

- **Pretraining:** Büyük metinlerle sonraki token'ı tahmin eder.
- **Instruction Tuning:** İnsan gibi yanıt verecek şekilde ince ayar yapılır.

**Yetenekleri:**
- Metin üretimi
- Özetleme
- Çeviri
- Kod üretimi
- Soru yanıtlama
- Akıl yürütme
- Few-shot öğrenme

---

## ⚙️ Verimli Attention Yöntemleri

### 🔹 LSH Attention (Reformer)
- Sadece ilgili kelimelere dikkat eder.
- Hashing ile benzer kelimeler seçilir.

### 🔹 Local Attention (Longformer)
- Sadece komşu kelimeler dikkate alınır.
- Bazı özel token'lara global dikkat verilir.

### 🔹 Axial Positional Encoding
- Pozisyon bilgisi büyük matris yerine iki küçük matrisle hesaplanır.
- Bellek kullanımını azaltır.

---

## ✅ Özet

- Görev türüne göre doğru mimari seçimi başarı için kritik:
  - Anlama görevleri → **Encoder**
  - Üretim görevleri → **Decoder**
  - Girdi-çıktı dönüşümleri → **Encoder-Decoder**
- Modern LLM’ler çoğunlukla decoder mimarisiyle çalışır.
- Uzun metinlerde özel attention stratejileri performansı artırır.

