# ⚙️$`\mathbb{R}\mathrm{ectify}`$🛠️

Welcome to the artifact repository for Rectify, a patch generation tool introduced in the ESEC/FSE'23 paper **"Copiloting the Copilots: Fusing Large Language Models with Completion Engines for Automated Program Repair"**!

> [!IMPORTANT]
>
> **Environment requirements**
> - **OS**: A Linux system with **[Docker](https://docs.docker.com/engine/install/)** support.
>   - Optional: [NVIDIA Docker](https://github.com/NVIDIA/nvidia-docker) support.
> - **Hardware**: X86/X64 CPU; 16GB RAM; 1TB Storage; Good Network to Docker Hub.
>   - Optional (a): NVIDIA GPU(s) with >6G memory (for CodeT5 patch generation)
>   - Optional (b): NVIDIA GPU(s) with >30G memory (for Incoder-6.7B patch generation)
> 
> Although it is recommended to run the artifact with NVIDIA GPUs for faster patch generation, it is not a requirement.
> In this case, the CPU will be responsible for the patch generation.
> In this artifact documentation, we only explain the CPU-only Docker-based pipeline for conciseness.
> We encourage advanced readers who want to run the artifact with GPU support to check the [documentation](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/index.html) of NVIDIA Docker.

> [!NOTE]
>
> Before you start, let's make sure Docker is installed: [Installation Guide](https://docs.docker.com/engine/install/).
>
> To check the installation:
> ```bash
> docker --version # Test docker availability
> # Docker version 20.10.21, build 20.10.21-0ubuntu1~20.04.2
> ```
> 
