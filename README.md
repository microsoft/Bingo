# Not All Tokens Matter: Towards Efficient LLM Reasoning via Token Significance in Reinforcement Learning

<p align="center">
  <a href="https://arxiv.org/abs/2506.08125">
    <img src="https://img.shields.io/badge/arXiv-2506.08125-b31b1b.svg">
  </a>
</p>

<div align="center">
  <img width="80%" src="https://arxiv.org/html/2506.08125v1/x3.png">
</div>

## Introduction 📖

We propose **Bingo**, an RL framework that enhances efficient reasoning in LLMs through advanced length-based reward design. It incorporates two key mechanisms: a significance-aware length reward that gradually guides the model to reduce only insignificant tokens, and a dynamic length reward that initially encourages elaborate reasoning for hard questions but decays over time. This approach achieves a favorable trade-off between accuracy and efficiency, outperforming vanilla rewards and other length-based reward baselines.

---

Notice: This repository provides the core Bingo component implementations integrated with the Verl framework. These components must be properly configured within the Verl codebase to function correctly.

## Quick Start ⚡

### Step 1: Set Up Environment 🛠️

```bash
# Create conda environment
conda create -n bingo python==3.10
conda activate bingo

# Install torch (adjust CUDA version if needed)
pip install torch==2.4.0 --index-url https://download.pytorch.org/whl/cu124

# Flash attention (for efficient training)
pip install flash-attn --no-build-isolation

# vLLM inference engine
pip install vllm==0.8.3
```

### Step 2: Configure Verl ⚙️

Our implementation builds upon the [Verl](https://github.com/volcengine/verl) framework.

**Note: Our codebase cannot be executed directly. It first requires the installation of Verl, along with additions or modifications to the original Verl framework code. Please be aware that different releases of Verl may require different implementation changes.**

```shell
# 1. Clone Verl (our implementation is based on Verl v0.3.0post1, released on 04/04/2025)
git clone https://github.com/volcengine/verl

# 2. Apply our patches to Verl
# Manually update the following files as instructed
# e.g., modify the contents of verl/verl/trainer/ppo/ray_trainer.py
# by applying the changes provided in verl_patches/trainer/ppo/ray_trainer.py
verl_patches/trainer/ppo/ray_trainer.py
verl_patches/utils/reward_score/__init__.py
verl_patches/utils/reward_score/gsm8k.py

# 3. Install Verl
cd verl
pip install -e .
```

### Step 3: Download LLMLingua2 Tool 🔧

```bash
# Clone and set up LLMLingua for compression capabilities
git clone https://github.com/microsoft/LLMLingua.git LLMLingua_repo
mv LLMLingua_repo/llmlingua ./
rm -rf LLMLingua_repo
```

### Step 4: Prepare Datasets 📂

We train our model on the MATH dataset and evaluate on four test sets:
- **MATH500**: A 500-problem subset of the MATH test set (in-distribution benchmark)
- **GSM8K**: Out-of-distribution evaluation
- **AIME2024**: Out-of-distribution evaluation
- **TheoremQA**: Targeting symbolic STEM reasoning

```bash
# Download all datasets
python data_process/math_dataset.py --local_dir '/path/to/local/directory'
python data_process/math500.py --local_dir "/path/to/local/directory"
python data_process/gsm8k.py --local_dir "/path/to/local/directory"
python data_process/theoremqa.py --local_dir "/path/to/local/directory"
python data_process/aime.py --local_dir "/path/to/local/directory"
```

### Step 5: Training 🚀

1. **Start the Compressor Server**
```bash
python compressor_server.py
```

2. **Run Training Script**
```bash
sh train/run_bingo_all.sh
```


## Citation 📑

If you find this project helpful, please consider citing:

```bibtex
@article{liu2025bingo,
    title = {Not All Tokens Matter: Towards Efficient LLM Reasoning via Token Significance in Reinforcement Learning},
    author = {Liu, Hanbing and Cao, Lang and Ren, Yuanyi and Zhou, Mengyu and Dong, Haoyu and Ma, Xiaojun and Han, Shi and Zhang, Dongmei},
    journal = {arXiv preprint arXiv:2506.08125},
    year = {2025}
}
```

## Contributing

This project welcomes contributions and suggestions. Most contributions require you to agree to a Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us the rights to use your contribution. For details, visit https://cla.opensource.microsoft.com.

When you submit a pull request, a CLA bot will automatically determine whether you need to provide a CLA and decorate the PR appropriately (e.g., status check, comment). Simply follow the instructions provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/). For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.

## Trademarks

This project may contain trademarks or logos for projects, products, or services. Authorized use of Microsoft trademarks or logos is subject to and must follow [Microsoft's Trademark & Brand Guidelines](https://www.microsoft.com/en-us/legal/intellectualproperty/trademarks/usage/general). Use of Microsoft trademarks or logos in modified versions of this project must not cause confusion or imply Microsoft sponsorship. Any use of third-party trademarks or logos are subject to those third-party's policies.