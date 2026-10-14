# FPHEAF: Fast Persistent Homology Extraction with Approximate Filtrations

FPHEAF is a Python research implementation for efficient topological feature extraction from electrocardiogram (ECG) signals. It combines time-delay (Takens) embedding, sparse neighbourhood construction, approximate filtration, and persistent homology to reduce computational cost while retaining useful topological structure.

The repository contains the benchmarking code, a Jupyter notebook, Excel result workbooks, and figures prepared for analysis and reporting.

## Key features

- **ECG time-series embedding:** Takens time-delay embedding.
- **Sparse distance construction:** k-nearest-neighbour (k-NN) neighbourhood sparsification.
- **Approximate persistent homology:** Efficient extraction of homology features, principally \(H_0\) and \(H_1\).
- **Benchmarking:** Runtime, memory consumption, topological fidelity, downstream classification, scalability, and streaming throughput.
- **Statistical analysis:** Paired comparisons, bootstrap confidence intervals, and Friedman/Nemenyi-style multiple-method comparisons where supported by the benchmark implementation.
- **Reporting:** Excel workbooks and figures for reviewing experimental results.

## Repository contents

```text
.
├── fpheaf_ecg_validation.ipynb       # End-to-end validation notebook
├── run_benchmarks.py                # Benchmarking and evaluation routines
├── run_ablation_experiments.py       # Ablation/sensitivity result table utility
├── generate_excel_report.py         # Excel report generation
├── build_notebook.py                # Notebook-generation script
├── Results_27Sept.xlsx               # Results workbook
├── Results_27Sept.xls                # Results workbook (legacy format)
├── Ultra_FPHEAF_Validation_Results.xlsx
├── Ultra_FPHEAF_Statistical_Results.xlsx
└── figures/
    ├── Figure1_FPHEAF_Workflow.jpg
    ├── Figure2_Runtime_Comparison.jpg
    ├── Figure3_Memory_Comparison.jpg
    ├── Figure4_Topological_Fidelity.jpg
    ├── Figure5_Feature_Preservation.jpg
    ├── Figure6_Classification_AUC.jpg
    ├── Figure7_Chapman_Scalability.jpg
    ├── Figure8_INCART_Throughput.jpg
    └── Figure9_Friedman_Nemenyi_CD.jpg
```

> **Note on filenames:** Some included workbook filenames retain earlier naming. The project is referred to as FPHEAF throughout this README.

## Datasets

The validation workflow is intended for the following ECG datasets:

- **PTB-XL** — clinical 12-lead ECG benchmark.
- **Chapman–Shaoxing ECG** — ECG recordings with rhythm annotations, used for evaluation and scalability analysis.
- **INCART** — annotated 12-lead arrhythmia recordings, used for streaming-oriented evaluation.

The ZIP package does **not** include the original clinical datasets. Obtain them from their official sources, review their terms of use, and configure the data paths in the code/notebook before running dataset-based experiments. Do not commit patient-level data or other restricted data to a public repository.

## Reported evaluation highlights

The supplied project materials report the following results for the proposed configuration. These are reported project results, not a guarantee that every value will reproduce on another machine or with a different dataset/preprocessing setup.

| Metric | Reported result |
|---|---:|
| Average processing time | 2.803 ms per ECG |
| Memory usage | 1.804 MB |
| Topological feature preservation | 99.640% |
| Downstream classification ROC-AUC | 0.978 |
| Reported speedup versus exact persistent homology | 1,741× |
| Chapman–Shaoxing 40,000-record processing estimate | 1.67 minutes |

**Interpretation:** Results depend on hardware, library versions, signal preprocessing, sampling strategy, and dataset configuration. The 40,000-record Chapman–Shaoxing figure is described in the supplied report as an estimate extrapolated from measurements up to 10,000 records; it should not be presented as a direct full-cohort measurement.

## Requirements

Use Python 3.10 or later (Python 3.11 is a reasonable starting point). Install the scientific Python packages used by the scripts and notebook:

```bash
python -m venv .venv
```

Activate the environment:

**Windows PowerShell**
```powershell
.\.venv\Scripts\Activate.ps1
```

**macOS / Linux**
```bash
source .venv/bin/activate
```

Install dependencies:

```bash
python -m pip install --upgrade pip
pip install numpy pandas scipy scikit-learn psutil matplotlib seaborn openpyxl jupyter ripser persim gudhi
```

Some optional TDA libraries may require platform-specific installation support. If installation of `gudhi` fails, consult its official installation instructions and use a compatible Python version.

## Quick start

1. Download or clone this repository.
2. Create and activate a Python environment, then install the dependencies above.
3. Download the datasets from their official sources and configure the input paths in the notebook and scripts.
4. Open the notebook:

   ```bash
   jupyter notebook fpheaf_ecg_validation.ipynb
   ```

5. Run the notebook cells in order, checking the dataset paths and experimental configuration first.

To run the benchmark script directly:

```bash
python run_benchmarks.py
```

To generate the Excel report:

```bash
python generate_excel_report.py
```

To print/export the ablation and neighbourhood-sensitivity table:

```bash
python run_ablation_experiments.py
```

### Important path configuration

The supplied Python scripts contain Windows-specific output paths under a local user directory. Before running them on another computer or in GitHub Codespaces/Colab, update those paths to a repository-relative output directory, for example:

```python
from pathlib import Path

OUTPUT_DIR = Path("results")
OUTPUT_DIR.mkdir(parents=True, exist_ok=True)
```

Apply the same change to output paths in `generate_excel_report.py` and `build_notebook.py` as needed. Ensure that the input data paths are configured separately.

## Reproducibility notes

For reproducible experiments:

- Record Python and package versions.
- Keep random seeds fixed where applicable.
- Document dataset versions, lead selection, sampling rate, preprocessing, and train/test splitting.
- Distinguish directly measured results from extrapolated estimates.
- Re-run the evaluation on the intended datasets before using the results in a publication.
- Treat the included Excel files and figures as existing outputs; they do not, by themselves, establish that all experiments can be reproduced without configuring the data and environment.

## Suggested citation

If you use this implementation in academic work, cite the associated paper or preprint for the FPHEAF method, once its final bibliographic details are available. Until then, cite this GitHub repository with its version or commit hash and access date.

## License

No license file was included in the supplied package. Add a `LICENSE` file before publishing if you want to specify how others may use, modify, or redistribute the code. Without an explicit license, standard copyright restrictions apply.

## Acknowledgements

This project explores the use of topological data analysis (TDA) and persistent homology for efficient ECG signal analysis.
