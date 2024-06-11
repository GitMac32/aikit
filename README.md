# AIKit ✨

<p align="center">
<img src="./website/static/img/logo.png" width="200"><br>
</p>

AIKit is a one-stop shop to quickly get started to host, deploy, build and fine-tune large language models (LLMs).

AIKit offers two main capabilities:

- **Inference**: AIKit uses [LocalAI](https://localai.io/), which supports a wide range of inference capabilities and formats. LocalAI provides a drop-in replacement REST API that is OpenAI API compatible, so you can use any OpenAI API compatible client, such as [Kubectl AI](https://github.com/sozercan/kubectl-ai), [Chatbot-UI](https://github.com/sozercan/chatbot-ui) and many more, to send requests to open LLMs!

- **[Fine Tuning](https://sozercan.github.io/aikit/docs/fine-tune)**: AIKit offers an extensible fine tuning interface. It supports [Unsloth](https://github.com/unslothai/unsloth) for fast, memory efficient, and easy fine-tuning experience.

👉 For full documentation, please see [AIKit website](https://sozercan.github.io/aikit/)!

## Features

- 🐳 No GPU, Internet access or additional tools needed except for [Docker](https://docs.docker.com/desktop/install/linux-install/)!
- 🤏 Minimal image size, resulting in less vulnerabilities and smaller attack surface with a custom [distroless](https://github.com/GoogleContainerTools/distroless)-based image
- 🎵 [Fine tune support](https://sozercan.github.io/aikit/docs/fine-tune)
- 🚀 Easy to use declarative configuration for [inference](https://sozercan.github.io/aikit/docs/specs-inference) and [fine tuning](https://sozercan.github.io/aikit/docs/specs-finetune)
- ✨ OpenAI API compatible to use with any OpenAI API compatible client
- 📸 [Multi-modal model support](https://sozercan.github.io/aikit/docs/vision)
- 🖼️ Image generation support with [Stable Diffusion](https://sozercan.github.io/aikit/docs/stablediffusion)
- 🦙 Support for GGUF ([`llama`](https://github.com/ggerganov/llama.cpp)), GPTQ ([`exllama`](https://github.com/turboderp/exllama) or [`exllama2`](https://github.com/turboderp/exllamav2)), EXL2 ([`exllama2`](https://github.com/turboderp/exllamav2)), and GGML ([`llama-ggml`](https://github.com/ggerganov/llama.cpp)) and [Mamba](https://github.com/state-spaces/mamba) models
- 🚢 [Kubernetes deployment ready](#kubernetes-deployment)
- 📦 Supports multiple models with a single image
- 🖥️ Supports [AMD64 and ARM64](https://sozercan.github.io/aikit/docs/create-images#multi-platform-support) CPUs and [GPU-accelerated inferencing with NVIDIA GPUs](https://sozercan.github.io/aikit/docs/gpu)
- 🔐 Ensure [supply chain security](https://sozercan.github.io/aikit/docs/security) with SBOMs, Provenance attestations, and signed images
- 🌈 Supports air-gapped environments with self-hosted, local, or any remote container registries to store model images for inference on the edge.

## Quick Start

You can get started with AIKit quickly on your local machine without a GPU!

```bash
docker run -d --rm -p 8080:8080 ghcr.io/sozercan/llama3:8b
```

After running this, navigate to [http://localhost:8080/chat](http://localhost:8080/chat) to access the WebUI!

### API

AIKit provides an OpenAI API compatible endpoint, so you can use any OpenAI API compatible client to send requests to open LLMs!

```bash
curl http://localhost:8080/v1/chat/completions -H "Content-Type: application/json" -d '{
    "model": "llama-3-8b-instruct",
    "messages": [{"role": "user", "content": "explain kubernetes in a sentence"}]
  }'
```

Output should be similar to:

```jsonc
{
  // ...
    "model": "llama-3-8b-instruct",
    "choices": [
        {
            "index": 0,
            "finish_reason": "stop",
            "message": {
                "role": "assistant",
                "content": "Kubernetes is an open-source container orchestration system that automates the deployment, scaling, and management of applications and services, allowing developers to focus on writing code rather than managing infrastructure."
            }
        }
    ],
  // ...
}
```

That's it! 🎉 API is OpenAI compatible so this is a drop-in replacement for any OpenAI API compatible client.

## Pre-made Models

AIKit comes with pre-made models that you can use out-of-the-box!

If it doesn't include a specific model, you can always [create your own images](https://sozercan.github.io/aikit/docs/create-images), and host in a container registry of your choice!

## CPU

| Model           | Optimization | Parameters | Command                                                             | Model Name              | License                                                                             |
| --------------- | ------------ | ---------- | ------------------------------------------------------------------- | ----------------------- | ----------------------------------------------------------------------------------- |
| 🦙 Llama 3       | Instruct     | 8B         | `docker run -d --rm -p 8080:8080 ghcr.io/sozercan/llama3:8b`        | `llama-3-8b-instruct`   | [Llama](https://ai.meta.com/llama/license/)                                         |
| 🦙 Llama 3       | Instruct     | 70B        | `docker run -d --rm -p 8080:8080 ghcr.io/sozercan/llama3:70b`       | `llama-3-70b-instruct`  | [Llama](https://ai.meta.com/llama/license/)                                         |
| 🦙 Llama 2       | Chat         | 7B         | `docker run -d --rm -p 8080:8080 ghcr.io/sozercan/llama2:7b`        | `llama-2-7b-chat`       | [Llama](https://ai.meta.com/llama/license/)                                         |
| 🦙 Llama 2       | Chat         | 13B        | `docker run -d --rm -p 8080:8080 ghcr.io/sozercan/llama2:13b`       | `llama-2-13b-chat`      | [Llama](https://ai.meta.com/llama/license/)                                         |
| Ⓜ️ Mixtral       | Instruct     | 8x7B       | `docker run -d --rm -p 8080:8080 ghcr.io/sozercan/mixtral:8x7b`     | `mixtral-8x7b-instruct` | [Apache](https://choosealicense.com/licenses/apache-2.0/)                           |
| 🅿️ Phi 3         | Instruct     | 3.8B       | `docker run -d --rm -p 8080:8080 ghcr.io/sozercan/phi3:3.8b`        | `phi-3-3.8b`            | [MIT](https://huggingface.co/microsoft/Phi-3-mini-4k-instruct/resolve/main/LICENSE) |
| 🔡 Gemma 1.1     | Instruct     | 2B         | `docker run -d --rm -p 8080:8080 ghcr.io/sozercan/gemma:2b`      | `gemma-2b-instruct` | [Gemma](https://ai.google.dev/gemma/terms)                                          |
| ⌨️ Codestral 0.1 | Code         | 22B        | `docker run -d --rm -p 8080:8080 ghcr.io/sozercan/codestral:22b` | `codestral-22b`     | [MNLP](https://mistral.ai/licenses/MNPL-0.1.md)                                     |

### NVIDIA CUDA

> [!NOTE]
> To enable GPU acceleration, please see [GPU Acceleration](https://sozercan.github.io/aikit/docs/gpu).
> Please note that only difference between CPU and GPU section is the `--gpus all` flag in the command to enable GPU acceleration.

| Model           | Optimization | Parameters | Command                                                                        | Model Name              | License                                                                             |
| --------------- | ------------ | ---------- | ------------------------------------------------------------------------------ | ----------------------- | ----------------------------------------------------------------------------------- |
| 🦙 Llama 3       | Instruct     | 8B         | `docker run -d --rm --gpus all -p 8080:8080 ghcr.io/sozercan/llama3:8b`        | `llama-3-8b-instruct`   | [Llama](https://ai.meta.com/llama/license/)                                         |
| 🦙 Llama 3       | Instruct     | 70B        | `docker run -d --rm --gpus all -p 8080:8080 ghcr.io/sozercan/llama3:70b`       | `llama-3-70b-instruct`  | [Llama](https://ai.meta.com/llama/license/)                                         |
| 🦙 Llama 2       | Chat         | 7B         | `docker run -d --rm --gpus all -p 8080:8080 ghcr.io/sozercan/llama2:7b`        | `llama-2-7b-chat`       | [Llama](https://ai.meta.com/llama/license/)                                         |
| 🦙 Llama 2       | Chat         | 13B        | `docker run -d --rm --gpus all -p 8080:8080 ghcr.io/sozercan/llama2:13b`       | `llama-2-13b-chat`      | [Llama](https://ai.meta.com/llama/license/)                                         |
| Ⓜ️ Mixtral       | Instruct     | 8x7B       | `docker run -d --rm --gpus all -p 8080:8080 ghcr.io/sozercan/mixtral:8x7b`     | `mixtral-8x7b-instruct` | [Apache](https://choosealicense.com/licenses/apache-2.0/)                           |
| 🅿️ Phi 3         | Instruct     | 3.8B       | `docker run -d --rm --gpus all -p 8080:8080 ghcr.io/sozercan/phi3:3.8b`        | `phi-3-3.8b`            | [MIT](https://huggingface.co/microsoft/Phi-3-mini-4k-instruct/resolve/main/LICENSE) |
| 🔡 Gemma 1.1     | Instruct     | 2B         | `docker run -d --rm --gpus all -p 8080:8080 ghcr.io/sozercan/gemma:2b`      | `gemma-2b-instruct` | [Gemma](https://ai.google.dev/gemma/terms)                                          |
| ⌨️ Codestral 0.1 | Code         | 22B        | `docker run -d --rm --gpus all -p 8080:8080 ghcr.io/sozercan/codestral:22b` | `codestral-22b`     | [MNLP](https://mistral.ai/licenses/MNPL-0.1.md)                                     |

## What's next?

👉 For more information and how to fine tune models or create your own images, please see [AIKit website](https://sozercan.github.io/aikit/)!
