# Layer Laboratory Rotation - CU Boulder

This repository contains computational projects completed during my rotation at the Layer Laboratory, CU Boulder.

## Repository Structure

This repository uses **Git submodules** to organize independent projects:

### 📊 Current Projects

#### 1. AlphaGenome Quick Start (`Alpha_genome_quickstart_notebook/`)
- **Repository:** [alphagenome-quickstart](https://github.com/gsstephenson/alphagenome-quickstart)
- **Description:** Comprehensive tutorial notebook for AlphaGenome API
- **Contents:** 
  - Interactive Jupyter notebook with all major AlphaGenome features
  - Sequence predictions, variant analysis, ISM, and visualization
  - Complete documentation and setup guide

#### 2. AlphaGenome vs MPRA Benchmark (`GSE84888_MPRA/`)
- **Repository:** [alphagenome-mpra-benchmark](https://github.com/gsstephenson/alphagenome-mpra-benchmark)
- **Description:** Systematic validation of AlphaGenome predictions against experimental MPRA data
- **Dataset:** GSE84888 (Fuentes et al., 2023) - Synthetic enhancer MPRA
- **Key Results:**
  - 18 sequences analyzed with 100% prediction success
  - Strong correlations (|r| = 0.6-0.7, p < 0.01)
  - Complete analysis pipeline with 13 publication-quality visualizations
- **Contents:**
  - Automated data preparation pipeline
  - AlphaGenome prediction scripts
  - Statistical benchmarking & visualization
  - Comprehensive documentation (README, results summary, quick reference)

---

## Quick Start

### Clone with Submodules

**Recommended method:**
```bash
git clone --recursive git@github.com:gsstephenson/Layer-Laboratory-Rotation.git
cd Layer-Laboratory-Rotation
```

**Alternative (if already cloned):**
```bash
git clone git@github.com:gsstephenson/Layer-Laboratory-Rotation.git
cd Layer-Laboratory-Rotation
git submodule init
git submodule update
```

### Setup Environment

```bash
# Create conda environment
conda create -n alphagenome-env python=3.11
conda activate alphagenome-env

# Install dependencies
pip install alphagenome python-dotenv matplotlib pandas jupyterlab ipython \
            scikit-learn scipy seaborn tqdm
```

### Configure API Key

Create `.env` file in `Alpha_genome_quickstart_notebook/`:
```bash
cd Alpha_genome_quickstart_notebook
echo "ALPHA_GENOME_API_KEY=your_key_here" > .env
```

---

## Project Highlights

### 🧬 AlphaGenome Quick Start
✅ Complete walkthrough of AlphaGenome capabilities  
✅ Production-ready code with secure API key handling  
✅ Extensive documentation and examples  

**Quick run:**
```bash
cd Alpha_genome_quickstart_notebook
jupyter lab quick_start.ipynb
```

### 📈 MPRA Benchmark Analysis
✅ Systematic validation against real experimental data  
✅ Statistical rigor (Pearson, Spearman, AUROC)  
✅ Automated pipeline (< 2 min runtime)  
✅ Publication-ready visualizations  

**Quick run:**
```bash
cd GSE84888_MPRA
conda activate alphagenome-env
python code/run_pipeline.py
```

**Key Finding:** AlphaGenome shows strong negative correlation with MPRA activity (r = -0.64, p < 0.01), suggesting systematic signal capture with inverted directionality—likely due to sequence context differences between synthetic constructs and genomic training data.

---

## Repository Organization

```
Layer-Laboratory-Rotation/
├── Alpha_genome_quickstart_notebook/    # Submodule: Tutorial & examples
│   ├── quick_start.ipynb                # Interactive notebook
│   ├── README.md                        # Setup & documentation
│   └── .env                             # API key (not committed)
│
├── GSE84888_MPRA/                       # Submodule: MPRA benchmark
│   ├── code/                            # Analysis scripts (5 files)
│   ├── data/                            # Input data (~84 MB)
│   ├── outputs/                         # Results & plots (30 files)
│   ├── README.md                        # Setup guide
│   ├── RESULTS_SUMMARY.md               # Detailed findings
│   ├── QUICK_REFERENCE.md               # Quick commands
│   └── FINAL_REPORT.md                  # Complete analysis
│
├── GIT_SUBMODULES_GUIDE.md              # Submodule management guide
├── .gitmodules                          # Submodule configuration
└── README.md                            # This file
```

---

## Working with Submodules

### Update a Submodule

When you make changes inside a submodule:

```bash
# 1. Make changes in the submodule
cd Alpha_genome_quickstart_notebook  # or GSE84888_MPRA
# ... edit files ...

# 2. Commit to the submodule
git add .
git commit -m "Description of changes"
git push origin main

# 3. Update main repository reference
cd ..
git add Alpha_genome_quickstart_notebook  # or GSE84888_MPRA
git commit -m "Update submodule reference"
git push origin main
```

### Update All Submodules to Latest

```bash
git submodule update --remote
git commit -am "Update all submodules to latest"
git push origin main
```

### Check Submodule Status

```bash
git submodule status
```

**Detailed guide:** See [`GIT_SUBMODULES_GUIDE.md`](GIT_SUBMODULES_GUIDE.md)

---

## Technologies & Tools

- **Language:** Python 3.11
- **Core Libraries:** AlphaGenome API, pandas, numpy, scipy, scikit-learn
- **Visualization:** matplotlib, seaborn
- **Environment:** Conda, Jupyter Lab
- **Version Control:** Git with submodules
- **Platform:** GitHub

---

## Results & Outputs

### AlphaGenome Quick Start
- ✅ Interactive tutorial notebook
- ✅ Working examples for all major features
- ✅ Comprehensive documentation

### MPRA Benchmark
- ✅ 31 output files (code, data, plots, docs)
- ✅ Strong statistical correlations (|r| ≈ 0.6-0.7)
- ✅ 13 publication-quality visualizations
- ✅ Automated reproducible pipeline

**View results online:**
- Quick Start: [alphagenome-quickstart](https://github.com/gsstephenson/alphagenome-quickstart)
- MPRA Benchmark: [alphagenome-mpra-benchmark](https://github.com/gsstephenson/alphagenome-mpra-benchmark)

---

## Documentation

Each submodule contains its own detailed documentation:

### Alpha_genome_quickstart_notebook/
- `README.md` - Setup, installation, usage guide
- Inline notebook documentation

### GSE84888_MPRA/
- `README.md` - Setup, pipeline overview, troubleshooting
- `RESULTS_SUMMARY.md` - Detailed analysis & interpretation
- `QUICK_REFERENCE.md` - Command cheat sheet
- `FINAL_REPORT.md` - Complete findings

---

## Citation

If you use this work, please cite:

**AlphaGenome:**
> Google DeepMind (2024) *AlphaGenome: Predicting functional genomic outputs from DNA sequences.* https://www.alphagenomedocs.com/

**MPRA Dataset:**
> Fuentes DR, et al. (2023) *Systematic perturbation of transcription factor binding sites reveals principles of enhancer design.* Nature Genetics. GEO: GSE84888.

---

## Contact

**Institution:** University of Colorado Boulder  
**Lab:** Layer Laboratory  
**GitHub:** [@gsstephenson](https://github.com/gsstephenson)

---

## License

This repository contains research code and analyses. Please see individual submodules for specific licensing information.

---

**Last Updated:** October 30, 2025  
**Status:** Active Development  
**Projects:** 2 (AlphaGenome Tutorial, MPRA Benchmark)
