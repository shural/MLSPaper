# Rubin Architecture Corrections - Official Specifications

**Date**: 2026-03-29
**Source**: NVIDIA official announcements (CES 2026, January 2026)

## INCORRECT Specifications (To Be Replaced)

❌ **Memory Bandwidth**: "10+ TB/s (est)" or "10+ TB/s"
❌ **Memory Capacity**: "256+ GB (est)"
❌ **Precision**: "FP2" / "binary" / "FP2/binary (est)"
❌ **Interconnect**: "Optical interconnect" / "optical NVLink 6"
❌ **Compute**: "8000+ TF (est)" or "~8000+ FP4"
❌ **Status**: "(Projected)" / "(est)" labels
❌ **Missing**: Transistor count, actual FLOPS numbers

## ✅ CORRECT Specifications (Official from NVIDIA)

### Core GPU Specifications
- **Year**: 2026 (CES announcement January 2026)
- **Process**: 3nm TSMC
- **Transistors**: **336 billion** (dual reticle design)
- **Memory**: HBM4
- **Memory Capacity**: **288 GB per GPU** (not 256+)
- **Memory Bandwidth**: **22 TB/s** (not 10+)

### Precision & Compute
- **Precision**: **NVFP4** (FP4 ONLY, NO FP2 or binary)
- **Inference**: **50 PFLOPS** NVFP4 (5x Blackwell)
- **Training**: **35 PFLOPS** NVFP4 (3.5x Blackwell)
- **NOT FP2**: No mention of FP2 or binary in any official sources

### Interconnect
- **NVLink 6**: **3.6 TB/s** bidirectional per GPU (NOT optical)
- **NOT optical**: No optical interconnect announced
- **Technology**: Standard electrical NVLink 6

### NVL72 Configuration
- **GPUs**: 72 Rubin GPUs
- **CPUs**: 36 Vera CPUs
- **Scale-up Bandwidth**: **260 TB/s** (total)
- **Total HBM4**: 20.7 TB
- **Total LPDDR5x**: 54 TB
- **Inference**: 3.6 EFLOPS NVFP4
- **Training**: 2.5 EFLOPS NVFP4

### Vera Rubin CPU-GPU Module
- **Configuration**: 1 Vera CPU + 2 Rubin GPUs per module
- **Purpose**: Integrated CPU-GPU compute

### Rubin CPX Variant (Context Processing)
- **Specialty**: Massive-context inference
- **Compute**: 30 PFLOPS NVFP4
- **Memory**: 128 GB GDDR7 (cost-efficient)
- **Use Case**: Million-token coding, generative video

### Performance Claims
- **Inference token cost**: 10x reduction vs Blackwell
- **MoE training**: 4x fewer GPUs needed vs Blackwell
- **Status**: Full production Q1 2026
- **Availability**: H2 2026 from partners

## Key Corrections Needed

### 1. Precision Evolution
**WRONG**: "FP32 → FP16 → FP8 → FP4 → FP2"
**RIGHT**: "FP32 → FP16 → FP8 → FP4 → (future unknown)"

Note: FP2/binary networks are theoretical research, NOT in Rubin

### 2. Memory Bandwidth Progression
**WRONG**: "Fermi 177 GB/s → ... → Blackwell 8 TB/s → Rubin 10+ TB/s"
**RIGHT**: "Fermi 177 GB/s → ... → Blackwell 8 TB/s → Rubin 22 TB/s"

### 3. Interconnect
**WRONG**: "Optical interconnect (10+ TB/s)"
**RIGHT**: "NVLink 6 (3.6 TB/s bidirectional per GPU)"

### 4. Compute Performance
**WRONG**: "~8000+ TFLOPS FP4 (est)"
**RIGHT**: "50 PFLOPS (50,000 TFLOPS) NVFP4 inference"

### 5. Status Labels
**WRONG**: "(Projected)", "(est)", "Future speculation"
**RIGHT**: Remove all speculation labels - Rubin is confirmed and in production

## Files Requiring Updates

### Quiz Materials (QUIZ/)
1. QUIZ_STUDY_GUIDE.md - Multiple tables and sections
2. SUPPLEMENTARY_QUESTIONS.md - Multiple tables
3. ONE_PAGE_CHEATSHEET.md - Quick reference table
4. ARCHITECTURE_CONCEPTS_REFERENCE.md - Reference tables

### Papers (paper/)
5. gpu_architecture_survey_optionA.tex
6. gpu_architecture_survey_optionB.tex

### Documentation
7. README.md - Architecture comparison table
8. Various docs/ files (if referenced)

## Replacement Patterns

### Pattern 1: Memory Bandwidth
```
OLD: 10+ TB/s (HBM4)
NEW: 22 TB/s (HBM4)

OLD: 10+ TB/s
NEW: 22 TB/s
```

### Pattern 2: Precision
```
OLD: FP2 (est)
NEW: NVFP4

OLD: FP2/binary
NEW: NVFP4

OLD: FP2? Binary?
NEW: NVFP4 (confirmed)
```

### Pattern 3: Interconnect
```
OLD: Optical interconnect (10+ TB/s)
NEW: NVLink 6 (3.6 TB/s per GPU)

OLD: optical NVLink 6
NEW: NVLink 6
```

### Pattern 4: Memory Capacity
```
OLD: 256+ GB (est)
NEW: 288 GB
```

### Pattern 5: Compute
```
OLD: ~8000+ FP4 (est)
NEW: 50 PFLOPS NVFP4 inference / 35 PFLOPS NVFP4 training

OLD: 8000+ TF
NEW: 50,000 TF (50 PFLOPS) NVFP4
```

### Pattern 6: Remove Speculation
```
OLD: (Projected)
NEW: [remove]

OLD: (est)
NEW: [remove]

OLD: Rubin (2026)
NEW: Rubin (2026) - keep year, it's correct
```

## Official Sources

1. **NVIDIA Newsroom**: [Rubin Platform Announcement](https://nvidianews.nvidia.com/news/rubin-platform-ai-supercomputer)
2. **NVIDIA Technical Blog**: [Inside the Rubin Platform](https://developer.nvidia.com/blog/inside-the-nvidia-rubin-platform-six-new-chips-one-ai-supercomputer/)
3. **VideoCardz**: [Vera Rubin NVL72 Details](https://videocardz.com/newz/nvidia-vera-rubin-nvl72-detailed-72-gpus-36-cpus-260-tb-s-scale-up-bandwidth)
4. **NVIDIA Investor Relations**: [Press Release](https://investor.nvidia.com/news/press-release-details/2026/NVIDIA-Kicks-Off-the-Next-Generation-of-AI-With-Rubin--Six-New-Chips-One-Incredible-AI-Supercomputer/)

## Update Priority

**HIGH PRIORITY** (User-facing documents):
1. QUIZ_STUDY_GUIDE.md
2. ONE_PAGE_CHEATSHEET.md
3. SUPPLEMENTARY_QUESTIONS.md
4. README.md
5. Paper files (.tex)

**MEDIUM PRIORITY** (Reference documents):
6. ARCHITECTURE_CONCEPTS_REFERENCE.md
7. OPTION_A_VS_B_COMPARISON.md

**LOW PRIORITY** (Historical/process docs):
8. Various validation and development docs

---

**Action Required**: Systematically update all files with correct Rubin specifications, removing all speculation and using confirmed numbers from official NVIDIA announcements.
