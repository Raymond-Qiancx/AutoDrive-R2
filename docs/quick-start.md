# Quick Start

## Installation

### Prerequisites

- Python 3.10+
- CUDA (for GPU acceleration)
- Git

### Clone the Repository

```bash
git clone https://github.com/cxqian/AutoDrive-R2.git
cd AutoDrive-R2
```

### Install Dependencies

```bash
pip install -r requirements.txt
```

## Basic Usage

### Running the Simulation

```bash
python run_simulation.py
```

### Training a Model

```bash
python train.py --config configs/default.yaml
```

### Evaluation

```bash
python evaluate.py --checkpoint checkpoints/model.pt
```

## Next Steps

- Read the [Configuration Guide](configs/configuration.md) to customize your setup
- Explore the [Environments](envs/environments.md) for different simulation scenarios
