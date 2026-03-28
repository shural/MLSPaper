# GPU Architecture Survey - Quiz Study Guide
## Quick Reference for Tuesday's Quiz

---

## ⚡ QUICK FACTS TABLE

| Architecture | Year | Main Innovation | Precision | Memory BW | Bottleneck Solved | Next Bottleneck |
|--------------|------|-----------------|-----------|-----------|-------------------|-----------------|
| **Fermi** | 2010 | Cache hierarchy, ECC | FP32/FP64 | 177 GB/s | HPC reliability | Matrix multiply throughput |
| **Volta** | 2017 | **Tensor Cores (1st gen)** | FP16/FP32 | 900 GB/s | GEMM throughput | Memory capacity |
| **Turing** | 2018 | INT8/INT4 Tensor Cores, RT Cores | INT8/INT4/FP16 | 616 GB/s | Inference efficiency | Training throughput |
| **Ampere** | 2020 | Sparsity, TF32, MIG | TF32/BF16/FP64TC | 1.6/2.039 TB/s | Training speed, multi-tenancy | Transformer attention |
| **Hopper** | 2022 | **Transformer Engine**, FP8 | **FP8** (E4M3/E5M2) | 3.35 TB/s | Transformer-specific | Inference scale |
| **Blackwell** | 2024 | **FP4**, dual-die, NVL72 | **FP4**/FP6/FP8 | 8 TB/s | Inference capacity/cost | Energy efficiency |
| **Rubin** | 2026 | (Projected) Sub-FP4?, energy-efficient | FP2? Binary? | 10+ TB/s | Energy/power | Physical limits |

---

## 📝 10 QUESTIONS - CONDENSED ANSWERS

### Question 1: Workload Shift & Bottleneck Resolved

**Fermi (2010)**
- **Shift**: Graphics → GPGPU/HPC
- **Bottleneck**: Random memory access, lack of caches
- **Solution**: L1/L2 cache hierarchy, ECC memory

**Volta (2017)**
- **Shift**: CNNs explosion (AlexNet→ResNet), early transformers
- **Bottleneck**: Matrix multiply throughput
- **Solution**: **Tensor Cores** (12x speedup for mixed-precision)

**Turing (2018)**
- **Shift**: Inference deployment, real-time ray tracing
- **Bottleneck**: Inference latency, quantization support
- **Solution**: INT8/INT4 Tensor Cores, RT Cores

**Ampere (2020)**
- **Shift**: GPT-3 era (100B+ params), trillion-param models
- **Bottleneck**: Training throughput, sparsity, cloud efficiency
- **Solution**: 2:4 structured sparsity (2x), TF32 (10x lazy speedup), MIG

**Hopper (2022)**
- **Shift**: Trillion-parameter transformers (GPT-4, PaLM)
- **Bottleneck**: Transformer attention dominance (40-60% of time)
- **Solution**: **Transformer Engine** (FP8 + auto-switching), Thread Block Clusters

**Blackwell (2024)**
- **Shift**: Multi-trillion params, inference at scale
- **Bottleneck**: Inference cost/throughput, long-context
- **Solution**: FP4 precision, dual-die design, NVL72 (72 GPU rack)

**Rubin (2026)**
- **Shift**: 10T+ params, multimodal, reasoning models
- **Bottleneck**: **Energy efficiency** (data center power limits)
- **Solution**: (Projected) Novel device physics, optical interconnects

---

### Question 2: Dominant Unit of Computation

**Fermi**: CUDA Core (scalar FP32/FP64 ALU)
- 512 cores total (16 SMs × 32 cores/SM)
- FP64 at 1/2 FP32 rate

**Volta**: **Tensor Core** (becomes dominant)
- 8 per SM, 640 total (80 SMs)
- 4×4×4 matrix multiply-accumulate
- FP16 input, FP32 accumulation

**Turing**: Tensor Core (Gen 2) + RT Core
- INT8/INT4 support added
- RT Cores for ray tracing

**Ampere**: Tensor Core (Gen 3)
- **TF32**, BF16, FP64 Tensor Core
- **Sparsity engine** (2:4 structured)

**Hopper**: Tensor Core (Gen 4) + **Transformer Engine**
- **FP8** (E4M3 and E5M2)
- 2x throughput per SM vs Ampere
- DPX instructions for dynamic programming

**Blackwell**: Tensor Core (Gen 5 / Ultra)
- **FP4** (NVFP4 with micro-tensor scaling)
- 2x attention acceleration (Ultra)
- 208B transistors (dual-die)

**Rubin**: (Projected) Tensor Core Gen 6
- FP2/binary? Specialized attention units?

---

### Question 3: General-Purpose vs Specialized Balance

| Architecture | General % | Specialized % | Notes |
|--------------|-----------|---------------|-------|
| Fermi | 100% | 0% | Pure CUDA cores |
| Volta | ~20% | ~80% | Tensor Cores dominate |
| Turing | ~30% | ~70% | TC + RT Cores |
| Ampere | ~30% | ~70% | TC handles FP64 too |
| Hopper | ~15% | ~85% | Transformer Engine |
| Blackwell | ~10% | ~90% | FP4, highly specialized |
| Rubin | ~5%? | ~95%? | Trend continues |

**Trade-off**: Flexibility vs performance. Tensor Cores idle for non-matrix ops but deliver 8-12x speedup for ML.

---

### Question 4: Memory Hierarchy Evolution

**Fermi (2010)**
- L1: 16/48 KB (configurable with shared memory)
- L2: 768 KB
- GDDR5: 177 GB/s
- **Innovation**: First true cache hierarchy

**Volta (2017)**
- L1/Shared: 128 KB per SM
- L2: 6 MB (8x Fermi)
- **HBM2**: 900 GB/s (5x Fermi) ← Major leap
- Capacity: 16/32 GB

**Ampere (2020)**
- L1/Shared: 192 KB per SM
- **L2: 40 MB** (6.7x Volta!) ← Massive cache
- HBM2e: 1.6 TB/s, 40/80 GB
- Compute data compression

**Hopper (2022)**
- L1/Shared: 256 KB per SM
- L2: 50 MB
- **HBM3**: 3.35 TB/s, 80 GB (first HBM3 GPU)
- **TMA** (Tensor Memory Accelerator)

**Blackwell (2024)**
- **HBM3e**: 8 TB/s, 192 GB (2.4x Hopper)
- Decompression engine (LZ4, Snappy)
- Grace-Blackwell: 900 GB/s CPU-GPU link

**Bottlenecks addressed**:
- Fermi: Random access patterns
- Volta: Bandwidth for large models
- Ampere: Capacity for GPT-3 scale
- Hopper: Multi-trillion parameter models
- Blackwell: Long-context (100K+ tokens), MoE

---

### Question 5: Parallel Execution & Scheduling

**Fermi**: Dual warp scheduler, GigaThread engine
- 48 warps/SM max (1536 threads)
- Concurrent kernel execution (16 kernels)

**Volta**: **Independent thread scheduling**
- Threads diverge/reconverge at sub-warp level
- 4 warp schedulers per SM

**Ampere**: Asynchronous execution
- Async copy (global→shared without registers)
- Task graphs (hardware kernel launch)
- Async barriers

**Hopper**: **Thread Block Clusters** ← Revolutionary
- Groups of thread blocks across multiple SMs
- **Distributed shared memory** (SM-to-SM direct)
- 7x faster than global memory

**Blackwell**: NVL72 architecture
- 72 GPUs as single programming unit
- Rack-scale computation

---

### Question 6: Numerical Precision Evolution

**Precision Timeline**:
```
FP32 → FP16 → INT8 → TF32/BF16 → FP8 → FP4 → FP2?
```

**Fermi**: FP32, FP64 only
- IEEE 754-2008 compliant
- HPC requires full precision

**Volta**: **FP16/FP32 mixed precision**
- FP16 inputs, FP32 accumulation
- Loss scaling prevents underflow
- Justification: DL gradients fit in FP16 range

**Turing**: **INT8/INT4**
- Post-training quantization (PTQ)
- <1% accuracy loss for most models

**Ampere**: **TF32** (automatic), **BF16**
- TF32: FP32 range, FP16 precision (10-bit mantissa)
- BF16: Better for transformers (wider range)
- FP64 Tensor Cores (HPC/AI convergence)

**Hopper**: **FP8** (E4M3 and E5M2)
- E4M3: 4-bit exp, 3-bit mantissa (precision)
- E5M2: 5-bit exp, 2-bit mantissa (range)
- Transformer Engine auto-selects per layer
- 2x throughput, 2x capacity vs FP16

**Blackwell**: **FP4** (NVFP4)
- Micro-tensor scaling
- 2x vs FP8 (4x vs FP16)
- Maintains accuracy

**Justification**: Neural networks tolerate noise, reduced precision with proper scaling/quantization maintains accuracy while doubling throughput and capacity.

---

### Question 7: Dominant Performance Bottleneck

| Architecture | Bottleneck | Why |
|--------------|------------|-----|
| Fermi | **Memory bandwidth** | 177 GB/s too low, ~8.5 FLOPs/byte |
| Volta | **Memory capacity** | 16 GB insufficient for large models |
| Turing | **Bandwidth (inference)** | Large batch memory-bound, small batch underutilizes |
| Ampere | **Communication** | Multi-GPU all-reduce for distributed training |
| Hopper | **Communication at scale** | NVLink 4.0 helps but exascale needs more |
| Blackwell | **Inference latency** | First-token latency for real-time AI |
| Rubin | **Energy efficiency** | Power density, cooling, data center limits |

---

### Question 8: Scaling Metrics (Compute vs Memory vs Interconnect)

| Gen | Compute (TFLOPS) | Memory BW | Interconnect | Ratio (FLOPs/byte) |
|-----|------------------|-----------|--------------|---------------------|
| Fermi | 1.5 FP32 | 177 GB/s | PCIe 2.0: 8 GB/s | ~8.5 |
| Volta | 125 FP16 (TC) | 900 GB/s | NVLink 2.0: 300 GB/s | ~139 |
| Ampere | 312 FP16 (TC) | 1.6 TB/s | NVLink 3.0: 600 GB/s | ~195 |
| Hopper | 2000 FP8 (TC) | 3.35 TB/s | NVLink 4.0: 900 GB/s | ~597 |
| Blackwell | ~4000 FP8 (est) | 8 TB/s | NVLink 5.0: 1.8 TB/s | ~500 |

**Pattern**: Compute grows faster than memory initially, then balances. Interconnect grows to support multi-GPU scaling.

---

### Question 9: Mapping to Dominant ML Workload

**Fermi (2010)**: Not designed for ML
- Pre-AlexNet era
- Used for: SIFT, SURF, shallow NNs
- Inefficient: No matrix ops, FP32 only

**Volta (2017)**: CNNs and early transformers
- Perfect for: ResNet-50, Inception v3 (batch 32-64)
- BERT-Base (110M): 3-4 fit in memory
- GPT-2 (1.5B): Requires model parallelism

**Turing (2018)**: Edge AI, real-time inference
- YOLO, SSD object detection
- DLSS (gaming super-resolution)
- Not for training (use V100/A100)

**Ampere (2020)**: Large transformer training
- GPT-3 175B: 1024×A100 for 34 days
- BERT: 4-8x faster than Volta
- Sparsity: 2x for pruned models

**Hopper (2022)**: Trillion-param transformers
- GPT-4 scale (rumored thousands of H100s)
- PaLM 540B efficient training
- 9x training speedup, 30x inference vs Ampere

**Blackwell (2024)**: Agentic AI, reasoning
- Reasoning models (o1-style): 50x better
- MoE models, long-context (100K+ tokens)
- 30x inference speedup vs Hopper

---

### Question 10: Architectural Invariants & Future Bottlenecks

**Invariants Across ALL Generations**:
1. **SM-based organization** (scales 16→144 SMs)
2. **Warp-level SIMT** (32 threads/warp, unchanged since 2006)
3. **Three-level memory hierarchy** (Reg → Shared/L1 → L2 → DRAM)
4. **Thread hierarchy** (Thread → Block → Grid, +Clusters in Hopper)

**Bottleneck Chain** (Each solution creates next problem):
```
Fermi: Memory BW
  ↓ (solved with HBM2)
Volta: GEMM throughput
  ↓ (solved with Tensor Cores)
Ampere: Training speed
  ↓ (solved with TF32, sparsity)
Hopper: Transformer attention
  ↓ (solved with FP8, Transformer Engine)
Blackwell: Inference scale
  ↓ (solved with FP4, NVL72)
Rubin: ENERGY EFFICIENCY ← Next frontier
  ↓ (requires novel physics?)
Future: Physical limits (CMOS, cooling, power density)
```

---

## 🎯 KEY CONCEPTS TO MEMORIZE

### 1. Tensor Core Evolution
- **Gen 1 (Volta)**: FP16×FP16→FP32, 4×4×4 MMA
- **Gen 2 (Turing)**: +INT8, +INT4
- **Gen 3 (Ampere)**: +TF32, +BF16, +FP64, +Sparsity
- **Gen 4 (Hopper)**: +FP8 (E4M3/E5M2), Transformer Engine
- **Gen 5 (Blackwell)**: +FP4 (NVFP4), 2x attention (Ultra)

### 2. Precision Formats
- **FP32**: 8-bit exp, 23-bit mantissa
- **FP16**: 5-bit exp, 10-bit mantissa
- **BF16**: 8-bit exp, 7-bit mantissa (same range as FP32!)
- **TF32**: 8-bit exp, **10-bit mantissa** (FP32 range, FP16 precision)
- **FP8 E4M3**: 4-bit exp, 3-bit mantissa (precision)
- **FP8 E5M2**: 5-bit exp, 2-bit mantissa (range)
- **FP4**: 4 bits total with micro-scaling

### 3. Memory Bandwidth Growth
177 GB/s (Fermi) → 900 GB/s (Volta, 5x) → 1.6 TB/s (Ampere, 1.8x) → 3 TB/s (Hopper, 1.9x) → 8 TB/s (Blackwell, 2.7x)

**Pattern**: ~2x every 2-3 years

### 4. Critical Innovations by Generation
- **Fermi**: Cache hierarchy, ECC
- **Volta**: Tensor Cores invented
- **Turing**: INT8 quantization
- **Ampere**: Structured sparsity (2:4), TF32, MIG
- **Hopper**: FP8, Transformer Engine, Thread Block Clusters
- **Blackwell**: FP4, dual-die, NVL72

### 5. Sparsity (Ampere)
- **2:4 structured**: Of every 4 values, exactly 2 are zero
- **2x speedup**: 312 TFLOPS → 624 TFLOPS
- **Accuracy**: <1% degradation with proper training
- **Compression**: Stores only 2 values + 2-bit metadata per 4 elements

### 6. Multi-Instance GPU (MIG) - Ampere
- Partition A100 into **up to 7 independent instances**
- Each gets: dedicated SMs, memory controllers, L2 cache
- **Benefit**: 30% → 90% utilization in cloud
- **Use case**: Multi-tenant inference

### 7. Transformer Engine (Hopper)
- Software + hardware for transformers
- **Auto FP8/FP16 switching** per layer
- **Dynamic loss scaling** from tensor statistics
- **Result**: 9x training, 30x inference speedup

### 8. NVLink Evolution
- NVLink 2.0 (Volta): 300 GB/s
- NVLink 3.0 (Ampere): 600 GB/s (2x)
- NVLink 4.0 (Hopper): 900 GB/s (1.5x)
- NVLink 5.0 (Blackwell): 1.8 TB/s (2x)
- **NVLink Switch System** (Hopper): 256 GPUs, 57.6 TB/s all-to-all

---

## 🔥 QUIZ TIPS

### High-Probability Questions:

1. **"What bottleneck did [Architecture X] solve?"**
   - Know the chain: Fermi (mem BW) → Volta (GEMM) → Ampere (training) → Hopper (transformers) → Blackwell (inference)

2. **"What precision does [Architecture X] support?"**
   - Fermi: FP32/FP64
   - Volta: FP16/FP32
   - Turing: +INT8/INT4
   - Ampere: +TF32/BF16/FP64TC
   - Hopper: +FP8 (E4M3/E5M2)
   - Blackwell: +FP4

3. **"What's the dominant compute unit in [Architecture X]?"**
   - Fermi: CUDA Core
   - Volta onward: Tensor Core (generation matters!)

4. **"How does [Architecture X] handle transformers?"**
   - Fermi-Ampere: Generic GEMM operations
   - Hopper: **Transformer Engine** (specialized)
   - Blackwell: Transformer Engine v2 + FP4

5. **"What's the next bottleneck after [Architecture X]?"**
   - Follow the chain, understand causality

### Memory Tricks:

- **"VTATHABR"**: Volta, Turing, Ampere, Hopper, Blackwell, Rubin (order)
- **Precision halving**: Every ~3 years, precision halves (32→16→8→4→2)
- **Memory 2x rule**: Bandwidth doubles every 2-3 years
- **Specialization increases**: Each gen becomes more specialized

### Common Mistakes to Avoid:

1. **Don't confuse Turing with Volta** - Turing is inference-focused, consumer GPU
2. **TF32 is Ampere, not Turing** - automatic FP32→TF32 acceleration
3. **FP8 is Hopper, not Blackwell** - Blackwell adds FP4
4. **Sparsity is 2:4, not 50%** - structured constraint matters
5. **MIG is Ampere, enhanced in Hopper** - not Volta

---

## 📊 COMPARISON TABLES

### Compute Performance (Peak TFLOPS/TOPS)

| Architecture | FP64 | FP32 | FP16 (TC) | INT8 | FP8 | FP4 |
|--------------|------|------|-----------|------|-----|-----|
| Fermi | 0.75 | 1.5 | — | — | — | — |
| Volta | 7.8 | 15.7 | 125 | — | — | — |
| Turing | — | 16 | 65 | 130 | — | — |
| Ampere | 19.5 (TC) | 19.5 | 312 | 624 | — | — |
| Hopper | 60 (TC) | 60 | 1000 | 2000 | **2000** | — |
| Blackwell | — | — | — | — | 4000+ | **8000+** |

### Memory Specifications

| Architecture | Type | Capacity | Bandwidth | L2 Cache |
|--------------|------|----------|-----------|----------|
| Fermi | GDDR5 | 6 GB | 177 GB/s | 768 KB |
| Volta | HBM2 | 16/32 GB | 900 GB/s | 6 MB |
| Turing | GDDR6 | 11 GB | 616 GB/s | 6 MB |
| Ampere | HBM2e | 40/80 GB | 1.6/2.039 TB/s | **40 MB** |
| Hopper | **HBM3** | 80 GB | 3.35 TB/s | 50 MB |
| Blackwell | HBM3e | **192 GB** | **8 TB/s** | — |

---

## 💡 FINAL STUDY CHECKLIST

Before the quiz, make sure you can answer:

- [ ] What bottleneck did each architecture solve?
- [ ] What precision formats does each support?
- [ ] What's the Tensor Core generation for Volta/Ampere/Hopper/Blackwell?
- [ ] Memory bandwidth for each generation
- [ ] What's TF32? (Ampere's auto FP32 acceleration)
- [ ] What's 2:4 sparsity? (Ampere's structured pruning)
- [ ] What's the Transformer Engine? (Hopper's FP8 auto-switching)
- [ ] What are Thread Block Clusters? (Hopper's multi-SM cooperation)
- [ ] What's MIG? (Multi-Instance GPU for multi-tenancy)
- [ ] What's the next bottleneck? (Energy efficiency for Rubin)

---

## 🎓 DEEP UNDERSTANDING QUESTIONS

If you can explain these, you've mastered the material:

1. **Why does precision halve every generation?**
   - ML workloads tolerate noise
   - Quantization-aware training compensates
   - 2x throughput + 2x capacity = massive win

2. **Why specialize at the cost of flexibility?**
   - 80-90% of workload is matrix multiplication
   - 8-12x speedup justifies idle time on other workloads
   - Trend: GPUs → Neural Processing Units

3. **Why did Ampere add FP64 Tensor Cores?**
   - HPC/AI convergence
   - Climate modeling, CFD need precision
   - 2.5x speedup over Volta FP64

4. **How does structured sparsity (2:4) work?**
   - Network pruned to 50% zeros with 2:4 constraint
   - Hardware compresses: stores 2 values + 2-bit index
   - Doubles throughput with <1% accuracy loss

5. **What makes Hopper's Transformer Engine special?**
   - Analyzes tensor statistics per layer
   - Dynamically selects E4M3 (precision) vs E5M2 (range)
   - Achieves FP16 accuracy at FP8 speed

6. **Why is energy efficiency the next bottleneck?**
   - Data centers hit megawatt power limits
   - Cooling becomes architectural constraint
   - Can't keep doubling transistors (Dennard scaling ended)
   - Need: Novel physics (photonics, superconductors)

---

Good luck on your quiz Tuesday! Focus on the bottleneck-solution chain and precision evolution—those are the core narrative threads.
