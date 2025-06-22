# 🧠 AirBench MuonNet — CIFAR-10 & CIFAR-100 Fast CNN Training

> **Research Reproduction & Extension Project**  
> Based on the paper: [94% on CIFAR-10 in 3.29 Seconds on a Single GPU (2024)](https://ar5iv.labs.arxiv.org/html/2404.00498v2)  
> Original Code: [KellerJordan/cifar10-airbench](https://github.com/KellerJordan/cifar10-airbench)

---

## 📌 Overview

This project reproduces the CNN training method proposed in the 2024 paper on CIFAR-10 and extends it to CIFAR-100. It includes:

- ✅ Full reproduction of CIFAR-10 training (94% in ~3s)
- ✅ CIFAR-100 adaptation with wider layers and tuning
- ✅ Training pipeline, test-time augmentation, whitening, and Muon optimizer

---

## 📂 Repository Structure

```
.
├── airbench94_muon.py              # Original CIFAR-10 implementation
├── airbench_muon_cifar100.py       # Modified version for CIFAR-100
├── logs/                           # Log files with saved metrics
├── datasets/                       # Folder for CIFAR datasets
├── README.md                       # This file
└── log.pt                          # Accuracy results saved
```

---

## 🔬 Phase I: Reproduction on CIFAR-10

- **Architecture**: VGG-like 7-layer CNN (GELU, MaxPool, BatchNorm)
- **Optimizer**: Muon (custom, whitening-based)
- **Augmentation**: Alternating Flip + Random Crop + Reflection Padding
- **Compile Mode**: `torch.compile(mode='max-autotune')`
- **Training Time**: ~20 seconds per run on NVIDIA T4
- **Accuracy**: `94.00% ± 0.13%` (200 trials)

---

## 🔁 Phase II: Extension to CIFAR-100

| Feature              | CIFAR-10                    | CIFAR-100                       |
|----------------------|-----------------------------|----------------------------------|
| Output Classes        | 10                          | 100                              |
| Network Width         | 64 → 256 → 256              | 96 → 384 → 384                   |
| Label Smoothing       | 0.2                         | 0.25                             |
| Batch Size            | 2000                        | 1500                             |
| Learning Rate         | 0.67 (head)                 | 0.85 (head), 0.32 (muon)         |
| Weight Decay          | 2e-6 × batch_size           | 5e-6 × batch_size                |
| Epochs                | 8                           | 12                               |
| Accuracy (TTA)        | 94.00%                      | 71.68% ± 0.28%                   |
| Time per Run          | ~20 sec                     | ~56 sec                          |

---

## 🛠️ Setup Instructions

### ✅ Requirements

- Python ≥ 3.8
- PyTorch ≥ 2.0 with `torch.compile`
- CUDA-enabled GPU (e.g., T4, A100)

### ⚙️ Install Dependencies

```bash
pip install torch torchvision
```

### 📦 Run CIFAR-10 Reproduction

```bash
python airbench94_muon.py
```

### 📦 Run CIFAR-100 Extension

```bash
python airbench_muon_cifar100.py
```

> Note: Logs will be saved automatically in the `logs/` directory.

---

## 📈 Results Summary

| Metric               | CIFAR-10        | CIFAR-100       |
|----------------------|------------------|------------------|
| Mean Accuracy (TTA)  | 94.00%           | 71.68%           |
| Std Dev              | ±0.13%           | ±0.28%           |
| Best Accuracy        | 94.13%           | 72.49%           |
| Training Time/Run    | ~20 sec          | ~56 sec          |
| Epochs               | 8                | 12               |
| Trials               | 200              | 50               |

---

## 🧠 Technical Highlights

- ✅ Patch Whitening Initialization via SVD of 2x2 image patches
- ✅ Custom Muon Optimizer (whitened, normalized updates)
- ✅ Alternating Flip: Deterministic data augmentation
- ✅ Torch Compile for acceleration (`torch.compile`)
- ✅ Test-Time Augmentation (TTA): Flip + Crop + Average

---

## 📊 CIFAR-100 Complexity vs CIFAR-10

- CIFAR-10: Broad classes (e.g., cat, truck)
- CIFAR-100: Fine-grained classes (e.g., apple vs. pear)
- More intra-class similarity → Requires deeper, wider models

---

## 📁 Dataset

- CIFAR-10 and CIFAR-100 automatically downloaded and saved as `.pt` files in:
  ```
  datasets/cifar10/
  datasets/cifar100/
  ```

---

## 📚 References

- [📄 Paper on arXiv](https://ar5iv.labs.arxiv.org/html/2404.00498v2)
- [💻 Original GitHub Repo](https://github.com/KellerJordan/cifar10-airbench)
- [📊 CIFAR Dataset Info](https://www.cs.toronto.edu/~kriz/cifar.html)

---

## 🙋‍♂️ Author

> Developed as part of a semester project (Spring 2025)  
> 📧 Contact: [your-email@example.com]  
> 🔗 LinkedIn/GitHub: [your-profile]

---

## 📝 License

This repository builds upon open-source research (MIT Licensed). See [LICENSE](LICENSE) for more details.
