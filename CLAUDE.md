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
│   ├── metrics.py         # Performance metrics
│   └── resume.py          # Pipeline resumption utilities
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
- Resume data pipeline: `make resume-data` (resumes from last successful step)

### Model Training
- Train models: `make train`
- Full training pipeline: `make train-all`
- Resume training: `make resume-train` (resumes from last checkpoint)
- Resume full pipeline: `make resume-all` (resumes entire data + training pipeline)

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
- **Pipeline Resumption**: Automatic checkpoint creation and resume capabilities for interrupted workflows

## Resumable Pipeline Features

### State Tracking
- Pipeline state files stored in `.pipeline_state/` directory
- Step completion status with timestamps and checksums
- Error logging with detailed failure information
- Automatic backup of intermediate results

### Resume Logic
- **Data Pipeline Resume**: Detects completed data processing steps (download, preprocessing, validation)
- **Training Resume**: Automatically finds latest model checkpoint and continues training
- **Full Pipeline Resume**: Seamlessly resumes from any interruption point in the complete workflow
- **Smart Recovery**: Validates existing artifacts before resuming to ensure data integrity

### Implementation Details
- State persistence using JSON format with atomic writes
- Configurable checkpoint intervals for long-running operations
- Graceful handling of SIGINT/SIGTERM signals for clean shutdowns
- Rollback capabilities for corrupted intermediate states