# ⚙️$`\mathbb{R}\mathrm{ectify}`$🛠️

Welcome to the artifact repository for **Rectify**, a patch generation tool introduced in the ESEC/FSE'23 paper **"Copiloting the Copilots: Fusing Large Language Models with Completion Engines for Automated Program Repair"**!

> [!IMPORTANT]
>
> **Environment requirements**
> 
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

# Before we start

Before we start, let's first make sure Docker is installed: [Installation Guide](https://docs.docker.com/engine/install/).

To check the installation:

```bash
docker --version # Test docker availability
# Docker version 20.10.21, build 20.10.21-0ubuntu1~20.04.2
``` 

Now we'll fetch the Docker image of Rectify that includes the implementation of the Algorithm, the Completion Engine, and all the dependencies needed:

```bash
# This may take some time...
docker pull universefly/rectify-fse23:latest

# Run the docker image
docker run -it --name rectify universefly/rectify-fse23
# Now you will get into a "virtual environment" provided by Docker
# Enter the `rectify` directory
cd rectify
echo "Hello Rectify!"
```

Congratulations! We are now ready for the artifact evaluation.

# Whet your appetite

Let's run some example scripts to see how Rectify works.

```bash
# The full rectify approach with CodeT5 as the base model
# Generate 5 patches for Chart-9 and save to `chart-9-rectify`
ACTIVE=1 python -m rectify.cli.main repair -b "Chart-9" --method "pruned-mem" -d chart-9-rectify -n 5
# You will see logs about the patch generation and which tokens are accepted/rejected.

# Validate the patch generation
python -m rectify.cli.main validate -d chart-9-rectify

# Print a table of the evaluation results
python -m rectify.cli.main evaluate -d chart-9-rectify
```

If everything works correctly, you will see an output table as follows:

TBD

# Reproduce RQ Evaluation

We will now show how each RQ can be reproduced through the artifact by applying Rectify evaluation script on **pre-generated patches**.

> [!WARNING]
> We also provide documentation to [reproduce the entire generation](#reproduce-patch-generation), but it is not recommended for the readers to go through the entire process as it may take days or weeks to finish.

# Reproduce patch generation

TBD
