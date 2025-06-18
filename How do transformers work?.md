# 🚀 Transformer Modelleri Nedir ve Nasıl Çalışır?

## 1. Transformer Nedir?

Transformer, 2017 yılında "Attention is All You Need" adlı makaleyle tanıtılan bir yapay zeka model mimarisidir. Özellikle doğal dil işleme (NLP) alanında büyük bir etki yaratmıştır.

Diğer modellerin aksine, Transformer'lar sıralı veriyle çalışırken verileri sırayla işlemez. Bunun yerine, tüm kelimelere aynı anda bakar ve dikkat (attention) mekanizmasıyla hangi kelimelere odaklanması gerektiğini belirler. Bu sayede uzun cümlelerde bile bağlamı koruyarak daha başarılı sonuçlar üretir.



## 2. Ana Yapısı Nasıldır?

Transformer iki temel bileşenden oluşur:

- **Encoder (Kodlayıcı):** Girdi metnini işler ve anlamlı temsiller (özellikler) oluşturur.
- **Decoder (Kod Çözücü):** Encoder çıktısını kullanarak hedef metni üretir (örneğin çeviri yaparken).

Bu yapı üç farklı kullanım şeklinde karşımıza çıkar:

- **Encoder-Only:** Girişin analiz edilmesi gereken görevlerde kullanılır (örneğin duygu analizi, metin sınıflandırma).
- **Decoder-Only:** Metin üretimi gibi görevlerde kullanılır (örneğin GPT modelleri).
- **Encoder-Decoder:** Girişten bir çıktı üretildiği durumlarda kullanılır (örneğin çeviri, özetleme).


## 3. Dikkat (Attention) Mekanizması

Attention, Transformer’ın temelidir. Modelin, cümledeki hangi kelimenin diğer kelimelerle olan ilişkisine daha fazla "dikkat" etmesi gerektiğini belirler.

Örneğin:  
"You like this course" cümlesini Fransızcaya çevirirken, "like" fiilinin doğru çekimi için "you" öznesine dikkat edilmelidir. Aynı şekilde, "this" kelimesi çevrilirken "course" kelimesinin cinsiyetine bakmak gerekebilir.

Bu sayede model, bağlama duyarlı çeviriler ve analizler yapabilir. Bu mekanizma her kelime için tüm diğer kelimeleri değerlendirir ve bağlamı dikkate alarak karar verir.



## 4. Transformer Ailesi ve Örnek Modeller

Yıllar içinde farklı özellikler taşıyan birçok Transformer tabanlı model geliştirildi:

- **GPT (2018):** Metin üretiminde başarılı. Tek yönlü, sadece geçmişe bakar (causal).
- **BERT (2018):** Cümle anlamı üzerine güçlü. Maskeli kelime tahmini yapar (bidirectional).
- **GPT-2 / GPT-3 / InstructGPT (2019-2022):** Daha büyük, daha güçlü versiyonlar. Özellikle GPT-3 sıfır örnekle (zero-shot) görevleri gerçekleştirebilir.
- **T5 (2019):** Tüm görevleri bir çeviri problemine dönüştürür.
- **LLaMA, Mistral, Gemma, SmolLM (2023–2024):** Daha verimli, hafif ve bazıları çok dilli yeni nesil modellerdir.

Bu modellerin çoğu üç ana gruba ayrılır:
- **GPT-türü (oto-regresif)**
- **BERT-türü (oto-şifreleyici)**
- **T5-türü (girişten çıkışa - sequence-to-sequence)**

---

## 5. Model Eğitimi: Pretraining ve Fine-Tuning

Transformer modelleri iki aşamada eğitilir:

### 🔧 Pretraining (Ön Eğitim):
- Büyük miktarda etiketsiz metinle model sıfırdan eğitilir.
- Model dilin istatistiksel yapısını öğrenir.
- Bu süreç çok zaman ve kaynak gerektirir (örneğin haftalar sürebilir).

### 🔧 Fine-Tuning (İnce Ayar):
- Önceden eğitilmiş model, daha küçük ve etiketli bir veriyle özel bir göreve göre ayarlanır.
- Daha az veri, daha az zaman ve daha az kaynakla iyi sonuçlar elde edilir.
- Bu nedenle her zaman hazır bir modelle başlamak, sıfırdan eğitimden çok daha avantajlıdır.

Örneğin: İngilizce eğitilmiş bir modeli, bilimsel makaleleri anlaması için arXiv veri kümesiyle fine-tune edebiliriz.

---

## 6. Transformer’lar Büyük ve Güçlüdür

Transformer modellerinin başarısı, genellikle boyutlarının büyüklüğüyle paraleldir.  
GPT-3 gibi modellerin yüz milyarlarca parametresi vardır. Bu da eğitimlerini:

- **Zaman açısından pahalı**,  
- **Donanım açısından zor**,  
- **Çevresel olarak maliyetli** hâle getirir.

Bu yüzden modellerin ağırlıklarının paylaşılması çok önemlidir. Böylece bir modeli sıfırdan eğitmek yerine, hazır bir modelin üzerine inşa etmek hem çevre dostu hem de verimlidir.

Ayrıca araçlar sayesinde eğitimin karbon ayak izi hesaplanabilir:  
- [`ML CO2 Impact`](https://mlco2.github.io/impact)  
- [`CodeCarbon`](https://codecarbon.io) (🤗 Transformers ile entegre çalışır)

---

## 7. Terimler

Bazı temel terimler:

- **Architecture (Mimari):** Modelin iskeleti, yani katmanlar ve yapı (örneğin: BERT).
- **Checkpoint:** O mimari üzerinde eğitilmiş özel ağırlıklar (örneğin: `bert-base-cased`).
- **Model:** Genel bir terim; bazen mimariyi, bazen checkpoint’i veya ikisini birden ifade eder.

Örneğin:
- "BERT" → bir mimaridir.  
- "bert-base-cased" → Google’ın bu mimariyle eğittiği ağırlıklardır.  
- "BERT modeli" → ikisini de kast edebilir.

---

📌 Bu temel bilgiler ışığında Transformer mimarilerinin detaylarına daha sonra daha derinlemesine dalacağız. Şimdilik genel yapıyı anlamanız yeterli.

