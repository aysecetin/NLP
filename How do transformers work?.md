# 🚀 Transformer Modelleri Nedir ve Nasıl Çalışır?

## 1. Transformer Nedir?
Transformer, doğal dil işleme (NLP) alanında devrim yaratan bir model mimarisidir. İlk olarak 2017 yılında tanıtıldı. Temel olarak “attention (dikkat)” mekanizmasıyla çalışır. Bu mekanizma sayesinde model, bir cümlede hangi kelimenin diğerine ne kadar önemli olduğunu öğrenir.

---

## 2. Ana Yapısı Nasıldır?
Transformer iki ana bloktan oluşur:

- **Encoder (Kodlayıcı)**: Girdi metnini anlar.  
- **Decoder (Kod Çözücü)**: Anlaşılan girdiye göre çıktı üretir (örneğin çeviri yapar).

Kullanım durumlarına göre bu bloklar şöyle kullanılır:

- **Sadece Encoder**: Sınıflandırma, duygu analizi gibi anlayan görevler.
- **Sadece Decoder**: Metin üretimi gibi üretici görevler.
- **Encoder + Decoder**: Çeviri, özetleme gibi girişten çıkış üreten görevler.

---

## 3. Dikkat (Attention) Mekanizması
Model, her kelimeye eşit davranmaz. Bazı kelimelere daha fazla "dikkat eder".  
Örneğin `"You like this course"` cümlesinde `"like"` kelimesini doğru çevirebilmek için `"You"` kelimesine de dikkat etmek gerekir.

---

## 4. Transformer Ailesi ve Örnek Modeller
Bazı önemli Transformer tabanlı modeller:

- **GPT (2018)**: Metin üretiminde başarılı.  
- **BERT (2018)**: Cümle anlamı üzerine güçlü.  
- **T5 (2019)**: Her şeyi çeviri gibi ele alır (input → output).  
- **GPT-3 (2020)**: Devasa boyutta, çok yönlü.  
- **InstructGPT (2022)**: Komutları takip etmeyi öğrenmiş hali.  
- **Llama, Mistral, Gemma, SmolLM** gibi daha yeni modeller de mevcuttur.

---

## 5. Model Eğitimi: Pretraining ve Fine-Tuning

- **Pretraining (Ön Eğitim)**: Model büyük veriyle sıfırdan eğitilir.
- **Fine-Tuning (İnce Ayar)**: Önceden eğitilmiş model, daha küçük ve özel veriyle belirli bir göreve adapte edilir.
  - Bu yöntem daha hızlı, ucuz ve çevreci bir alternatiftir.

---

## 6. Transformer’lar Büyük ve Güçlüdür
Transformer modelleri milyarlarca parametreye sahip olabilir.  
Bu yüzden eğitimi pahalı ve çevresel açıdan maliyetlidir.  
Model paylaşımı, kaynak israfını önlemek için çok önemlidir.

---

## 7. Terimler

- **Mimari (Architecture)**: Modelin yapısı (örneğin `BERT`).
- **Checkpoint**: Mimariyle eğitilmiş ağırlıklar (örneğin `bert-base-cased`).
- **Model**: Genel bir terim olup mimari veya checkpoint anlamında kullanılabilir.
h
