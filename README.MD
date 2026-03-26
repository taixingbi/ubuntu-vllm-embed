# vLLM Embeddings (Ubuntu GPU)

Run **BAAI/bge-m3** embeddings with vLLM on Ubuntu using an NVIDIA GPU.

## Prerequisites

- **NVIDIA GPU** — 24GB VRAM recommended for embeddings
- **NVIDIA drivers + CUDA** — `nvidia-smi` works
- **Docker** — [Docker Engine](https://docs.docker.com/engine/install/)

```bash
sudo apt  install docker.io
sudo apt install docker-compose-plugin -y
```

- **NVIDIA Container Toolkit** — [Install guide](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html); [docker.md](docker.md) for Ubuntu fix. Verify: `docker run --rm --gpus all nvidia/cuda:12.0-base nvidia-smi`
- **Python 3.10+** — for local run only

## Run with Docker

```bash
docker compose up -d
```

- **Endpoint:** Embeddings `http://localhost:8001/v1/embeddings`
- **Logs:** `docker logs -f vllm-embeddings`
- **Gated models:** set `HUGGING_FACE_HUB_TOKEN` in env or `.env`; models cached in a Docker volume.

**Test from another machine** (e.g. Mac): use host IP (e.g. `192.168.86.173`) or `localhost` on the same machine.

Embeddings:

```bash
curl http://192.168.86.173:8001/v1/embeddings \
  -H "Content-Type: application/json" \
  -d '{"model":"BAAI/bge-m3","input":"Hello"}'
```

## Local run (no Docker)

Use a venv on Ubuntu/Debian to avoid the externally-managed-environment error:

```bash
source /home/tb/miniconda3/etc/profile.d/conda.sh
conda --version
conda env list
conda create -n vllm python=3.11 -y
conda activate vllm
pip install -U pip
pip install vllm
```

**Embeddings:**

```bash
vllm serve BAAI/bge-m3 --host 0.0.0.0 --port 8001 --gpu-memory-utilization 0.07
```

## Models

- [BAAI/bge-m3](https://huggingface.co/BAAI/bge-m3) — embeddings
- [vLLM docs](https://docs.vllm.ai/)
