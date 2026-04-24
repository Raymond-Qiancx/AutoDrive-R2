<div align="center">

# AutoDrive-R2

### Incentivizing Reasoning and Self-Reflection Capacity for VLA Models in Autonomous Driving

[![ICLR 2026](https://img.shields.io/badge/ICLR-2026-8A2BE2)](https://openreview.net/forum?id=KVWaCzJrrq)
[![arXiv](https://img.shields.io/badge/arXiv-2509.01944-b31b1b.svg)](https://arxiv.org/pdf/2509.01944)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

[Paper](https://arxiv.org/pdf/2509.01944) ·
[AutoDrive-R2-7B](https://huggingface.co/ZhenlongYuan/AutoDrive-R2-7B) ·
[AutoDrive-R2-7B-COT-SFT](https://huggingface.co/ZhenlongYuan/AutoDrive-R2-7B-COT-SFT) ·
[Dataset](https://huggingface.co/datasets/ZhenlongYuan/AutoDrive-R2-all-data/tree/main)

</div>

<p align="center">
  <img src="./images/1.png" width="92%" alt="AutoDrive-R2 overview">
</p>

## Overview

**AutoDrive-R2** is a Vision-Language-Action (VLA) model for autonomous driving trajectory prediction. It strengthens reasoning and self-reflection through a two-stage training recipe:

- **Chain-of-Thought SFT** on `nuScenesR²-6K`, a reasoning dataset with explicit logical chains and self-reflection.
- **Rule-based RL optimization** with physics-grounded rewards for spatial alignment, vehicle dynamics, and temporal smoothness.

Given a front-view camera image and historical ego-vehicle states, AutoDrive-R2 predicts six future waypoints at 0.5-second intervals for the next 3 seconds.

## Highlights

- **Reasoning-aware trajectory prediction** with explicit visual analysis, motion modeling, logical deduction, and self-reflection.
- **Qwen2.5-VL backbone** for strong multimodal understanding.
- **Physics-grounded GRPO training** with rule-based rewards tailored to driving trajectories.
- **nuScenes and Waymo support** for training and benchmark evaluation.

## Task Definition

| Item | Description |
| --- | --- |
| Input image | Front-view camera image |
| Historical states | Ego-vehicle position, velocity, acceleration, and steering angle from the previous 2.0-3.0 seconds |
| Prediction horizon | 3 seconds |
| Output | 6 future waypoints sampled every 0.5 seconds |
| Coordinate system | `x` is forward, `y` is leftward, measured in meters |

The model predicts:

```text
[(x0.5, y0.5), (x1.0, y1.0), ..., (x3.0, y3.0)]
```

## Method

<p align="center">
  <img src="./images/2.png" width="92%" alt="AutoDrive-R2 training pipeline">
</p>

AutoDrive-R2 follows a two-stage pipeline:

1. **CoT Supervised Fine-Tuning**
   - Builds `nuScenesR²-6K` with a four-step reasoning chain.
   - Teaches the model to analyze driving scenes and reflect on trajectory feasibility.

2. **Physics-Grounded Reinforcement Learning**
   - Optimizes with GRPO.
   - Uses rewards for spatial alignment, vehicle dynamics, and temporal smoothness.

## Data

### Training Data

| File | Samples | Description |
| --- | ---: | --- |
| `AData/sft.json` | ~60K | Raw SFT training data |
| `AData/sft_cot.json` | ~60K | CoT-annotated SFT data |
| `AData/rl.json` | ~60K | RL training data |

### Evaluation Data

| File | Samples | Benchmark |
| --- | ---: | --- |
| `AData/nuscenes_test.json` | ~54K | nuScenes |
| `AData/waymo_test.json` | ~12K | Waymo |

### Vehicle State Format

Each sample contains historical states at 0.5-second intervals:

- Position: `[x, y]`
- Velocity: `v`, in m/s
- Acceleration: `a_x`, `a_y`, in m/s²
- Steering angle: `theta`, where positive means left turn and negative means right turn

### Raw Dataset Layout

Download nuScenes from the [official website](https://www.nuscenes.org/download) and organize it as:

```text
nuscenes/
├── samples/
│   ├── CAM_BACK/
│   ├── CAM_BACK_LEFT/
│   ├── CAM_BACK_RIGHT/
│   ├── CAM_FRONT/
│   ├── CAM_FRONT_LEFT/
│   └── CAM_FRONT_RIGHT/
└── sweeps/
    ├── CAM_BACK/
    ├── CAM_BACK_LEFT/
    ├── CAM_BACK_RIGHT/
    ├── CAM_FRONT/
    ├── CAM_FRONT_LEFT/
    └── CAM_FRONT_RIGHT/
```

Waymo download instructions will be released later.

## Installation

```bash
git clone https://github.com/your-repo/AutoDrive-R2
cd AutoDrive-R2

conda create -n autodrive-r2 python=3.11
conda activate autodrive-r2

pip install -r requirements.txt
pip install flash-attn --no-build-isolation
```

## Training

### 1. Generate CoT Data

```bash
python AScripts/cot.py
```

### 2. Run Supervised Fine-Tuning

```bash
bash src/scripts/run_sft_video_7B_6k.sh
```

### 3. Run GRPO Reinforcement Learning

```bash
bash src/scripts/run_grpo_video_7B_6k.sh
```

## Evaluation

Evaluate on nuScenes:

```bash
python AScripts/eval_nuscene.py
```

Evaluate on Waymo:

```bash
python AScripts/eval_waymo.py
```

## Repository Structure

```text
.
├── AData/                 # Training and evaluation JSON files
├── AScripts/              # CoT generation and evaluation scripts
├── images/                # README figures
├── src/
│   ├── scripts/           # SFT and GRPO launch scripts
│   ├── r1-v/              # Training framework and model code
│   └── qwen-vl-utils/     # Qwen-VL utility package
├── requirements.txt
└── README.md
```

## Citation

If you find this work useful, please cite:

```bibtex
@inproceedings{
    yuan2026autodriver,
    title={AutoDrive-R{\texttwosuperior}: Incentivizing Reasoning and Self-Reflection Capacity for {VLA} Model in Autonomous Driving},
    author={Zhenlong Yuan and Chengxuan Qian and Jing Tang and Rui Chen and Zijian Song and Lei Sun and Xiangxiang Chu and Yujun Cai and Dapeng Zhang and Shuo Li},
    booktitle={The Fourteenth International Conference on Learning Representations},
    year={2026},
    url={https://openreview.net/forum?id=KVWaCzJrrq}
}
```

## Acknowledgements

This project builds on the following open-source projects and datasets:

- [Video-R1](https://github.com/tulerfeng/Video-R1)
- [lmm-r1](https://github.com/TideDra/lmm-r1)
- [DeepSeek-R1](https://github.com/deepseek-ai/DeepSeek-R1)
- [Qwen2.5-VL](https://huggingface.co/Qwen/Qwen2.5-VL-7B-Instruct)
- [nuScenes](https://www.nuscenes.org/)
- [Waymo Open Dataset](https://waymo.com/open/)
