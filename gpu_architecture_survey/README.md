# GPU Architecture Evolution Survey
## From Fermi to Rubin: A Bottleneck-Driven Analysis

This repository contains a comprehensive academic survey examining NVIDIA GPU architecture evolution from 2010 to 2026, with focus on how machine learning workload pressure drives architectural innovation.

**Authors:**
- Heng Xiong (25300130028@m.fudan.edu.cn) - Fudan University, Shanghai, China
- Claude - Anthropic

---

## 📁 Repository Structure

```
gpu_architecture_survey/
├── README.md                          # This file
├── ORGANIZATION_PLAN.md               # Detailed organization guide
│
├── paper/                             # LaTeX Papers & Publications
│   ├── gpu_architecture_survey_optionA.tex   # Concise version (5 pages)
│   ├── gpu_architecture_survey_optionB.tex   # Comprehensive version (8 pages)
│   ├── gpu_architecture_survey_optionA.pdf   # Compiled PDF (Option A)
│   ├── gpu_architecture_survey_optionB.pdf   # Compiled PDF (Option B)
│   ├── gpu_survey_optionA_overleaf.zip       # Ready for Overleaf upload
│   ├── gpu_survey_optionB_overleaf.zip       # Ready for Overleaf upload
│   ├── references.bib                 # Bibliography database
│   ├── icml2026.sty                   # ICML 2026 style file
│   └── [other LaTeX support files]
│
├── QUIZ/                              # Quiz Preparation Materials
│   ├── QUIZ_STUDY_GUIDE.md            # Core study guide (17 questions)
│   ├── SUPPLEMENTARY_QUESTIONS.md     # Advanced topics (13 questions)
│   ├── COMPLETE_30_QUESTIONS.md       # All 30 questions consolidated
│   ├── COMPLETE_30_QUESTIONS_INDEX.md # Navigation guide
│   ├── ONE_PAGE_CHEATSHEET.md         # Quick reference for exam
│   └── QUESTION_COVERAGE_ANALYSIS.md  # Meta-analysis
│
├── validation/                        # Quality Assurance Reports
│   ├── FINAL_VALIDATION_10_10.md      # Final Gemini 10/10 validation
│   ├── FINAL_VALIDATION_SUMMARY.md    # Validation summary
│   ├── VALIDATION_REPORT.md           # Detailed validation report
│   └── VALIDATION_RESULTS.md          # Validation results
│
├── docs/                              # Process Documentation
│   ├── development/                   # Development logs
│   │   ├── SESSION_SUMMARY.md
│   │   ├── UPDATE_SUMMARY.md
│   │   ├── TABLE_OPTIMIZATION_SUMMARY.md
│   │   ├── TRIMMING_OPTIONS.md
│   │   └── TRIMMING_OPTIONS_V2.md
│   ├── comparison/                    # Analysis documents
│   │   └── OPTION_A_VS_B_COMPARISON.md
│   └── guides/                        # Usage guides
│       └── OVERLEAF_PACKAGES_README.md
│
├── research/                          # Research Materials
│   └── architecture_analysis.md       # Comprehensive research notes
│
└── figures/                           # Paper Figures (if needed)
```

---

## 📄 Documents Overview

### 1. Papers (`paper/`)

**Two versions available:**

#### Option A: Concise Version (5 pages + references)
- **File**: `gpu_architecture_survey_optionA.tex` / `.pdf`
- **Focus**: Key transitions and bottleneck evolution
- **Tables**: 3 (Volta, Blackwell, Bandwidth comparison)
- **Use case**: Quick overview, conference submission

#### Option B: Comprehensive Version (8 pages + references)
- **File**: `gpu_architecture_survey_optionB.tex` / `.pdf`
- **Focus**: Detailed architectural analysis
- **Tables**: 8 (covering all generations)
- **Use case**: Deep understanding, reference material

**Both papers cover:**
- Fermi (2010) - HPC Foundation
- Volta (2017) - Tensor Core Revolution
- Turing (2018) - Inference & Ray Tracing
- Ampere (2020) - Sparsity & TF32
- Hopper (2022) - Transformer Engine
- Blackwell (2024) - Inference at Scale
- Rubin (2026) - Projected Evolution

### 2. Quiz Materials (`QUIZ/`)

#### Primary Study Resources:
- **QUIZ_STUDY_GUIDE.md** - Covers 17 of 30 questions, core architecture evolution
- **SUPPLEMENTARY_QUESTIONS.md** - Covers 13 advanced questions
- **ONE_PAGE_CHEATSHEET.md** - Quick reference tables and memory tricks

#### Navigation & Reference:
- **COMPLETE_30_QUESTIONS_INDEX.md** - Maps all 30 questions to their locations
- **COMPLETE_30_QUESTIONS.md** - Consolidated view (all 30 in one file)
- **QUESTION_COVERAGE_ANALYSIS.md** - Meta-analysis of coverage

**Perfect for quiz preparation!**

### 3. Validation Reports (`validation/`)

Quality assurance documentation showing technical accuracy verification:
- **FINAL_VALIDATION_10_10.md** - Gemini validation achieving 10/10 accuracy
- **FINAL_VALIDATION_SUMMARY.md** - Summary of validation process
- **VALIDATION_REPORT.md** / **VALIDATION_RESULTS.md** - Detailed findings

### 4. Process Documentation (`docs/`)

Development history and analysis:
- **development/** - Session logs, update summaries, table optimizations
- **comparison/** - Option A vs Option B detailed comparison
- **guides/** - Overleaf upload instructions

---

## 🔧 Using the Papers

### Option 1: Download Pre-compiled PDFs

Simply open the PDF files in `paper/`:
- `gpu_architecture_survey_optionA.pdf` (concise)
- `gpu_architecture_survey_optionB.pdf` (comprehensive)

### Option 2: Upload to Overleaf (Recommended)

1. Go to [Overleaf.com](https://www.overleaf.com)
2. Click "New Project" → "Upload Project"
3. Select either:
   - `paper/gpu_survey_optionA_overleaf.zip` (concise version)
   - `paper/gpu_survey_optionB_overleaf.zip` (comprehensive version)
4. Overleaf will automatically compile
5. Edit and export as needed

See `docs/guides/OVERLEAF_PACKAGES_README.md` for detailed instructions.

### Option 3: Local LaTeX Compilation

```bash
cd gpu_architecture_survey/paper

# For Option A (concise):
pdflatex gpu_architecture_survey_optionA.tex
bibtex gpu_architecture_survey_optionA
pdflatex gpu_architecture_survey_optionA.tex
pdflatex gpu_architecture_survey_optionA.tex

# For Option B (comprehensive):
pdflatex gpu_architecture_survey_optionB.tex
bibtex gpu_architecture_survey_optionB
pdflatex gpu_architecture_survey_optionB.tex
pdflatex gpu_architecture_survey_optionB.tex
```

**Prerequisites:**
- **macOS**: MacTeX (`brew install --cask mactex`)
- **Linux**: TeX Live (`sudo apt-get install texlive-full`)
- **Windows**: MiKTeX or TeX Live

---

## 📚 Quiz Preparation Guide

### Quick Review (30 minutes before quiz):
1. Read **QUIZ/ONE_PAGE_CHEATSHEET.md** - bottleneck chain, key facts
2. Review **QUIZ/QUIZ_STUDY_GUIDE.md** - section summaries
3. Memorize precision evolution: FP32 → FP16 → INT8 → FP8 → FP4
4. Know Tensor Core generations (Gen 1-6)

### Deep Understanding (2-3 hours):
1. Read one of the survey papers (Option A or B)
2. Work through **QUIZ/QUIZ_STUDY_GUIDE.md** and **QUIZ/SUPPLEMENTARY_QUESTIONS.md**
3. Use **QUIZ/COMPLETE_30_QUESTIONS_INDEX.md** to navigate all 30 questions
4. Understand causal relationships (bottleneck → solution → new bottleneck)

### Study Strategy:
- **3 days before**: Read full survey paper (Option B for depth)
- **1 day before**: Review quiz study guide + supplementary questions
- **Morning of quiz**: ONE_PAGE_CHEATSHEET.md + bottleneck chain
- **Focus**: Understand trends and causality, not raw numbers

---

## 🎯 The 30 Key Questions

The survey and quiz materials answer 30 questions across 7 architectures:

**Core Questions (Q1-10):**
1. Workload shift & bottleneck resolved
2. Dominant unit of computation
3. General-purpose vs specialized balance
4. Memory hierarchy evolution
5. Data movement cost reduction
6. Data orchestration mechanisms
7. Parallel execution & scheduling
8. Latency hiding vs throughput maximization
9. Numerical precision evolution
10. Error tolerance model

**Evolution Questions (Q11-20):**
11-16. Architecture transitions and software stack evolution
17-20. Performance bottlenecks and scaling metrics

**Advanced Questions (Q21-30):**
21-27. Hardware-ML operation mapping, sparsity, future workloads
28-30. GPU to AI accelerator transition, next bottlenecks

See **QUIZ/COMPLETE_30_QUESTIONS_INDEX.md** for question-to-file mapping.

---

## 📊 Quick Architecture Comparison

| Architecture | Year | Key Innovation | Precision | Memory BW | Bottleneck Solved |
|--------------|------|----------------|-----------|-----------|-------------------|
| **Fermi** | 2010 | Cache, ECC | FP32/64 | 177 GB/s | HPC memory |
| **Volta** | 2017 | **Tensor Cores** | FP16/32 | 900 GB/s | GEMM throughput |
| **Turing** | 2018 | INT8, RT Cores | INT8/4 | 616 GB/s | Inference |
| **Ampere** | 2020 | Sparsity, TF32 | TF32/BF16 | 1.6 TB/s | Training speed |
| **Hopper** | 2022 | **FP8, Trans Eng** | **FP8** | 3.35 TB/s | Transformers |
| **Blackwell** | 2024 | **FP4**, NVL72 | **FP4** | 8 TB/s | Inference cost |
| **Rubin** | 2026 | 3nm, HBM4, NVLink 6 | **NVFP4** | 22 TB/s | Energy efficiency |

---

## 🔬 The Bottleneck Chain (Causal Flow)

**The key insight**: Each architecture directly addresses the dominant bottleneck of its era while introducing new constraints.

```
Fermi (2010): HPC memory bottleneck
    ↓ L1/L2 cache hierarchy solves random memory access
    ↓ But matrix multiplication becomes bottleneck with CNNs

Volta (2017): GEMM throughput bottleneck
    ↓ Tensor Cores deliver 125 TFLOPS mixed-precision
    ↓ But training large models is still slow

Ampere (2020): Training throughput bottleneck
    ↓ Sparsity (2x) + TF32 accelerate training
    ↓ But transformer attention dominates compute time

Hopper (2022): Transformer-specific bottleneck
    ↓ Transformer Engine + FP8 optimize attention
    ↓ But inference deployment cost is prohibitive

Blackwell (2024): Inference scale/cost bottleneck
    ↓ FP4 + NVL72 reduce inference cost/token
    ↓ But energy efficiency becomes limiting factor

Rubin (2026): Energy efficiency & inference cost bottleneck
    ↓ 3nm + HBM4 (22 TB/s) + 50 PFLOPS NVFP4 (5x Blackwell)
    ↓ 10x lower token cost, 336B transistors
    ↓ Next: ???
```

---

## 🎓 Understanding the Narrative

### Core Principle:
**Workload pressure drives architectural specialization**

### Evolution Pattern:
1. **Fermi era (2010-2016)**: General-purpose HPC computing
2. **Volta era (2017-2019)**: Matrix multiplication specialization (Tensor Cores)
3. **Hopper era (2022-2024)**: Transformer specialization (Transformer Engine)
4. **Future (2026+)**: Model-specific primitives? Energy efficiency?

### Specialization Trend:
- Fermi: 100% general-purpose CUDA cores
- Volta: ~20% specialized (Tensor Cores)
- Hopper: ~15% specialized (Transformer Engine)
- Blackwell: ~10% specialized (deeper integration)
- Rubin: ~5% general? (AI accelerator, not GPU?)

**Trade-off**: Specialization increases performance but reduces flexibility.

---

## 📬 Contact & Citation

**Authors:**
- Heng Xiong - 25300130028@m.fudan.edu.cn (Fudan University)
- Claude - Anthropic

**Citation:**
```
GPU Architecture Evolution Under ML Workload Pressure:
A Causal, Bottleneck-Driven Analysis from Fermi to Rubin

Heng Xiong, Claude
Survey Paper, 2026
```

---

## ✅ Quiz Preparation Checklist

**Understanding (most important):**
- [ ] Can explain the bottleneck chain from memory
- [ ] Understand why each architectural innovation happened
- [ ] Know the trade-offs of specialization
- [ ] Can map ML operations to hardware primitives

**Memorization (supporting facts):**
- [ ] Precision evolution (FP32→FP16→FP8→FP4→FP2)
- [ ] Tensor Core generations (Gen 1-6)
- [ ] Memory bandwidth scaling (~2x every 2-3 years)
- [ ] Key innovations per architecture (one sentence each)

**Materials to review:**
- [ ] Read survey paper (Option A or B)
- [ ] Review QUIZ_STUDY_GUIDE.md thoroughly
- [ ] Review SUPPLEMENTARY_QUESTIONS.md for advanced topics
- [ ] Use ONE_PAGE_CHEATSHEET.md for quick reference
- [ ] Practice explaining causal relationships out loud

**Common exam questions:**
- "What bottleneck did [Architecture] solve?"
- "Compare Volta and Hopper Tensor Cores"
- "Why does precision decrease over generations?"
- "Explain structured sparsity (2:4)"
- "What's the Transformer Engine?"

---

## 🚀 Final Tips

**The quiz tests understanding, not memorization.**

**Focus on:**
1. **Causality**: Why did each bottleneck arise? What solved it?
2. **Trade-offs**: What was sacrificed for specialization?
3. **Trends**: How do compute/memory/precision scale over time?
4. **Workload mapping**: How does hardware match ML operations?

**Remember**: Each generation's architectural decisions directly respond to the workload pressures of its era. Understanding this causal relationship—not memorizing numbers—is the key to mastering the material.

**Good luck! 🎓**

---

## 📖 Project Organization

For details on the repository structure and file organization, see **ORGANIZATION_PLAN.md**.

---

*Last updated: March 28, 2026*
*Repository organized for publication-ready structure*
