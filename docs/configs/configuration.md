# Configuration Guide

## Overview

AutoDrive-R2 uses YAML configuration files to manage various settings for training, simulation, and evaluation.

## Configuration Structure

```yaml
# Example configuration structure
model:
  type: "ppo"
  learning_rate: 0.0003
  batch_size: 64

environment:
  name: "highway"
  max_steps: 1000

training:
  epochs: 100
  save_interval: 10
```

## Model Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `type` | Algorithm type (ppo, dqn, etc.) | ppo |
| `learning_rate` | Learning rate for optimizer | 0.0003 |
| `batch_size` | Training batch size | 64 |

## Environment Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `name` | Environment name | highway |
| `max_steps` | Maximum steps per episode | 1000 |
| `render` | Enable rendering | false |

## Training Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `epochs` | Number of training epochs | 100 |
| `save_interval` | Checkpoint save interval | 10 |
| `log_interval` | Logging interval | 1 |
