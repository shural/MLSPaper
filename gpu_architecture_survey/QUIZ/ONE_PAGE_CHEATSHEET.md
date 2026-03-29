# GPU Architecture Quiz - One-Page Cheatsheet

## THE BOTTLENECK CHAIN (MEMORIZE THIS!)
```
Fermi (2010): Memory BW → Volta (2017): GEMM → Turing (2018): Inference
     ↓                         ↓                        ↓
Ampere (2020): Training → Hopper (2022): Transformers → Blackwell (2024): Inference Scale
     ↓
Rubin (2026): ENERGY EFFICIENCY
```

## QUICK REFERENCE TABLE

| Gen | Year | Key Innovation | Precision | Mem BW | Compute | Bottleneck Solved |
|-----|------|----------------|-----------|--------|---------|-------------------|
| **Fermi** | 2010 | Cache, ECC | FP32/64 | 177 GB/s | 1.5 TF | HPC memory |
| **Volta** | 2017 | **Tensor Cores** | FP16/32 | 900 GB/s | 125 TF (TC) | GEMM throughput |
| **Turing** | 2018 | INT8, RT Cores | INT8/4 | 616 GB/s | 130 TOPS | Inference |
| **Ampere** | 2020 | Sparsity, TF32 | TF32/BF16 | 1.6 TB/s | 312 TF | Training speed |
| **Hopper** | 2022 | **FP8, Trans Eng** | **FP8** | 3.35 TB/s | 2000 TF | Transformers |
| **Blackwell** | 2024 | **FP4**, NVL72 | **FP4** | 8 TB/s | 4000+ TF | Inference cost |
| **Rubin** | 2026 | **3nm, HBM4, NVLink 6** | **NVFP4** | 22 TB/s | 50 PFLOPS | Energy efficiency |

## PRECISION EVOLUTION (halves every ~3 years)
FP32 (Fermi) → FP16 (Volta) → INT8 (Turing) → FP8 (Hopper) → FP4 (Blackwell) → NVFP4 (Rubin)

## TENSOR CORE GENERATIONS
- **Gen 1 (Volta)**: FP16×FP16→FP32, 4×4×4 MMA
- **Gen 2 (Turing)**: +INT8, +INT4
- **Gen 3 (Ampere)**: +TF32, +BF16, +FP64TC, +**Sparsity (2:4)**
- **Gen 4 (Hopper)**: +**FP8** (E4M3/E5M2), **Transformer Engine**
- **Gen 5 (Blackwell)**: +**FP4** (NVFP4), 2x attention (Ultra)
- **Gen 6 (Rubin)**: 50 PFLOPS NVFP4 inference, 35 PFLOPS training, 336B transistors

## KEY CONCEPTS

### TF32 (Ampere)
- FP32 range, FP16 precision (10-bit mantissa)
- **Automatic** 10x speedup for lazy FP32 code
- No code changes needed!

### Structured Sparsity (Ampere)
- **2:4 constraint**: Of every 4 values, exactly 2 are zero
- **2x speedup**: 312 → 624 TFLOPS
- <1% accuracy loss with proper training

### Transformer Engine (Hopper)
- Auto **FP8/FP16 switching** per layer
- Analyzes tensor statistics
- **9x training**, **30x inference** vs Ampere

### Thread Block Clusters (Hopper)
- Groups of thread blocks across **multiple SMs**
- Direct SM-to-SM communication
- 7x faster than global memory

### MIG - Multi-Instance GPU (Ampere)
- Partition into **7 independent instances**
- Each gets: SMs, memory, L2 cache
- 30% → 90% utilization in cloud

## MEMORY BANDWIDTH GROWTH
177 GB/s → 900 GB/s (5x) → 1.6/2.039 TB/s (1.8x) → 3.35 TB/s (1.7x) → 8 TB/s (2.4x) → 22 TB/s (2.75x)
**Pattern**: ~2x every 2-3 years (HBM2 → HBM2e → HBM3 → HBM3e → HBM4)

## DOMINANT COMPUTE UNIT
- **Fermi**: CUDA Core (scalar ALU)
- **Volta-Blackwell**: Tensor Core (matrix unit)
- **Future**: Model-specific primitives

## ARCHITECTURAL INVARIANTS (Never Change)
1. **SM-based** organization
2. **32 threads/warp** (SIMT)
3. **3-level memory**: Reg → Shared/L1 → L2 → DRAM
4. **Thread hierarchy**: Thread → Block → Grid

## SPECIALIZATION TREND
100% general (Fermi) → 20% (Volta) → 15% (Hopper) → 10% (Blackwell) → 5%? (Rubin)
**Trend**: GPUs becoming Neural Processing Units

## TOP 5 QUIZ QUESTIONS

**Q1: What bottleneck did [X] solve?**
- Follow the chain above!

**Q2: What precision does [X] support?**
- Fermi: FP32/64 | Volta: +FP16 | Turing: +INT8 | Ampere: +TF32/BF16 | Hopper: +FP8 | Blackwell: +FP4

**Q3: What's the Tensor Core generation?**
- Volta: Gen1 | Turing: Gen2 | Ampere: Gen3 | Hopper: Gen4 | Blackwell: Gen5

**Q4: How does [X] handle transformers?**
- Pre-Hopper: Generic GEMM
- Hopper+: **Transformer Engine** (specialized)

**Q5: What's the next bottleneck?**
- Current: Inference scale → Next: **ENERGY EFFICIENCY**

## FP8 FORMATS (Hopper)
- **E4M3**: 4-bit exp, 3-bit mantissa (high precision, limited range)
- **E5M2**: 5-bit exp, 2-bit mantissa (wide range, lower precision)
- Transformer Engine auto-selects per layer

## CRITICAL NUMBERS
- Fermi SMs: 16 | Volta: 80 | Ampere: 108 | Hopper: 132
- Threads/warp: **Always 32** (invariant!)
- Sparsity ratio: **2:4** (50% zeros, structured)
- MIG instances: Up to **7** (Ampere/Hopper)

## NVLINK EVOLUTION
- V2 (Volta): 300 GB/s
- V3 (Ampere): 600 GB/s (2x)
- V4 (Hopper): 900 GB/s (1.5x)
- V5 (Blackwell): 1.8 TB/s (2x)
- V6 (Rubin): 3.6 TB/s bidirectional per GPU (2x)

## WORKLOAD MAPPING
- **Fermi**: HPC (not ML)
- **Volta**: CNNs, early transformers (ResNet, BERT-Base)
- **Turing**: Edge inference (YOLO, DLSS)
- **Ampere**: Large transformers (GPT-3 175B)
- **Hopper**: Trillion-param models (GPT-4 scale)
- **Blackwell**: Agentic AI, reasoning (o1-style)
- **Rubin**: Massive-context inference (million-token), 10x lower token cost, sustainable AI

## MEMORY TRICKS
- **"VTATHABR"**: Volta, Turing, Ampere, Hopper, Blackwell, Rubin
- **Precision halves every 3 years**: 32→16→8→4 (NVFP4 continues in Rubin)
- **Memory 2x every 2-3 years**: 177→900→1600→3350→8000→22000
- **Each solution → next bottleneck**: Causality is key!

## COMMON MISTAKES TO AVOID
❌ TF32 is Turing (NO! It's **Ampere**)
❌ FP8 is Blackwell (NO! It's **Hopper**)
❌ Sparsity is 50% (NO! It's **2:4 structured**)
❌ Volta is for inference (NO! **Turing** is inference-focused)
❌ MIG is Volta (NO! It's **Ampere**)

---
**REMEMBER**: Understand the **causality** (why each bottleneck arose), not just facts!
