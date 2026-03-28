# Complete 30 Questions with Answers - GPU Architecture Survey

**Comprehensive Reference**: All 30 quiz questions with detailed answers  
**Last Updated**: 2026-03-28  
**Source**: Consolidated from QUIZ_STUDY_GUIDE.md + SUPPLEMENTARY_QUESTIONS.md

---

## 📖 Usage Guide

This document contains **all 30 quiz questions** with complete answers in a single file.

**Questions 1-4, 7, 9, 11-13, 17-20, 23-24, 28, 30**: From QUIZ_STUDY_GUIDE.md  
**Questions 5-6, 8, 10, 14-16, 21-22, 25-27, 29**: From SUPPLEMENTARY_QUESTIONS.md

---

## Question 1: Workload Shift & Bottleneck Resolved



**Fermi (2010)**
- **Shift**: Graphics → GPGPU/HPC
- **Bottleneck**: Random memory access, lack of caches
- **Solution**: L1/L2 cache hierarchy, ECC memory

**Volta (2017)**
- **Shift**: CNNs explosion (AlexNet→ResNet), early transformers
- **Bottleneck**: Matrix multiply throughput
- **Solution**: **Tensor Cores** (12x speedup for mixed-precision)

**Turing (2018)**
- **Shift**: Inference deployment, real-ti