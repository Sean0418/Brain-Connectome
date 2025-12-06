# Brain Connectome

[![Python 3.9+](https://img.shields.io/badge/python-3.9+-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Code style: ruff](https://img.shields.io/badge/code%20style-ruff-000000.svg)](https://github.com/astral-sh/ruff)

A Python package for analyzing brain structural and functional connectomes from the Human Connectome Project (HCP).

## Features

- **Data Loading**: Load and merge HCP structural/functional connectome data with traits
- **Dimensionality Reduction**: PCA and VAE for connectome feature extraction
- **Statistical Analysis**: Sexual dimorphism analysis with effect sizes and FDR correction
- **Machine Learning**: Random Forest and XGBoost classifiers for trait prediction
- **Visualization**: Publication-ready plots for connectome analysis

## Installation

```bash
# Clone the repository
git clone https://github.com/Sean0418/Brain-Connectome.git
cd Brain-Connectome

# Install in development mode with all dependencies
pip install -e ".[dev,docs]"

# Install pre-commit hooks
pre-commit install
```

## Quick Start

```python
from brain_connectome import ConnectomeDataLoader, DimorphismAnalysis
from brain_connectome.models import ConnectomeRandomForest

# Load data
loader = ConnectomeDataLoader("data/")
data = loader.load_merged_dataset()

# Analyze sexual dimorphism
analysis = DimorphismAnalysis(data)
features = [f"Struct_PC{i}" for i in range(1, 61)]
results = analysis.analyze(feature_columns=features)

# Get significant features
print(analysis.get_top_features(10))

# Train a classifier
X = data[features].values
y = (data["Gender"] == "M").astype(int).values

clf = ConnectomeRandomForest()
clf.fit(X, y, feature_names=features)
print(f"Top biomarkers:\n{clf.get_top_features(5)}")
```

## Project Structure

```
Brain-Connectome/
├── brain_connectome/           # Python package
│   ├── data/                   # Data loading and preprocessing
│   │   ├── loader.py          # ConnectomeDataLoader
│   │   └── preprocessing.py   # Preprocessing utilities
│   ├── analysis/              # Analysis modules
│   │   ├── pca.py            # PCA analysis
│   │   ├── vae.py            # VAE dimensionality reduction
│   │   └── dimorphism.py     # Sexual dimorphism analysis
│   ├── models/                # Machine learning models
│   │   └── classifiers.py    # RF and XGBoost classifiers
│   └── visualization/         # Plotting functions
│       └── plots.py          # Visualization utilities
├── tests/                     # Unit tests
├── docs/                      # Sphinx documentation
├── data/                      # Data directory (see below)
│   ├── raw/                   # Raw HCP data files
│   └── processed/             # Generated datasets
├── code/                      # Original R analysis (legacy)
└── pyproject.toml            # Package configuration
```

## Data

The package expects HCP data in the following structure:

```
data/
├── raw/
│   ├── SC/                    # Structural Connectome .mat files
│   ├── FC/                    # Functional Connectome .mat files
│   ├── TNPCA_Result/          # Tensor Network PCA coefficients
│   └── traits/                # Subject trait CSV files
└── processed/                 # Generated datasets
```

**Data Access**: Raw data must be downloaded from [ConnectomeDB](https://db.humanconnectome.org/) after agreeing to HCP data usage terms.

## Development

### Running Tests

```bash
pytest
```

### Linting

```bash
ruff check .
ruff format .
```

### Building Documentation

```bash
cd docs
make html
```

## Legacy R Code

The original R analysis is preserved in the `code/` directory. The `jasa-template` git tag marks the state before Python refactoring.

To checkout the original R version:
```bash
git checkout jasa-template
```

## Contributors

- Riley Harper
- Sean Shen
- Yinyu Yao

## License

MIT License - see LICENSE file for details.

## References

- Van Essen, D. C., et al. (2013). The WU-Minn Human Connectome Project: An overview. NeuroImage.
- Zhu, H., et al. (2019). Tensor Network Factorizations. NeuroImage.
