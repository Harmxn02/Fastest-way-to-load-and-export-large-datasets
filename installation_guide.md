# Installation Guide

## Recommended Installation Method

For the best experience, follow the official guide: <https://docs.rapids.ai/install/#wsl2>. Since it's written by the developers, it’s the most reliable source of information.

I personally used the **"WSL2 SDK Manager Install"** method. It took a long time to install, but everything worked on the first try, including the creation of a new virtual environment.

If you already have `conda` set up in WSL, you can also use the **"Release Selector"** method.

### Finding the Conda Environment Name

If you opt for the **"WSL2 SDK Manager Install"** method, you can find the name of the conda environment by running:

```bash
conda info --envs
```

For me, it was `rapids-24.12`, which you can activate with:

```bash
conda activate rapids-24.12
```

### Steps That Still Apply

Even if you follow the official guide, steps **1, 2, 5, 6, and 7** in this guide are still relevant, but they are also mentioned in the official documentation.

---

## Prerequisites

### CUDA Toolkit

Ensure you have **CUDA Toolkit 11.1** installed. You can download it [here](https://developer.nvidia.com/cuda-11.1.1-download-archive).

You will also need **Visual Studio Build Tools**, specifically the "Development with C++" workload. Install it from [this link](https://visualstudio.microsoft.com/visual-cpp-build-tools/).

To verify your installation, run the following commands:

```bash
nvcc --version
```

or

```bash
nvidia-smi
```

### Clone This Repository

Clone this repository using Git, or download it as a ZIP and extract it.

```bash
git clone <repository-url>
cd <repository-folder>
```

---

## Installation Steps

### 1. Install WSL 2

Follow the instructions [here](https://docs.microsoft.com/en-us/windows/wsl/install) to install WSL 2 on your Windows machine.

Alternatively, run one of the following commands to install a Linux distribution:

```bash
wsl --install -d Ubuntu-24.04
wsl --install -d Ubuntu-20.04
wsl --install -d Debian
```

### 2. Set WSL 2 as the Default Version

```bash
wsl --set-default-version 2
```

### 3. Create a Virtual Environment in WSL 2

A virtual environment is required to install Python packages.

```bash
sudo apt update
sudo apt install python3 python3-venv python3-pip
python3 -m venv my-env
```

### 4. Activate the Virtual Environment

```bash
source my-env/bin/activate
```

### 5. Install Required Packages

```bash
pip install -r requirements.txt
```

### 6. Open the Project in VS Code

Run the following command to install VS Code for WSL and open the project:

```bash
code .
```

*Note:* This installation does not automatically include all your VS Code extensions, but you can enable them in WSL.

### 7. Use the Virtual Environment in VS Code

When selecting a Python interpreter in VS Code, choose the one that matches your virtual environment’s name. It should look like this:

- ✅ `gpu-env (Python 3.11.2)`  **← Select this one**
- ❌ `Python 3.11.2 /bin/python3`
- ❌ `Python 3.11.2 /usr/bin/python3`

#### Installing `ipykernel`

When you first run Python code in VS Code, you may be prompted to install `ipykernel`. Install it with:

```bash
pip install ipykernel
```
