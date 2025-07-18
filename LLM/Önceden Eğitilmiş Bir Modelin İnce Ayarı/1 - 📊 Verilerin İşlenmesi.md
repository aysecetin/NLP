# 📊 Verilerin İşlenmesi

## 🔢 Tokenizer ile Veri Hazırlığı

Bir tokenizer, cümleleri modele uygun sayısal tensörlere dönüştürmek için kullanılır. Örnek olarak, `"bert-base-uncased"` modeli ile cümle çiftleri token'lanır. Bu işlem:
- Padding (doldurma),
- Truncation (kısaltma),
- Attention mask ve token type ID üretimini kapsar.

## 📚 MRPC Dataset Örneği

GLUE benchmark'ındaki MRPC veri kümesi 5801 cümle çifti içerir. Cümlelerin eşdeğer (paraphrase) olup olmadığını gösteren etiketler içerir. `load_dataset("glue", "mrpc")` ile kolayca yüklenebilir.

Dataset yükledikten sonra `sentence1`, `sentence2`, `label` gibi sütunlara erişebilirsiniz.

## ✂️ Tokenization & Ön İşleme

Model iki cümleyi birlikte işleyecekse, tokenizer iki cümleyi tek seferde alabilir:
```python
tokenizer("Bu ilk cümledir.", "Bu ikinci cümledir.")
```
Bu işlem `input_ids`, `attention_mask`, ve `token_type_ids` döndürür.  
- `token_type_ids` her token’ın hangi cümleye ait olduğunu belirtir.  
- Bazı modeller (örneğin DistilBERT) bunu desteklemez.

## 🛠️ Dataset.map() ile Tokenleştirme

Veri kümesini dönüştürmenin en verimli yolu `Dataset.map()` fonksiyonudur.  
```python
def tokenize_function(example):
    return tokenizer(example["sentence1"], example["sentence2"], truncation=True)

tokenized_datasets = raw_datasets.map(tokenize_function, batched=True)
```
Bu yöntem, belleğe tüm dataset'i yüklemeye gerek kalmadan çalışır.

## 🧩 Dinamik Padding

Daha verimli eğitim için dinamik padding önerilir. Bunun için `DataCollatorWithPadding` kullanılır:
```python
from transformers import DataCollatorWithPadding
data_collator = DataCollatorWithPadding(tokenizer=tokenizer)
```
Bu yapı, batch içerisindeki en uzun girdiye göre padding yapar.

## 🚀 Önemli Noktalar

- `batched=True` ile `Dataset.map()` işlemleri hızlandırılır.
- Dinamik padding, sabit padding’e göre daha verimlidir.
- Veri işleme çıktısı, modelin beklediği formatta olmalıdır.
