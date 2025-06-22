
Paper: 94% on CIFAR-10 in 3.29 Seconds on a Single GPU

Link: https://ar5iv.labs.arxiv.org/html/2404.00498v2?authuser=0#:~:text=match%20at%20L119%20A100,2011

GitHub: https://github.com/KellerJordan/cifar10-airbench.git

Prior work

This project builds on the excellent previous record [https://github.com/tysam-code/hlb-CIFAR10](https://github.com/tysam-code/hlb-CIFAR10) (6.3 A100-seconds to 94%).

Which itself builds on the amazing series [https://myrtle.ai/learn/how-to-train-your-resnet/](https://myrtle.ai/learn/how-to-train-your-resnet/) (26 V100-seconds to 94%, which is >=8 A100-seconds)



AirBench MuonNet — CIFAR-10 & CIFAR-100 Fast CNN Training
Research Reproduction + Dataset Generalization
Based on the paper: 94% on CIFAR-10 in 3.29 Seconds on a Single GPU (2024)
Original Code: KellerJordan/cifar10-airbench

📌 Overview
This repository reproduces and extends the methodology proposed in the 2024 paper "94% on CIFAR-10 in 3.29 Seconds on a Single GPU". The original work focuses on extremely fast CNN training for CIFAR-10 using a lightweight, VGG-style network with novel optimization techniques. We:

✅ Reproduce the CIFAR-10 results with airbench94_muon.py

✅ Extend the method to CIFAR-100 with airbench_muon_cifar100.py

✅ Apply training enhancements, architectural adjustments, and optimizer tuning for fine-grained classification.

🔬 Project Structure
bash
Copy
Edit
.
├── airbench94_muon.py              # Original CIFAR-10 reproduction
├── airbench_muon_cifar100.py       # Modified implementation for CIFAR-100
├── logs/                           # Saved accuracy and metadata logs
├── datasets/
│   └── cifar10, cifar100/          # .pt files and dataset folders
├── README.md                       # This file
└── log.pt                          # Final results serialized for analysis
🚀 Phase I: Paper Reproduction (CIFAR-10)
Objective: Reproduce the fast CNN training pipeline achieving 94% accuracy in ~3 seconds.

Core Features:

VGG-style CNN with identity-initialized layers

Patch whitening using eigenvectors of 2×2 image patches

Alternating flip augmentation

Muon optimizer for fast convergence

Label smoothing & TTA (test-time augmentation)

📈 Performance:

Mean Accuracy: 94.00% ± 0.13%

Time per Run: ~20s (on T4)

Total Runs: 200

🔁 Phase II: Extension to CIFAR-100
New Dataset: Switched from CIFAR-10 to CIFAR-100 (100 classes)

Challenges:

Higher intra-class similarity

More fine-grained feature learning required

🔧 Key Modifications:
Component	CIFAR-10	CIFAR-100 (Modified)
Output Layer	nn.Linear(..., 10)	nn.Linear(..., 100)
Channels (blocks)	64 → 256 → 256	96 → 384 → 384
Label Smoothing	0.2	0.25
Batch Size	2000	1500
Weight Decay	2e-6 × batch_size	5e-6 × batch_size
Learning Rate	0.67	0.85 (head), 0.32 (Muon)
Epochs	8	12

📊 Performance on CIFAR-100:

Mean TTA Accuracy: 71.68% ± 0.28%

Best Accuracy: 72.49%

Time per Run: ~56s (Tesla T4)

Total Runs: 50

🛠️ How to Run
✅ Requirements
Python 3.8+

PyTorch ≥ 2.0 with torch.compile support

GPU (A100/T4 recommended for speed)

📦 Setup
bash
Copy
Edit
# Clone this repo
git clone https://github.com/your-username/airbench-cnn-cifar
cd airbench-cnn-cifar

# Install dependencies (use your preferred environment)
pip install torch torchvision
🚀 Run CIFAR-10 Reproduction
bash
Copy
Edit
python airbench94_muon.py
🔁 Run CIFAR-100 Extension
bash
Copy
Edit
python airbench_muon_cifar100.py
Each script will automatically:

Download and preprocess the dataset

Run model training for multiple trials

Save logs to logs/ and print summary statistics

📊 Results Summary
Dataset	Accuracy (TTA)	Std Dev	Time/Run	Epochs	Runs
CIFAR-10	94.00%	±0.13%	~20 sec	8	200
CIFAR-100	71.68%	±0.28%	~56 sec	12	50
