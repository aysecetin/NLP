# 🧠 LLM’lerde Metin Üretimi (Inference) Derinlemesine İnceleme

## 🔍 Inference Nedir?
LLM’ler eğitildikten sonra, onlardan çıktı almak için kullanılan sürece **inference (çıkarım)** denir. Model, verilen bir input prompt’a göre **birer birer token üreterek** yanıt oluşturur.

---

## 🧲 Dikkat Mekanizması (Attention)
LLM’lerin bağlamı anlamasını ve tutarlı metin üretmesini sağlayan temel yapı taşıdır. Önemli kelimelere odaklanarak doğru tahmin yapılmasını sağlar.

> Örnek: “Fransa’nın başkenti …” → “Paris”  
Burada “Fransa” ve “başkenti” kelimeleri daha önemlidir.

---

## 🧮 Bağlam Uzunluğu (Context Length)
Modelin aynı anda işleyebildiği maksimum token sayısıdır. Uzun bağlam daha anlamlı metinler üretir, ancak:

- **Bellek kullanımı**: bağlam uzunluğu arttıkça **kareli (quadratic)** artar  
- **İşlem süresi**: doğrusal (linear) artar

---

## ✍️ Etkili Girdi Hazırlama (Prompting)
Model sadece bir sonraki token’ı tahmin eder. Bu yüzden **iyi yapılandırılmış promptlar**, çıktının kalitesini doğrudan etkiler.

---

## 🧱 Inference Süreci: İki Aşama

### 1️⃣ Prefill Aşaması
- **Tokenization**: Metin → Tokenlar  
- **Embedding**: Tokenlar → Vektörler  
- **İlk İşlem**: Vektörler → Modelden geçirilerek bağlamsal temsil

> ⛽ Bu aşama çok yoğundur, çünkü tüm girdi bir defada işlenir.

### 2️⃣ Decode Aşaması
Model birer birer token üretir. Her adımda:
- Önceki tokenlara **dikkat** uygulanır  
- Olasılıklar hesaplanır  
- Bir token seçilir  
- Devam edip etmeyeceği kontrol edilir

---

## 🎛️ Token Seçim Stratejileri

### 📉 Sıcaklık (Temperature)
- Yüksek değer (ör. 1.2): Daha rastgele ve yaratıcı
- Düşük değer (ör. 0.7): Daha tutarlı ve deterministik

### ✅ Top-k / Top-p Sampling
- **Top-k**: En olası k seçenek arasından seçim  
- **Top-p**: Olasılık toplamı belirli bir eşik (ör. %90) geçene kadar olanlar arasından seçim

### 🔁 Tekrardan Kaçınma
- **Presence penalty**: Daha önce geçen kelimelere ceza  
- **Frequency penalty**: Sık kullanılanlara artan ceza

---

## 📏 Metin Uzunluğu Kontrolü
- **Token sınırları**  
- **Durdurma dizileri (stop sequences)**  
- **EOS tokenı** (ör. `<|im_end|>`)

---

## ♟️ Beam Search
Modelin her adımda birden fazla olasılığı paralel olarak değerlendirdiği tekniktir.

- 5–10 farklı yol takip edilir  
- Her yol için yeni tokenlar denenir  
- En yüksek toplam olasılığa sahip zincir seçilir

> Daha iyi tutarlılık sağlar, ancak hesaplama maliyeti yüksektir.

---

## ⚙️ Performans ve Optimizasyon

### 🔢 Temel Metrikler
- **TTFT**: İlk token’ın süresi  
- **TPOT**: Her token üretim süresi  
- **Throughput**: Aynı anda kaç istek yanıtlanabilir  
- **VRAM kullanımı**: Bellek gereksinimi

---

## 🧠 KV Cache ile Hızlandırma
**KV caching**, önceki adımlarda hesaplanan **Key-Value** değerlerini saklayarak:

- Tekrar hesaplamayı engeller  
- Decode sürecini hızlandırır  
- Uzun bağlamlarla çalışmayı mümkün kılar

> Dezavantajı: Bellek tüketimi artar.

---

## ✅ Sonuç
LLM inference sürecini etkili kullanmak için şu başlıkları iyi kavramak gerekir:

- Attention ve bağlamın rolü  
- Prefill ve Decode aşamaları  
- Sampling stratejileri ve beam search  
- Performans darboğazları ve optimizasyonlar (KV Cache)

> 📌 LLM dünyası hızla gelişiyor. Yeni teknikleri takip etmek ve farklı senaryolarda denemeler yapmak kritik önem taşır.
