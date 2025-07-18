
# 🚀 Optimize Edilmiş Inference Dağıtımı

Bu bölümde büyük dil modellerini (LLM) **üretim ortamında** verimli bir şekilde sunmak için kullanılan üç güçlü framework’ü inceleyeceğiz:

- **TGI (Text Generation Inference)**
- **vLLM**
- **llama.cpp**

Bu sistemler; yüksek verimlilik, hafıza yönetimi, esneklik ve üretime kolay entegrasyon açısından farklı avantajlar sunar.



### ⚙️ Framework Karşılaştırma Tablosu

| Özellik | TGI | vLLM | llama.cpp |
|--------|-----|------|-----------|
| 🚀 Performans | Flash Attention 2 + sürekli batchleme | PagedAttention ile KV cache optimizasyonu | CPU için optimizasyon + quantization |
| 🧠 Bellek Kullanımı | Sabit uzunluklu sekanslarla tahmin edilebilir | Sayfa tabanlı bellek yönetimi | 2-bit’e kadar quantization |
| 📦 Dağıtım | Kubernetes desteği, Prometheus, Grafana | OpenAI API uyumu, Ray entegrasyonu | Minimal, C/C++ tabanlı, portatif |
| 🧱 Donanım Desteği | GPU zorunlu | GPU | CPU ağırlıklı, opsiyonel GPU |



### 💡 TGI: Flash Attention 2

- **Flash Attention**, belleğe veri transferini minimuma indirerek GPU’yu daha verimli kullanır.
- Bellek taşmaları önlenir, VRAM verimli kullanılır.
- Sürekli batchleme ile GPU hep meşgul tutulur.



### 🔁 vLLM: PagedAttention

- **PagedAttention**, KV cache yönetiminde “sayfalama” yaparak bellek parçalanmasını azaltır.
- Bellek paylaşımı sayesinde paralel örnekler daha verimli işlenir.
- 24 kata kadar throughput artışı sağlanabilir.



### 🧮 llama.cpp: Quantization + CPU Optimizasyonu

- 32-bit FP yerine 8-bit, 4-bit, 2-bit gibi quantization ile daha az bellek kullanımı
- GGUF formatı ile CPU dostu inference
- AVX2, AVX-512 gibi CPU mimarileri için optimize edilmiş



### 🧪 Kurulum ve Sunucu Başlatma

### 🔹 TGI

```bash
docker run --gpus all -p 8080:80     -v ~/.cache/huggingface:/data     ghcr.io/huggingface/text-generation-inference:latest     --model-id HuggingFaceTB/SmolLM2-360M-Instruct
```

### 🔹 vLLM

```bash
python -m vllm.entrypoints.openai.api_server \
    --model HuggingFaceTB/SmolLM2-360M-Instruct \
    --host 0.0.0.0 \
    --port 8000
```

### 🔹 llama.cpp

```bash
git clone https://github.com/ggerganov/llama.cpp
cd llama.cpp
make

./server -m model.gguf --port 8080
```



### 🧵 Metin Üretimi (Text Generation)

Tüm framework’ler hem **chat** hem de **text generation** için kullanılır:

**Örnek (Hugging Face Client ile):**

```python
from huggingface_hub import InferenceClient

client = InferenceClient(model="http://localhost:8080")

response = client.text_generation(
    "Tell me a story",
    max_new_tokens=100,
    temperature=0.8,
    top_p=0.95,
    repetition_penalty=1.1,
    details=True,
)
print(response.generated_text)
```

---

### 🎛️ Gelişmiş Kontroller

**Sampling Ayarları:**

- `temperature`: Rastgelelik (yüksekse daha yaratıcı)
- `top_p`: Belirli olasılık eşiğine kadar olan token’lar
- `top_k`: En olası k token’dan seçim

**Tekrardan Kaçınma:**

- `repetition_penalty`: Aynı token tekrarını engeller
- `frequency_penalty`: Sık gelen token’lara ceza
- `presence_penalty`: Önceki token’lara ceza

**Uzunluk & Durdurma:**

- `max_tokens`: Maksimum üretim uzunluğu
- `stop_sequences`: Belirli diziler geldiğinde durdur



### 💾 Bellek Yönetimi

### TGI

```bash
docker run --gpus all -p 8080:80 \
    --shm-size 1g \
    ghcr.io/huggingface/text-generation-inference:latest \
    --model-id ... \
    --max-batch-total-tokens 8192 \
    --max-input-length 4096
```

### llama.cpp

```bash
./server \
    -m model.gguf \
    --n-gpu-layers 20 \
    --mlock \
    --cont-batching
```

### vLLM

```python
from vllm.engine.arg_utils import AsyncEngineArgs

engine_args = AsyncEngineArgs(
    model="...",
    gpu_memory_utilization=0.85,
    max_num_batched_tokens=8192,
    block_size=16,
)

llm = LLM(engine_args=engine_args)
```

Bu üç sistem de farklı senaryolar için idealdir:

- **TGI**: Kurumsal, stabil, izlenebilirlik yüksek dağıtımlar için
- **vLLM**: Esnek, performans odaklı, API uyumlu sistemler için
- **llama.cpp**: Donanımı kısıtlı, taşınabilir yerel uygulamalar için

