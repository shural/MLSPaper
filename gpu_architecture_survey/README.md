# GPU Architecture Evolution Survey
## From Fermi to Rubin: A Bottleneck-Driven Analysis

This repository contains a comprehensive academic survey examining NVIDIA GPU architecture evolution from 2010 to 2026, with focus on how machine learning workload pressure drives architectural innovation.

---

## 📁 Repository Structure

```
gpu_architecture_survey/
├── README.md                          # This file
├── QUIZ_STUDY_GUIDE.md               # Detailed study guide for quiz preparation
├── research/
│   └── architecture_analysis.md       # Comprehensive research notes
├── paper/
│   ├── gpu_architecture_survey.tex   # Main survey paper (LaTeX, ICML format)
│   ├── references.bib                 # Bibliography
│   └── (compiled PDF will go here)
└── figures/                           # Directory for figures (if needed)
```

---

## 📄 Documents

### 1. Main Survey Paper (`paper/gpu_architecture_survey.tex`)
- **Format**: ICML 2024 LaTeX template
- **Length**: ~8 pages (excluding references)
- **Content**: Academic survey covering 7 GPU generations
- **Structure**:
  - Introduction & Background
  - Fermi (2010) - HPC Foundation
  - Volta (2017) - Tensor Core Revolution
  - Turing (2018) - Inference & Ray Tracing
  - Ampere (2020) - Sparsity & TF32
  - Hopper (2022) - Transformer Engine
  - Blackwell (2024) - Inference at Scale
  - Rubin (2026) - Projected Evolution
  - Cross-Generational Analysis
  - Conclusions

### 2. Quiz Study Guide (`QUIZ_STUDY_GUIDE.md`)
- Quick reference tables
- Condensed answers to all 10 questions per architecture
- Key concepts to memorize
- Quiz tips and common mistakes
- Memory tricks and comparison tables
- **Perfect for last-minute review before Tuesday's quiz!**

### 3. Research Notes (`research/architecture_analysis.md`)
- Detailed analysis for each architecture
- Answers all 10 questions systematically
- Cross-generational trends
- Source material for the survey paper

---

## 🔧 Compiling the Paper

### Prerequisites
You need a LaTeX distribution installed:
- **macOS**: Install MacTeX (`brew install --cask mactex`)
- **Linux**: Install TeX Live (`sudo apt-get install texlive-full`)
- **Windows**: Install MiKTeX or TeX Live

### Option 1: Command Line Compilation

```bash
cd ~/gpu_architecture_survey/paper

# Compile with pdflatex (run twice for references)
pdflatex gpu_architecture_survey.tex
bibtex gpu_architecture_survey
pdflatex gpu_architecture_survey.tex
pdflatex gpu_architecture_survey.tex

# View the PDF
open gpu_architecture_survey.pdf  # macOS
# or
xdg-open gpu_architecture_survey.pdf  # Linux
```

### Option 2: Using Overleaf (Recommended for Easy Editing)

1. Go to [Overleaf.com](https://www.overleaf.com)
2. Create a new project → Upload Project
3. Upload `gpu_architecture_survey.tex` and `references.bib`
4. Set compiler to `pdfLaTeX`
5. Compile and download PDF

### Option 3: Local LaTeX Editor

Use any LaTeX editor:
- **TeXShop** (macOS)
- **TeXworks** (cross-platform)
- **VS Code** with LaTeX Workshop extension

---

## 📚 Study Materials for Quiz

### For Quick Review (30 minutes before quiz):
1. Read **QUIZ_STUDY_GUIDE.md** - especially the Quick Facts Table
2. Memorize the bottleneck chain
3. Review precision evolution (FP32 → FP16 → FP8 → FP4)
4. Know Tensor Core generations

### For Deep Understanding (2-3 hours):
1. Read the full survey paper PDF
2. Review research notes for detailed explanations
3. Focus on the 10 questions for each architecture
4. Understand the causal relationships (why each bottleneck led to the next solution)

---

## 🎯 The 10 Key Questions (Framework)

For each architecture generation, the survey answers:

1. **Workload Shift & Bottleneck**: What problem drove this design?
2. **Compute Unit**: What's the dominant computational primitive?
3. **Specialization Balance**: General-purpose vs. specialized acceleration?
4. **Memory Hierarchy**: How does data movement work?
5. **Parallelism Model**: How are threads scheduled?
6. **Numerical Precision**: What precisions are supported and why?
7. **Performance Bottleneck**: What's the limiting factor?
8. **Scaling Metrics**: Compute vs. memory vs. interconnect ratios?
9. **Workload Mapping**: How does it map to ML tasks?
10. **Invariants & Future**: What persists? What's the next bottleneck?

---

## 📊 Quick Architecture Comparison

| Architecture | Year | Key Innovation | Bottleneck Solved |
|--------------|------|----------------|-------------------|
| **Fermi** | 2010 | Cache hierarchy, ECC | HPC memory access |
| **Volta** | 2017 | Tensor Cores | Matrix multiply throughput |
| **Turing** | 2018 | INT8/RT Cores | Inference efficiency |
| **Ampere** | 2020 | Sparsity, TF32, MIG | Training throughput |
| **Hopper** | 2022 | Transformer Engine, FP8 | Transformer-specific ops |
| **Blackwell** | 2024 | FP4, dual-die | Inference scale/cost |
| **Rubin** | 2026 | (Projected) Energy-efficient | Power efficiency |

---

## 🔬 Research Sources

This survey synthesizes information from:
- NVIDIA Architecture Whitepapers (Fermi, Volta, Turing, Ampere, Hopper, Blackwell)
- NVIDIA Technical Blogs
- Academic papers on deep learning systems
- Industry announcements and technical documentation

---

## 📝 Notes for Quiz Preparation

### High-Probability Topics:
1. **Bottleneck-solution chain**: Understand the causal flow
2. **Precision evolution**: FP32 → FP16 → INT8 → TF32/BF16 → FP8 → FP4
3. **Tensor Core generations**: What each generation added
4. **Memory bandwidth scaling**: ~2x every 2-3 years
5. **Key innovations per architecture**: One sentence per generation

### Study Strategy:
- **3 days before**: Read full survey paper
- **1 day before**: Review study guide thoroughly
- **Morning of quiz**: Quick facts table + bottleneck chain
- **Don't memorize numbers**: Understand trends and relationships

### Common Exam Questions:
- "What bottleneck did [Architecture] solve?"
- "Compare Volta and Hopper Tensor Cores"
- "Why does precision decrease over generations?"
- "Explain structured sparsity in Ampere"
- "What's the Transformer Engine?"

---

## 🎓 Understanding the Narrative

The key insight: **Each architecture directly addresses the dominant bottleneck of its era while introducing new constraints.**

### The Causal Chain:
```
Fermi (2010): HPC bottleneck
    ↓ Cache hierarchy solves memory access
    ↓ But matrix multiplication becomes bottleneck with deep learning
Volta (2017): GEMM bottleneck
    ↓ Tensor Cores solve matrix throughput
    ↓ But memory capacity limits large models
Ampere (2020): Training throughput bottleneck
    ↓ Sparsity & TF32 accelerate training
    ↓ But transformer attention dominates time
Hopper (2022): Transformer bottleneck
    ↓ Transformer Engine optimizes attention
    ↓ But inference deployment cost is high
Blackwell (2024): Inference scale bottleneck
    ↓ FP4 reduces inference cost
    ↓ But energy efficiency becomes limiting
Rubin (2026): Energy bottleneck
    ↓ Novel device physics needed?
```

---

## 📬 Contact & Feedback

This survey was created for academic purposes. For questions or corrections, please refer to course materials or instructor.

---

## 📖 Citation

If referencing this work:

```
GPU Architecture Evolution Under ML Workload Pressure:
A Causal, Bottleneck-Driven Analysis from Fermi to Rubin
Survey Paper, 2026
```

---

## ✅ Quiz Preparation Checklist

- [ ] Read full survey paper (2-3 hours)
- [ ] Review study guide thoroughly (1 hour)
- [ ] Memorize bottleneck chain
- [ ] Understand precision evolution (FP32→FP16→FP8→FP4)
- [ ] Know Tensor Core evolution (Gens 1-5)
- [ ] Review memory bandwidth scaling
- [ ] Understand specialization trade-offs
- [ ] Can explain TF32, sparsity, Transformer Engine
- [ ] Know what MIG is (Multi-Instance GPU)
- [ ] Understand Thread Block Clusters (Hopper)
- [ ] Practice explaining causal relationships

---

## 🚀 Good Luck!

The quiz tests understanding, not memorization. Focus on:
1. **Causality**: Why did each bottleneck arise?
2. **Trade-offs**: What was sacrificed for specialization?
3. **Trends**: How do metrics scale over time?
4. **Workload mapping**: How does hardware match ML needs?

**Remember**: Each generation's architectural decisions directly respond to the workload pressures of its era. Understanding this causal relationship is key to mastering the material.

---

*Last updated: March 28, 2026*
