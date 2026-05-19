# Contributing to Robotics Lab

Thanks for your interest in contributing! This document outlines the process for contributing code, docs, simulations, or experiments.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [Development Workflow](#development-workflow)
- [Coding Standards](#coding-standards)
- [Pull Request Process](#pull-request-process)
- [Testing](#testing)
- [Documentation](#documentation)
- [Adding New Simulations](#adding-new-simulations)
- [Adding New Controllers](#adding-new-controllers)
- [Adding New Perception Modules](#adding-new-perception-modules)

---

## Code of Conduct

Be respectful, inclusive, and collaborative. Harassment of any kind will not be tolerated. Let's build awesome robotics tools together.

---

## Getting Started

### 1. Fork & Clone

```bash
git clone https://github.com/YOUR_USERNAME/robotics-lab.git
cd robotics-lab
git remote add upstream https://github.com/AtlasNexusTech/robotics-lab.git
```

### 2. Set Up Environment

```bash
python -m venv .venv
source .venv/bin/activate       # Linux/macOS
# .venv\Scripts\activate      # Windows

pip install -e ".[all]"
pip install -e ".[dev]"
pre-commit install
```

### 3. Verify

```bash
pytest
ruff check .
mypy .
```

---

## Development Workflow

1. **Create a branch** from `main`:
   ```bash
   git checkout -b feature/my-feature
   ```

2. **Make changes** following coding standards.

3. **Test locally** before pushing.

4. **Commit** with descriptive messages:
   ```
   feat(simulation): add quadruped walking gait
   fix(control): correct PID integral windup
   docs(readme): update installation instructions
   ```

   We follow [Conventional Commits](https://www.conventionalcommits.org/):
   - `feat:` — new feature
   - `fix:` — bug fix
   - `docs:` — documentation
   - `refactor:` — code restructuring
   - `test:` — adding/updating tests
   - `chore:` — maintenance tasks

5. **Push** and open a Pull Request.

---

## Coding Standards

### Python

- **Style**: [Black](https://black.readthedocs.io/) (line length 100)
- **Linting**: [Ruff](https://docs.astral.sh/ruff/)
- **Type hints**: Use for all public functions/methods
- **Docstrings**: [Google style](https://google.github.io/styleguide/pyguide.html#38-comments-and-docstrings)
- **Imports**: `isort` via Ruff (stdlib → third-party → local)

Example:

```python
def compute_inverse_kinematics(
    target_pose: np.ndarray,
    joint_limits: list[tuple[float, float]],
    max_iterations: int = 100,
) -> np.ndarray | None:
    """Compute IK solution for a robotic arm.

    Args:
        target_pose: 4x4 homogeneous transformation matrix.
        joint_limits: List of (min, max) tuples for each joint.
        max_iterations: Maximum solver iterations.

    Returns:
        Joint angle array if solution found, None otherwise.
    """
    ...
```

### Robotics-Specific

- URDF/Xacro files go in `worlds/` or `simulation/`
- Launch files (ROS 2) go in `simulation/launch/`
- Controller implementations go in `control/`
- All simulation worlds define a standard `reset()` / `step(action)` interface

---

## Directory Structure

```
robotics-lab/
├── simulation/          # Simulation environments (PyBullet, Gazebo, etc.)
│   ├── __init__.py
│   ├── envs/            # Gymnasium-compatible environments
│   ├── robots/          # Robot model loaders
│   └── utils/           # Simulation utilities
├── control/             # Control algorithms
│   ├── __init__.py
│   ├── pid/             # PID controllers
│   ├── mpc/             # Model Predictive Control
│   └── rl/              # RL policy deployment
├── perception/          # Perception & computer vision
│   ├── __init__.py
│   ├── detection/       # Object detection
│   ├── depth/           # Depth estimation
│   └── slam/            # SLAM modules
├── worlds/              # Robot & environment description files
│   ├── urdf/            # URDF robot models
│   ├── sdf/             # SDF world files
│   └── meshes/          # 3D mesh assets
├── config/              # Configuration files
├── docs/                # Documentation & tutorials
├── tests/               # Test suite
├── pyproject.toml       # Project configuration
├── CONTRIBUTING.md      # This file
└── README.md            # Project overview
```

---

## Pull Request Process

1. Ensure all tests pass: `pytest`
2. Ensure linting passes: `ruff check .`
3. Update documentation if APIs have changed
4. Add an entry to `CHANGELOG.md` (if applicable)
5. One approval from a maintainer required
6. Squash and merge preferred

---

## Testing

- Write tests for new functionality using `pytest`
- Simulation tests should run headless (`pybullet.DIRECT` or Gazebo headless)
- Use fixtures for robot/simulation setup
- Aim for meaningful coverage on control and perception modules

```bash
# Run all tests
pytest

# Run specific test file
pytest tests/test_pid_controller.py -v

# With coverage
pytest --cov=. --cov-report=html
```

---

## Documentation

- Module-level docstrings for all `__init__.py` files
- Tutorial-style notebooks in `docs/notebooks/`
- API documentation via docstrings
- Setup guides for each simulator in `docs/`

---

## Adding New Simulations

1. Create a new module under `simulation/envs/`
2. Implement the Gymnasium `Env` interface (`reset`, `step`, `render`, `close`)
3. Register the environment
4. Add configuration to `config/`
5. Add a tutorial notebook in `docs/`

---

## Adding New Controllers

1. Implement in `control/` with a clear base class or protocol
2. Provide `update(state, dt) -> action` interface where possible
3. Include unit tests with known dynamics
4. Benchmark against baselines

---

## Adding New Perception Modules

1. Implement in `perception/`
2. Process ROS 2 sensor messages or numpy arrays as input
3. Provide visualization utilities
4. Include example data and tests

---

## Questions?

Open a [GitHub Discussion](https://github.com/AtlasNexusTech/robotics-lab/discussions) or file an issue.

Happy building! 🤖
