# Ollama — Custom GTX 1080 Ti Build

Ollama dropped support for older NVIDIA cards, so I built my own image and hosted it on my self-hosted Docker registry.

## The Problem

Newer Ollama releases no longer include CUDA backends for **Pascal** architecture GPUs (like the GTX 1080 Ti). My TrueNAS server has a GTX 1080 Ti and I wanted GPU-accelerated inference.

## The Solution

I compiled Ollama from source with a CUDA 12 backend targeting **sm_61** (Pascal compute capability 6.1) and pushed the image to my private registry.

### Process

1. **Built on a PC with an RTX 5070 Ti** — The build GPU doesn't matter; I explicitly forced the target architecture to `sm_61` so the CUDA backend is compiled only for the GTX 1080 Ti, keeping the image small.
2. **Custom Dockerfile** — Multi-stage build using `nvidia/cuda:12.4.1-devel-ubuntu22.04` for compilation and `nvidia/cuda:12.4.1-runtime-ubuntu22.04` for the final image. Key CMake flags:
   
   - `-DOLLAMA_LLAMA_BACKENDS="cuda_v12"`
   - `-DCMAKE_CUDA_ARCHITECTURES="61"`
3. **Pushed to self-hosted registry** — The image is hosted on a private registry at `[redacted]/docker-harbour/ollama-1080ti:latest`.
4. **Deployed on TrueNAS** — The TrueNAS host is configured with `insecure-registries` and the NVIDIA container runtime. It pulls the image and runs alongside Open WebUI via Docker Compose.

## Deployment

```bash
# Pull the custom image on TrueNAS
sudo docker pull [redacted]/docker-harbour/ollama-1080ti:latest

# Run with Open WebUI
sudo docker compose -f new.yaml up -d
```

Ollama on port `11434`, Open WebUI on port `3001`.

