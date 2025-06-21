# 📚 Transformer Modelleri ve LLM’lere Giriş – Özet

### 🧠 Doğal Dil İşleme (NLP) ve LLM’ler

- NLP, metin sınıflandırmadan metin üretimine kadar birçok görevi kapsar.
- LLM’ler, büyük miktarda metin verisiyle eğitilmiş, çok güçlü dil modelleridir.
- Tek bir model, çok çeşitli görevleri yerine getirebilir.
- Ancak bu modellerin **halüsinasyon üretme**, **önyargı taşıma** gibi sınırlılıkları da vardır.



### ⚙️ Transformer Yetenekleri

🤗 Transformers kütüphanesindeki `pipeline()` fonksiyonu ile önceden eğitilmiş modelleri kolayca kullanabilirsin:

- Metin sınıflandırma, varlık tanıma, soru-cevaplama
- Metin üretimi ve özetleme
- Çeviri ve diğer diziden diziye görevler
- Ses tanıma ve görüntü sınıflandırma



## 🏗️ Transformer Mimarisi

Transformer modelinin temel çalışma prensipleri:

- **Attention (dikkat)** mekanizması modelin bağlamı anlamasını sağlar.
- **Transfer learning** ile modeller, yeni görevlerde yeniden kullanılabilir.
- Üç ana mimari türü vardır:
  - **Encoder-only** (sadece kodlayıcı)
  - **Decoder-only** (sadece çözücü)
  - **Encoder-decoder** (hem kodlayıcı hem çözücü)



## 🧩 Model Mimarileri ve Kullanım Alanları

| Mimari Türü     | Örnek Modeller                   | Kullanım Alanları                                       |
|------------------|-----------------------------------|----------------------------------------------------------|
| Encoder-only     | BERT, DistilBERT, ModernBERT      | Cümle sınıflandırma, varlık tanıma, bilgi çıkarımı       |
| Decoder-only     | GPT, LLaMA, Gemma, SmolLM         | Metin üretimi, sohbet botları, yaratıcı yazım            |
| Encoder-decoder  | BART, T5, Marian, mBART           | Özetleme, çeviri, üretici soru-cevap                     |



## 🚀 Modern LLM Gelişmeleri

- Modeller zamanla hem boyut hem de yetenek açısından büyüdü.
- **Scaling laws** (ölçekleme yasaları), modelin boyutuyla performansı nasıl artıracağını öngörür.
- Daha uzun bağlamları işlemek için **özel attention mekanizmaları** geliştirildi.
- Eğitim süreci iki aşamaya ayrılmıştır:
  - **Pretraining** (ön eğitim)
  - **Instruction tuning** (talimatla ince ayar)



## 🛠️ Pratik Uygulamalar

Bu bölümde öğrendiklerinle, modelleri gerçek dünyada nasıl kullanabileceğini de gördün:

- **Hugging Face Hub** üzerinden önceden eğitilmiş modelleri bulmak ve kullanmak
- **Inference API** ile modelleri doğrudan tarayıcıda test etmek
- Hangi modelin hangi göreve uygun olduğunu anlamak



## 🔭 Geleceğe Bakış

Artık Transformer modellerinin nasıl çalıştığına dair sağlam bir temel oluşturduğuna göre, sonraki bölümlerde şunları öğreneceksin:

- Transformers kütüphanesiyle modelleri nasıl yükleyip ince ayar yapacağın
- Farklı veri türlerini modele nasıl hazırlayacağın
- Önceden eğitilmiş modelleri kendi görevlerine nasıl uyarlayacağın
- Bu modelleri nasıl dağıtacağın (deploy)
