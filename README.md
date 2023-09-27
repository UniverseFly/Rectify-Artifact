# ⚙️$`\mathbb{R}\mathrm{ectify}`$🛠️

> [!WARNING]
> Rectify is renamed to **Repilot** and this repo will be archived soon. Please check our official repo **[ise-uiuc/Repilot](https://github.com/ise-uiuc/Repilot)** for comprehensive documentation and source code, along with an updated artifact documentation.


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
> When there is no GPU available, the CPU will be responsible for the patch generation.
> In this artifact documentation, we only explain the CPU-only Docker-based pipeline for conciseness.
> We encourage advanced readers who want to run the artifact with GPU support to check the [documentation](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/index.html) of NVIDIA Docker.

## Before we start

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
cd /rectify
echo "Hello Rectify!"
```

Congratulations! We are now ready for the artifact evaluation.

## Whet your appetite

Let's run some example scripts to see how Rectify works.

```bash
# The full rectify approach with CodeT5 as the base model
# Generate 5 patches for Chart-9 and save to `chart-9-rectify`
ACTIVE=1 python -m rectify.cli.main repair -b "Chart-9" --method pruned-mem -d chart-9-rectify -n 5
# You will see logs about the patch generation and which tokens are accepted/rejected.

# Validate the patch generation
python -m rectify.cli.main validate -d chart-9-rectify

# Print a table of the evaluation results
python -m rectify.cli.main evaluate -d chart-9-rectify
```

If everything works correctly, you will see a similar output table as follows:

```
root@1d7fea7789ed:/rectify# python -m rectify.cli.main evaluate -d chart-9-rectify
[chart-9-rectify] Loading raw generation data...
Done
[chart-9-rectify] Loading transformed raw generation data...
Done
[chart-9-rectify] Loading validation raw data...
Done
                                             Rectify Evaluation Results                                              
┏━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━━┓
┃ Tag             ┃ Average Gen Time ┃ %Compilable Patches ┃ %Plausible Patches ┃ #Plausible Fixes ┃ #Correct Fixes ┃
┡━━━━━━━━━━━━━━━━━╇━━━━━━━━━━━━━━━━━━╇━━━━━━━━━━━━━━━━━━━━━╇━━━━━━━━━━━━━━━━━━━━╇━━━━━━━━━━━━━━━━━━╇━━━━━━━━━━━━━━━━┩
│ chart-9-rectify │ 1.33s            │ 100.0%              │ 0.000%             │ 0                │ -              │
└─────────────────┴──────────────────┴─────────────────────┴────────────────────┴──────────────────┴────────────────┘
```

## Reproduce RQ Evaluation

We will now show how each RQ can be reproduced through the artifact by applying Rectify evaluation script on **pre-generated patches**.

> [!WARNING]
> We also provide documentation to [reproduce the entire patch generation](#reproduce-patch-generation) in different RQs, but it is not recommended for the readers to go through the entire process as it may take days or weeks to finish.

### RQ1: Comparison with existing tools

We will now reproduce Table 1, Figure 6, and the number of bugs fixed by removing the bugs that overlap with the CodeT5 training data, which is shown in Section 8 THREATS TO VALIDITY.

```bash
python -m rectify.cli.rq1
```

You will see two tables printed in the console, where the first table corresponds to Table 1 and the second table corresponds to following the sentence in Section 8:

> For comparison fairness, if we were to exclude these 7 and 6 bugs and compare them with the previous baseline tools on the remaining bugs, we are still able to achieve the highest bug fixes at 59 and 44 (best baseline at 45 and 29)

The detailed correct patches can be found through the following links:
- [Defects4j 1.2 correct patches](data/correct-patches/rq1/d4j1-codet5-template-rectify)
- [Defects4j 2.0 correct patches](data/correct-patches/rq1/d4j2-codet5-template-rectify)

Also the two venn diagrams shown in Figure 6 are saved in the `plots` directory. To check the plots, you may need to temporarily exit the Docker container and save the plots to your local machine:

```bash
# Exit the docker container with e.g., Ctrl-D
# Save the plots to your local machine
sudo docker cp rectify:/rectify/plots /path/to/your/local/directory
# Now you can open the plots with your favorite image viewer
# Return to the docker container
docker start -ai rectify

# Return to the `rectify` directory
cd /rectify
```

### RQ2: Compilation rate analysis

We will now reproduce Table 2. This script may take longer to run as it needs to iterate through 5000 generated patches per bug.

```bash
python -m rectify.cli.rq2
```

This command will print a table in the console, which corresponds to Table 2.

### RQ3: Component contribution

We now reproduce Table 3.

```bash
python -m rectify.cli.rq3
```

The detailed correct patches can be found through the following links:
- [[Vanilla] correct patches](data/correct-patches/rq3/d4j1-codet5-vanilla)
- [[NoMem] correct patches](data/correct-patches/rq3/d4j1-codet5-nomem)
- [[Mem] correct patches](data/correct-patches/rq3/d4j1-codet5-mem)
- [[Rectify] correct patches](data/correct-patches/rq3/d4j1-codet5-rectify)

### RQ4: Generalizability

This script will reproduce Table 4.

```bash
python -m rectify.cli.rq4
```

The detailed correct patches can be found through the following links:
- [CodeT5/D4J1.2 vanilla](data/correct-patches/rq3/d4j1-codet5-vanilla)
- [CodeT5/D4J1.2 rectify](data/correct-patches/rq3/d4j1-codet5-rectify)
- [CodeT5/D4J2.0 vanilla](data/correct-patches/rq4/d4j2-codet5-vanilla)
- [CodeT5/D4J2.0 rectify](data/correct-patches/rq4/d4j2-codet5-rectify)
- [Incoder/D4J1.2 vanilla](data/correct-patches/rq4/d4j1-incoder-vanilla)
- [Incoder/D4J1.2 rectify](data/correct-patches/rq4/d4j1-incoder-rectify)
- [Incoder/D4J2.0 vanilla](data/correct-patches/rq4/d4j2-incoder-vanilla)
- [Incoder/D4J2.0 rectify](data/correct-patches/rq4/d4j2-incoder-rectify)

🎉🎉🎉 Congratulations! You have successfully reproduced all the results in the paper! 🎉🎉🎉

## Reproduce patch generation

> [!WARNING]
> ⚠️⚠️⚠️ This section is mainly for advanced readers who have time to reproduce the entire patch generation process. These commands may take days or weeks to finish. Also, the generation time may vary significantly depending on the hardware used for patch generation. ⚠️⚠️⚠️

### RQ1

We generate Defects4j 1.2 single-hunk bugs and 2.0 single-line bugs with the help of repair templates. This is achieved through the following command:

```bash
D4J1_SINGLE_HUNK=1 ACTIVE=1 TEMPLATE=1 python -m rectify.cli.main repair -b ".*" --method pruned-mem -n 5000 -d rq1-d4j1
D4J2_SINGLE_LINE=1 ACTIVE=1 TEMPLATE=1 python -m rectify.cli.main repair -b ".*" --method pruned-mem -n 5000 -d rq1-d4j2
```

### RQ2

RQ2 is based on RQ1's generated patches, so we don't need to run any additional commands.

### RQ3

In RQ3, we generate 500 patches for each bug with 4 different configurations, using the following commands:

```bash
# Vanilla
D4J1_SINGLE_HUNK=1 python -m rectify.cli.main repair -b ".*" --method plain -n 500 -d rq3-vanilla
D4J1_SINGLE_HUNK=1 python -m rectify.cli.main repair -b ".*" --method pruned-nomem -n 500 -d rq3-nomem
D4J1_SINGLE_HUNK=1 python -m rectify.cli.main repair -b ".*" --method pruned-mem -n 500 -d rq3-mem
ACTIVE=1 D4J1_SINGLE_HUNK=1 python -m rectify.cli.main repair -b ".*" --method pruned-mem -n 500 -d rq3-rectify
```

### RQ4

We further include Incoder-6.7B as the base model to generate patches for RQ4.

> [!WARNING]
> Incoder is not downloaded beforehand in the image due to its large size, so you may spend extra time waiting for the download to finish.

```bash
# The first two configurations are the same as RQ3
# D4J1_SINGLE_HUNK=1 python -m rectify.cli.main repair -b ".*" --method plain -n 500 -d rq3-vanilla
# ACTIVE=1 D4J1_SINGLE_HUNK=1 python -m rectify.cli.main repair -b ".*" --method plain -n 500 -d rq3-rectify


D4J2_SINGLE_HUNK=1 python -m rectify.cli.main repair -b ".*" --method plain -n 500 -d rq4-codet5-d4j2-vanilla
ACTIVE=1 D4J2_SINGLE_HUNK=1 python -m rectify.cli.main repair -b ".*" --method pruned-mem -n 500 -d rq4-codet5-d4j2-rectify

INCODER=1 D4J1_SINGLE_HUNK=1 python -m rectify.cli.main repair -b ".*" --method plain -n 500 -d rq4-incoder-d4j1-vanilla
INCODER=1 ACTIVE=1 D4J1_SINGLE_HUNK=1 python -m rectify.cli.main repair -b ".*" --method pruned-mem -n 500 -d rq4-incoder-d4j1-rectify

INCODER=1 D4J2_SINGLE_HUNK=1 python -m rectify.cli.main repair -b ".*" --method plain -n 500 -d rq4-incoder-d4j2-vanilla
INCODER=1 ACTIVE=1 D4J2_SINGLE_HUNK=1 python -m rectify.cli.main repair -b ".*" --method pruned-mem -n 500 -d rq4-incoder-d4j2-rectify
```
