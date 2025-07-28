# Research Code Production Conversion Project

## Project Overview
This project converts research code into a production-ready, containerized system using Docker and GNU Makefile.

## Current Code Structure
- **Location**: `/src/` (any project under src folder)
- **Format**: Research code in various formats (Python scripts, notebooks, etc.)
- **Dependencies**: Analyzed dynamically by Claude Code
- **Data Sources**: External datasets specific to each project

## Target Production Structure
```
project_name/
├── __init__.py
├── data/
│   ├── __init__.py
│   ├── preprocessing.py      # Data download and preprocessing
│   ├── datasets.py           # Dataset classes
│   └── loaders.py           # Data loading utilities
├── models/
│   ├── __init__.py
│   ├── architectures.py     # Model architectures
│   └── losses.py           # Custom loss functions
├── training/
│   ├── __init__.py
│   ├── trainer.py          # Training orchestration
│   └── evaluation.py      # Model evaluation
├── utils/
│   ├── __init__.py
│   └── metrics.py         # Performance metrics
└── config/
    ├── __init__.py
    └── settings.py        # Configuration management
```

## Key Commands for Development

### Testing
- Run unit tests: `make test`
- Run integration tests: `make integration-test`

### Linting & Type Checking
- Run linting: `make lint` (or `ruff check .` if using ruff)
- Run type checking: `make typecheck` (or `mypy project_name/` if using mypy)

### Data Pipeline
- Download datasets: `make download-data`
- Preprocess data: `make preprocess-data`
- Validate data integrity: `make validate-data`

### Model Training
- Train models: `make train`
- Full training pipeline: `make train-all`

### Docker Operations
- Build containers: `make build`
- Deploy services: `make deploy`
- Clean artifacts: `make clean`

## Development Workflow
1. Analyze existing code structure and dependencies
2. Refactor code into modular Python packages
3. Implement Docker containerization with multi-stage builds
4. Create comprehensive Makefile for automation
5. Add configuration management and testing
6. Implement logging, monitoring, and error handling

## Key Components Analysis
Claude Code will analyze the existing codebase to identify:
- **Data processing components**: Preprocessing, feature extraction, data loading
- **Model components**: Architecture definitions, training logic, evaluation
- **Utility functions**: Helper functions, metrics, common operations
- **Dependencies**: Required libraries and their versions

## Production Requirements
- Reproducibility with fixed seeds and version pinning
- Scalability with multi-GPU and distributed training support
- Robust error handling and recovery mechanisms
- Comprehensive logging and monitoring
- Data validation and caching strategies