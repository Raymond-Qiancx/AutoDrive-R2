# Environments

## Overview

AutoDrive-R2 supports multiple simulation environments for autonomous driving research.

## Available Environments

### Highway Environment

A highway driving simulation with multiple lanes and traffic.

```python
from autodrive import make_env

env = make_env("highway")
obs = env.reset()
```

**Features:**
- Multiple lanes
- Dynamic traffic
- Lane changing scenarios

### Urban Environment

Complex urban driving scenarios with intersections and pedestrians.

```python
env = make_env("urban")
```

**Features:**
- Traffic lights
- Pedestrian crossings
- Complex intersections

### Parking Environment

Precision parking scenarios.

```python
env = make_env("parking")
```

**Features:**
- Parallel parking
- Reverse parking
- Tight spaces

## Creating Custom Environments

You can create custom environments by extending the base environment class:

```python
from autodrive.env.base import BaseEnv

class CustomEnv(BaseEnv):
    def __init__(self, config):
        super().__init__(config)
        # Custom initialization

    def step(self, action):
        # Implement step logic
        pass

    def reset(self):
        # Implement reset logic
        pass
```

## Environment Configuration

```yaml
environment:
  name: "highway"
  config:
    lanes: 4
    traffic_density: 0.3
    max_speed: 120
```
