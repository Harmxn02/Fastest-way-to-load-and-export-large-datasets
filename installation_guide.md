# Installation guide

**NOTE**: You can just follow this guide: <https://docs.rapids.ai/install/#wsl2>. Contrary to the one below, it was written by the people who developed it, so they know what they're talking about. I personally went with the "WSL2 SDK Manager Install" and it took about 500 centuries to finish installing (but everything worked first try, it even created a new virtual environment for me). If you already have `conda` set up on WSL, you can also just use the "Release Selector" method.

*ps. If you opt for the "WSL2 SDK Manager Install" method, you can find the name of the conda environment by running the command `conda info --envs`, for me it was 'rapids-24.12', which you can then activate by typing `conda activate rapids-24.12`*

Steps 1, 2, 5, 6, and 7 still apply, but they are mentioned in the official guide as well.

---

## Prerequisites

### CUDA

Make sure you have CUDA Toolkit 11.1 installed on your machine. You can download it [here](https://developer.nvidia.com/cuda-11.1.1-download-archive).

You'll need Visual Studio Build Tools, in particular the "Development with C++" workload. You can install it from this link [here](https://visualstudio.microsoft.com/visual-cpp-build-tools/).

Check if everything is installed correctly by running the following command in your terminal

```bash
nvcc --version
```

or

```bash
nvidia-smi
```

### This repository

Start by cloning this repository, or download as ZIP and extract it.

## 1. Install WSL 2 (Windows Subsystem for Linux) on your Windows machine

You can follow the instructions [here](https://docs.microsoft.com/en-us/windows/wsl/install).

Basically, you need to enable WSL 2 feature on your Windows machine, and then install a Linux distribution from the Microsoft Store (or with this command: `wsl --install -d Ubuntu-24.04`, or this one `wsl --install -d Ubuntu-20.04`, or this one `wsl --install -d Debian`).

## 2. Set WSL version 2 as the default

```bash
wsl --set-default-version 2
```

## 3. Create a virtual environment inside WSL 2 (in this case we call it `my-env`)

**Why?** You need to create a virtual environment to be able to install pip-packages, I don't know why either but it's a thing.

```bash
sudo apt update
sudo apt install python3 python3-venv python3-pip
python3 -m venv my-env
```

## 4. activate the virtual environment

```bash
source my-env/bin/activate
```

## 5. Install the required packages

```bash
pip install -r requirements.txt
```

## 6. Open project in VS Code

```bash
code .
```

This will install VS Code for WSL and then open the project in VS Code. This installation does not contain all your extensions, but you can activate them in WSL.

## 7. Use the virtual environment

When selecting an interpreter, select `<your-environment-name>`, instead of Python version with a version number. The interpreter you need has the exact same name as the virtual environment, and will have a Python version number after it, in this format

- ✅ gpu-env (Python 3.11.2) <<< select this one
- ❌ Python 3.11.2 /bin/python3
- ❌ Python 3.11.2 /usr/bin/python3

When you first try running some code, it will ask you to install `ipykernel`, do this by running the following command

```bash
pip install ipykernel
```
