# Research Code Production Conversion Project

## Project Overview
This project converts research code into production-ready, containerized systems using Docker and GNU Makefile. It provides a standardized framework for transforming experimental research code into robust, scalable, and reproducible production pipelines.

## Current Example: DD-PRiSM
- **Location**: `/dd_prism/` (Drug synergy prediction project - current example)
- **Format**: Research code converted to production-ready Python package
- **Dependencies**: PyTorch, RDKit, scikit-learn, pandas, numpy
- **Data Sources**: 6 public research datasets (NCI60, NCI-ALMANAC, CCLE, DepMap, MSigDB)

## Generalized Production Structure

The framework adapts to your research project's needs. Common components include:

### Core Structure (Always Present)
```
project_name/
├── __init__.py
├── config/
│   ├── __init__.py
│   └── settings.py          # Configuration management with dataclasses
├── scripts/                 # Automation and utility scripts
│   └── download_data.py     # Data acquisition (if needed)
└── datasets/                # Data storage (if applicable)
    ├── raw/                 # Raw downloaded/input data
    ├── processed/           # Preprocessed data
    └── input/              # Final model-ready data
```

### Optional Components (Based on Project Type)
```
project_name/
├── data/                    # Data processing modules (for data-heavy projects)
│   ├── __init__.py
│   ├── preprocessing.py     # Data preprocessing pipelines
│   ├── datasets.py         # Dataset classes and loaders
│   └── loaders.py          # Data loading utilities
├── models/                  # ML/AI model components (for ML projects)
│   ├── __init__.py
│   ├── architectures.py    # Model architectures
│   ├── losses.py          # Custom loss functions
│   └── inference.py       # Inference utilities
├── training/               # Training components (for ML projects)
│   ├── __init__.py
│   ├── trainer.py         # Training orchestration
│   └── evaluation.py     # Model evaluation and metrics
├── analysis/              # Analysis modules (for research analysis)
│   ├── __init__.py
│   ├── statistical.py    # Statistical analysis
│   ├── visualization.py  # Plotting and visualization
│   └── reporting.py      # Report generation
├── simulation/            # Simulation components (for computational research)
│   ├── __init__.py
│   ├── solvers.py        # Numerical solvers
│   └── physics.py        # Domain-specific computations
├── utils/                 # General utilities (as needed)
│   ├── __init__.py
│   ├── metrics.py        # Performance metrics
│   ├── io.py            # Input/output utilities
│   └── helpers.py       # General helper functions
└── tests/                # Testing (recommended for all projects)
    ├── unit/
    ├── integration/
    └── fixtures/
```

### Project Type Examples
- **ML/AI Projects**: Include `data/`, `models/`, `training/` modules
- **Data Analysis**: Include `data/`, `analysis/`, focus on `visualization.py` and `reporting.py`
- **Computational Research**: Include `simulation/`, `analysis/`, domain-specific modules
- **Pure Algorithm Research**: Focus on `utils/`, `analysis/`, minimal data components

## Adapting the Framework

### Step 1: Analyze Your Research Code
- Identify main computational components (models, analysis, simulation, etc.)
- Determine data requirements (external datasets, generated data, etc.)
- List dependencies and external tools

### Step 2: Choose Your Structure
Based on your project type, select appropriate modules:
```bash
# For ML projects
mkdir -p project_name/{data,models,training,utils,config}

# For data analysis projects  
mkdir -p project_name/{data,analysis,utils,config}

# For computational research
mkdir -p project_name/{simulation,analysis,utils,config}
```

### Step 3: Migrate and Refactor
- Move research code into appropriate modules
- Extract configuration into `config/settings.py` using dataclasses
- Create data download script if external data is needed
- Add proper Python package structure with `__init__.py` files

### Step 4: Configure Makefile
Adapt the Makefile variables to your project:
```makefile
PROJECT_NAME := your_project_name
DATA_DIR := your_project_name/datasets  # or remove if no data pipeline
```

Remove unused targets (e.g., remove `download-data` if no external datasets)

## Key Commands for Development

### Testing
- Run unit tests: `make test`
- Run integration tests: `make integration-test`

### Linting & Type Checking
- Run linting: `make lint` (uses ruff for fast Python linting)
- Run type checking: `make typecheck` (uses mypy for static type analysis)

### Data Pipeline (If Applicable)
- Download datasets: `make download-data` (skips existing files by default)
- Force download all: `make download-data-force` (overwrites existing files)
- Preprocess data: `make preprocess-data` 
- Validate data integrity: `make validate-data`
- Resume data pipeline: `make resume-data` (resumes from last successful step)

### Model Training (If Applicable)
- Train models: `make train`
- Full training pipeline: `make train-all`
- Resume training: `make resume-train` (resumes from last checkpoint)
- Resume full pipeline: `make resume-all` (resumes entire data + training pipeline)

### Docker Operations
- Build containers: `make build`
- Deploy services: `make deploy`
- Clean artifacts: `make clean`

## Development Workflow
1. **Analysis Phase**: Analyze existing research code structure and dependencies
2. **Refactoring Phase**: Convert research code into modular Python packages following standard structure
3. **Containerization Phase**: Implement Docker containerization with multi-stage builds for different environments
4. **Automation Phase**: Create comprehensive Makefile for pipeline automation and development workflows
5. **Configuration Phase**: Add robust configuration management with dataclasses and validation
6. **Testing Phase**: Implement unit tests, integration tests, and data validation
7. **Production Phase**: Add logging, monitoring, error handling, and resumable pipeline capabilities

## Data Download System Features
The framework includes a comprehensive automated data download system with the following capabilities:

### Core Features
- **Multi-source support**: Download from various research databases and public repositories
- **Automated extraction**: Handles ZIP archives including deflate64 compression using system unzip
- **Progress tracking**: Real-time download progress with file size information and completion status
- **Smart skipping**: Default behavior skips existing files to save time and bandwidth
- **Docker integration**: Runs inside containerized environment with proper permissions
- **Error handling**: Robust fallback mechanisms for various archive formats and network issues

### Configurable Data Sources
Each project defines its own data sources in the download script:
```python
DATA_SOURCES = {
    "dataset_name": {
        "url": "https://example.com/dataset.zip",
        "filename": "dataset.zip",
        "extracted": "dataset.csv",
        "description": "Dataset description"
    }
}
```

### Usage Examples (DD-PRiSM Implementation)
The current DD-PRiSM example downloads 6 datasets totaling 3.8GB:
- NCI60 single drug response data (2.4GB)
- NCI-ALMANAC combination drug data (615MB)
- Chemical structure data (710MB)
- Gene expression and pathway annotation files (35KB total)

This system can be adapted for any research project requiring automated dataset acquisition.

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
