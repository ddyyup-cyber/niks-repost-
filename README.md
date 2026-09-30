# Qwen3-14B Uncensored di Google Colab

Model: `mradermacher/Qwen3-14B-Uncensored-GGUF:Q6_K` (12 GB)  
GPU: Tesla T4 (Colab Free)  
API Key: `niksganteng098`

---

## Cara Pakai

1. Buka [Google Colab](https://colab.research.google.com)
2. Buat **New notebook**
3. Runtime → Change runtime type → **T4 GPU** → Save
4. Copy-paste cell di bawah ini **satu per satu**

---

## Cell 1 – Install zstd

```python
!apt-get update -qq && apt-get install -y zstd
```

---

## Cell 2 – Install Ollama

```python
!curl -fsSL https://ollama.com/install.sh | sh
```

---

## Cell 3 – Jalankan Ollama Server

```python
import subprocess, time
subprocess.Popen(["ollama", "serve"])
time.sleep(12)
print("Ollama server sudah jalan")
```

---

## Cell 4 – Cek GPU

```python
!nvidia-smi
!ollama list
```

Harus muncul **Tesla T4**.

---

## Cell 5 – Download Model (lama, ~12 GB)

```python
!ollama pull hf.co/mradermacher/Qwen3-14B-Uncensored-GGUF:Q6_K
```

Tunggu sampai selesai (5–15 menit).

---

## Cell 6 – Cek Model Sudah Ada

```python
!ollama list
```

Harus muncul:
```
hf.co/mradermacher/Qwen3-14B-Uncensored-GGUF:Q6_K   12 GB
```

---

## Cell 7 – Test Model

```python
import ollama

response = ollama.chat(
    model='hf.co/mradermacher/Qwen3-14B-Uncensored-GGUF:Q6_K',
    messages=[{'role': 'user', 'content': 'Halo, siapa kamu?'}]
)
print(response['message']['content'])
```

Kalau model jawab → **sudah jalan**.

---

## Cell 8 – Install Cloudflared (buat public API)

```python
!wget -q https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb
!dpkg -i cloudflared-linux-amd64.deb
```

---

## Cell 9 – Buka Public Tunnel

```python
import subprocess, threading, time

def run_tunnel():
    subprocess.run([
        "cloudflared", "tunnel", "--url", "http://localhost:11434",
        "--no-autoupdate"
    ])

threading.Thread(target=run_tunnel, daemon=True).start()
time.sleep(12)
print("Tunggu muncul link https://xxxx.trycloudflare.com di output...")
```

Copy link yang muncul (contoh: `https://xxxxx.trycloudflare.com`)

---

## Cara Pakai di Aplikasi (Ganti Gemini)

```python
from openai import OpenAI

client = OpenAI(
    base_url="https://xxxxx.trycloudflare.com/v1",  # ganti dengan link cloudflare kamu
    api_key="niksganteng098"
)

response = client.chat.completions.create(
    model="hf.co/mradermacher/Qwen3-14B-Uncensored-GGUF:Q6_K",
    messages=[
        {"role": "user", "content": "Halo, ini model sendiri"}
    ]
)

print(response.choices[0].message.content)
```

---

## Ringkasan Setting

| Setting     | Isi |
|------------|-----|
| **base_url** | `https://xxxx.trycloudflare.com/v1` |
| **api_key**  | `niksganteng098` |
| **model**    | `hf.co/mradermacher/Qwen3-14B-Uncensored-GGUF:Q6_K` |

---

## Catatan

- Colab free bisa disconnect setelah idle / ~12 jam
- Kalau runtime restart, harus jalankan ulang dari Cell 3 (Ollama serve) + Cell 5 (kalau model hilang)
- Jangan tutup tab Colab saat model sedang dipakai
