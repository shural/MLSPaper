# GPU Architecture Evolution: Comprehensive Analysis
## Fermi through Rubin - A Bottleneck-Driven Survey

This document organizes research findings to answer the 10 key questions for each GPU architecture generation.

---

## FERMI (2010) - GF100/GF104

### 1. Fundamental Workload Shift & Bottleneck Resolved
**Workload Context**: Pre-deep learning era - GPGPU computing for HPC, scientific computing, financial modeling
**Bottleneck Resolved**: Transition from graphics-only to general-purpose computing
- Previous generation (G80/GT200) lacked ECC memory, double-precision performance
- Limited programmability with basic CUDA
- Fermi introduced: unified address space, ECC memory, true cache hierarchy

### 2. Dominant Unit of Computation
**CUDA Core** (scalar FP32/FP64 ALU)
- 512 CUDA cores per chip (16 SMs × 32 cores/SM)
- Dual-issue per core: FP32 + INT32 or FP32 + FP32
- FP64 support: 1/2 rate of FP32 (16 FP64 units per SM)
- **Evolution**: From purely graphics shaders → programmable CUDA cores

### 3. General-Purpose vs. Specialized Balance
**Highly general-purpose biased**
- Unified shader architecture
- Full IEEE 754-2008 compliance for FP32/FP64
- Programmable via C/C++ (CUDA)
- No specialized ML units (Tensor Cores didn't exist yet)
- Special Function Units (SFUs) for transcendentals only

### 4. Memory Hierarchy Evolution
**Major advancement**: True cache hierarchy introduced
- **L1 Cache**: 16KB or 48KB configurable (shared with 48KB or 16KB shared memory)
- **L2 Cache**: 768KB unified
- **GDDR5**: 384-bit interface, up to 6GB, ~177 GB/s bandwidth
- **Bottleneck addressed**: Random access patterns (HPC codes)
- **Problem**: Memory bandwidth still limiting factor for data-intensive workloads

### 5. Parallel Execution & Scheduling
- **Dual warp scheduler** per SM
- 32 threads/warp, up to 48 warps/SM (1536 threads/SM)
- **GigaThread engine**: hardware context switching
- **Concurrent kernel execution**: up to 16 kernels simultaneously
- **Evolution**: From single warp scheduler → dual dispatch for ILP

### 6. Numerical Precision
- **FP32**: Primary precision, IEEE 754-2008 compliant
- **FP64**: Full support at 1/2 FP32 rate
- **FP16**: Not supported
- **Justification**: HPC workloads require IEEE compliance, full precision
- No reduced precision - accuracy paramount for scientific computing

### 7. Dominant Performance Bottleneck
**Memory bandwidth** (secondary: compute)
- 177 GB/s vs. 1.5 TFLOPS FP32 = poor compute-to-bandwidth ratio
- Cache hierarchy helps but insufficient for streaming workloads
- PCIe 2.0 bottleneck for CPU-GPU transfers

### 8. Scaling Metrics
- **Compute**: 1.5 TFLOPS FP32, 0.75 TFLOPS FP64
- **Memory BW**: 177 GB/s
- **Interconnect**: PCIe 2.0 × 16 = 8 GB/s
- **Ratio**: ~8.5 FLOPs/byte (compute-bound on cache-friendly codes)

### 9. Dominant ML Workload Mapping
**Not designed for ML** - Pre-deep learning revolution
- AlexNet (2012) came 2 years after Fermi
- Used for: early computer vision (SIFT, SURF), basic neural nets
- Inefficient: No specialized matrix operations, FP32 only

### 10. Architectural Invariants & Future Bottlenecks
**Invariants established**:
- SM as basic compute unit
- Warp-based SIMT execution
- Shared memory + register file architecture

**New bottleneck**: Matrix multiplication throughput needed for emerging ML workloads

---

## VOLTA (2017) - GV100

### 1. Fundamental Workload Shift & Bottleneck Resolved
**Workload Shift**: Deep learning training explosion (AlexNet 2012 → ResNet 2015 → Transformer models emerging)
**Bottleneck Resolved**: Matrix multiplication throughput for deep learning
- **Key innovation**: Tensor Cores introduced
- Addresses: FP32 GEMM becoming training bottleneck
- 12x speedup for mixed-precision training over Pascal

### 2. Dominant Unit of Computation
**Tensor Core** (specialized matrix multiplication unit)
- **4 Tensor Cores per SM** (640 total in V100)
- Each performs 4×4×4 matrix multiply-accumulate per clock
- Operates on FP16 inputs, FP32 accumulation
- **Evolution**: CUDA Core → Tensor Core for ML dominance
- CUDA cores still present (5120 FP32 cores) for general compute

### 3. General-Purpose vs. Specialized Balance
**Hybrid architecture**:
- **80% specialized** for ML: Tensor Cores dominate die area
- **20% general**: CUDA cores for non-ML
- First GPU to prioritize specialized acceleration over pure programmability
- **Trade-off**: 12x faster ML, but Tensor Cores idle for non-matrix workloads

### 4. Memory Hierarchy Evolution
**HBM2 introduction** - major bandwidth leap
- **HBM2**: 4096-bit interface, 16GB/32GB, 900 GB/s (5x vs Fermi)
- **L2 Cache**: 6MB (8x larger than Fermi)
- **L1/Shared**: 128KB configurable per SM
- **Bottleneck addressed**: Memory wall for large models (ResNet-50, Inception)
- **New problem**: Large model capacity (16GB insufficient for GPT-scale)

### 5. Parallel Execution & Scheduling
- **Independent thread scheduling**: major change from lockstep warps
- Threads can diverge and reconverge at sub-warp granularity
- 4 warp schedulers per SM (2x Fermi)
- **Enables**: Fine-grained synchronization, better handling of irregular codes

### 6. Numerical Precision Evolution
- **FP16**: Primary for ML training (Tensor Core inputs)
- **FP32**: Accumulation in Tensor Cores, CUDA core default
- **FP64**: 7.8 TFLOPS (HPC still supported)
- **Justification**: FP16 sufficient for DL gradients with loss scaling
- **Mixed precision training**: maintains FP32 master weights

### 7. Dominant Performance Bottleneck
**Memory capacity** (for large models)
- Bandwidth improved 5x, but model sizes growing faster
- 16GB VRAM insufficient for large transformers
- **Communication** becoming bottleneck in multi-GPU (NVLink 2.0: 300 GB/s helps)

### 8. Scaling Metrics
- **Compute (Tensor)**: 125 TFLOPS FP16
- **Compute (CUDA)**: 15.7 TFLOPS FP32
- **Memory BW**: 900 GB/s
- **NVLink**: 300 GB/s total bandwidth (6 links × 50 GB/s)
- **Ratio**: ~139 FLOPs/byte (highly compute-bound for Tensor Core workloads)

### 9. Dominant ML Workload Mapping
**Perfect for 2017-era CNNs and early transformers**:
- ResNet-50, Inception v3: batch size 32-64 fits in 16GB
- BERT-Base (110M params): 3-4 fits in memory
- **Inefficiency**: GPT-2 (1.5B) requires model parallelism
- Enabled: Mixed-precision training becoming standard

### 10. Architectural Invariants & New Bottlenecks
**Invariants**:
- Tensor Cores become permanent fixture
- Mixed precision as default paradigm
- HBM standard for high-end GPUs

**New bottleneck**:
- Multi-GPU communication for model parallelism
- Attention mechanism quadratic complexity in transformers

---

## TURING (2018) - TU102/TU104

### 1. Fundamental Workload Shift & Bottleneck Resolved
**Workload Shift**: Inference deployment + real-time ray tracing
**Bottleneck Resolved**: Inference latency and INT8 quantization support
- Volta optimized for training, Turing targets inference
- RT Cores for ray tracing (graphics + AI hybrid)
- INT8/INT4 Tensor Cores for efficient inference

### 2. Dominant Unit of Computation
**Dual architecture**:
- **Tensor Cores (Gen 2)**: INT8/INT4/FP16 support
- **RT Cores**: Ray-triangle intersection + BVH traversal
- CUDA cores: 4608 in TU102
- **Evolution**: Specialization diversifies - not just ML, but also ray tracing

### 3. General-Purpose vs. Specialized Balance
**Highly specialized** (70% specialized):
- Tensor Cores: ML inference
- RT Cores: Ray tracing
- General CUDA: 30% of compute capability
- **Gaming GPU** with ML capabilities (vs. V100 data center focus)

### 4. Memory Hierarchy Evolution
**GDDR6** - different approach from V100
- GDDR6: 352-bit, 11GB, ~616 GB/s (consumer focus)
- L2: 6MB (same as V100)
- L1/Shared: 96KB per SM
- **Trade-off**: Lower bandwidth than HBM2, but cheaper for consumer market
- **Bottleneck**: Bandwidth constrained for large batch inference

### 5. Parallel Execution & Scheduling
- Concurrent int32 and FP32 execution paths
- Independent thread scheduling (inherited from Volta)
- Improved occupancy for graphics + compute hybrid workloads
- **Graphics-specific**: Mesh shaders, variable rate shading

### 6. Numerical Precision Evolution
**Inference-focused precision**:
- **INT8**: 4x throughput of FP16 for inference
- **INT4**: 8x throughput for extreme quantization
- **FP16**: Training and high-accuracy inference
- **Justification**: Post-training quantization maintains accuracy for most models
- Enables: Real-time inference on edge devices

### 7. Dominant Performance Bottleneck
**Bandwidth for inference**:
- Large batch inference memory-bound
- Small batch inference underutilizes Tensor Cores
- **Latency** critical for real-time applications (gaming, edge AI)

### 8. Scaling Metrics
- **Tensor (INT8)**: 130 TOPS
- **Tensor (FP16)**: 65 TFLOPS
- **CUDA FP32**: 16 TFLOPS
- **Memory BW**: 616 GB/s
- **Ratio**: ~106 OPs/byte (INT8)

### 9. Dominant ML Workload Mapping
**Edge AI and inference deployment**:
- Real-time object detection (YOLO, SSD)
- Style transfer, super-resolution (DLSS uses Tensor Cores)
- **Not ideal for**: Large model training (use V100/A100 instead)
- Perfect for: Inference at the edge, gaming AI

### 10. Architectural Invariants & New Bottlenecks
**Invariants**:
- Multi-precision Tensor Cores
- Hybrid graphics + compute architecture

**New bottleneck**:
- Attention mechanism still quadratic
- Model size growing faster than inference optimization

---

## AMPERE (2020) - GA100 (A100)

### 1. Fundamental Workload Shift & Bottleneck Resolved
**Workload Shift**: GPT-3 era - massive language models, trillion-parameter models emerging
**Bottleneck Resolved**: Training throughput + structural sparsity + multi-tenancy
- **Sparsity**: 2:4 fine-grained structured sparsity (2x speedup)
- **MIG**: Multi-Instance GPU for cloud efficiency
- **TF32**: Automatic FP32 acceleration for lazy migrations

### 2. Dominant Unit of Computation
**Third-Generation Tensor Core**:
- **TF32**: New precision - FP32 range, FP16 precision (10-bit mantissa)
- **BF16**: Google's Brain Float 16
- **FP64 Tensor Core**: HPC acceleration (2.5x over V100 FP64)
- **Sparsity engine**: 2x throughput for sparse networks
- **Evolution**: Precision diversity + HPC/AI convergence

### 3. General-Purpose vs. Specialized Balance
**70% specialized, but more flexible**:
- Tensor Cores handle FP64 (HPC) + TF32/FP16/BF16 (AI)
- Sparsity hardware fixed-function (structured only)
- CUDA cores: simultaneous FP32 + INT32 execution
- **Balance shift**: Specialization with broader applicability

### 4. Memory Hierarchy Evolution
**Capacity explosion**:
- **HBM2e**: 40GB (2.5x V100), 1.6 TB/s bandwidth
- **L2**: 40MB (6.7x V100!) - massive cache
- **L1/Shared**: 192KB per SM (1.5x V100)
- **Bottleneck addressed**: GPT-3 175B fits on 8× A100 (model parallelism)
- **Compression**: Compute data compression reduces effective bandwidth needs

### 5. Parallel Execution & Scheduling
- **Async copy**: Direct global→shared memory transfers
- **Task graphs**: Hardware-accelerated kernel launch
- **Async barriers**: Producer-consumer patterns in hardware
- **Evolution**: Latency hiding through asynchrony, not just parallelism

### 6. Numerical Precision Evolution
**Precision explosion**:
- **TF32**: Default for PyTorch/TensorFlow (automatic 10x speedup)
- **BF16**: Better for transformers (wider dynamic range)
- **FP64 Tensor**: HPC workloads (climate, CFD)
- **INT8**: Inference
- **Sparsity**: 2x on all precisions
- **Justification**: Different workloads need different precisions

### 7. Dominant Performance Bottleneck
**Communication in distributed training**:
- Single-GPU memory (40GB) enough for moderate models
- **NVLink 3.0**: 600 GB/s helps but insufficient for trillion-param models
- **All-reduce** communication dominates in large-scale training

### 8. Scaling Metrics
- **Tensor (TF32)**: 156 TFLOPS (312 sparse)
- **Tensor (FP16)**: 312 TFLOPS (624 sparse)
- **Tensor (FP64)**: 19.5 TFLOPS
- **Memory BW**: 1.6 TB/s
- **NVLink 3.0**: 600 GB/s
- **Ratio**: ~97 FLOPs/byte (TF32), but sparsity doubles effective ratio

### 9. Dominant ML Workload Mapping
**Large transformer training**:
- GPT-3 175B: 1024× A100 for 34 days
- BERT training: 4-8x faster than V100
- **Sparsity**: 2x speedup for pruned models (requires training)
- **MIG**: 7 instances for multi-tenant inference

### 10. Architectural Invariants & New Bottlenecks
**Invariants**:
- Tensor Cores support all workload precisions
- Large L2 cache reduces DRAM traffic

**New bottleneck**:
- **Transformer attention**: O(n²) memory and compute
- FP8 precision could provide next leap (Hopper preview)

---

## HOPPER (2022) - GH100 (H100)

### 1. Fundamental Workload Shift & Bottleneck Resolved
**Workload Shift**: Trillion-parameter LLMs (GPT-4, PaLM, Megatron), transformer dominance
**Bottleneck Resolved**: Transformer-specific acceleration + FP8 precision
- **Transformer Engine**: Automatic FP8/FP16 switching with dynamic scaling
- **Thread block clusters**: Multi-SM cooperation for large GEMMs
- **DPX instructions**: Dynamic programming (genomics, routing)

### 2. Dominant Unit of Computation
**Fourth-Generation Tensor Core + Transformer Engine**:
- **FP8**: 2x throughput over FP16 (E4M3 and E5M2 formats)
- **Transformer Engine**: Software + hardware for attention layers
- **DPX units**: Smith-Waterman (7x over A100)
- **Evolution**: Domain-specific primitives (transformers, DP algorithms)

### 3. General-Purpose vs. Specialized Balance
**80% specialized, transformer-aware**:
- Transformer Engine = Tensor Cores + software orchestration
- DPX = fixed-function for DP algorithms
- CUDA cores: 3x faster FP64/FP32 for HPC
- **Philosophy**: Accelerate the most common patterns, not generic operations

### 4. Memory Hierarchy Evolution
**HBM3 debut + giant L2**:
- **HBM3**: 80GB, 3 TB/s bandwidth (2x A100)
- **L2**: 50MB (25% larger than A100)
- **L1/Shared**: 256KB per SM (33% larger)
- **TMA**: Tensor Memory Accelerator for efficient data movement
- **Bottleneck addressed**: Multi-trillion parameter models, long-context transformers

### 5. Parallel Execution & Scheduling
**Thread Block Clusters** (revolutionary):
- Clusters: groups of thread blocks across multiple SMs
- **Distributed shared memory**: Direct SM-to-SM communication
- 7x faster than global memory for inter-block exchange
- **Enables**: Efficient large-matrix tiling, cooperative fetching

### 6. Numerical Precision Evolution
**FP8 introduction**:
- **E4M3**: 4-bit exponent, 3-bit mantissa (precision)
- **E5M2**: 5-bit exponent, 2-bit mantissa (range)
- **Transformer Engine**: Per-layer adaptive FP8/FP16 selection
- **Justification**: Transformers tolerate lower precision with proper scaling
- 2x throughput, 2x capacity over FP16

### 7. Dominant Performance Bottleneck
**Communication at scale**:
- **NVLink 4.0**: 900 GB/s (1.5x A100)
- **NVLink Switch System**: 256 GPUs, 57.6 TB/s all-to-all
- Training GPT-4 scale models needs exaflop systems
- **Bottleneck**: Communication, not local compute

### 8. Scaling Metrics
- **Tensor (FP8)**: 2000 TFLOPS (4000 sparse)
- **Tensor (FP16)**: 1000 TFLOPS (2000 sparse)
- **FP64**: 60 TFLOPS (3x A100)
- **Memory BW**: 3 TB/s
- **NVLink 4.0**: 900 GB/s
- **Ratio**: ~667 FLOPs/byte (FP8) - extremely compute-bound

### 9. Dominant ML Workload Mapping
**Trillion-parameter transformers**:
- GPT-4 rumored to use thousands of H100s
- PaLM-540B: efficiently trainable
- Long-context transformers (32K+ tokens)
- **Transformer Engine**: 9x training speedup, 30x inference vs A100

### 10. Architectural Invariants & New Bottlenecks
**Invariants**:
- Tensor Cores as primary compute
- Multi-precision support
- Asynchronous execution model

**New bottleneck**:
- **Attention remains O(n²)**: FlashAttention helps but not architectural
- Need: Even lower precision (FP4?) or algorithm changes

---

## BLACKWELL (2024) - GB100/GB200

### 1. Fundamental Workload Shift & Bottleneck Resolved
**Workload Shift**: Multi-trillion parameter models, inference at scale, mixture-of-experts
**Bottleneck Resolved**: Inference throughput, FP4/FP6 precision, multi-chip scaling
- **Second-gen Transformer Engine**: FP4 support
- **Dual-die design**: 2× reticle-limited dies with 10 TB/s interconnect
- **NVLink Switch 5.0**: 1.8 TB/s per GPU

### 2. Dominant Unit of Computation
**Fifth-Generation Tensor Core (Ultra variant)**:
- **NVFP4**: 4-bit floating point with micro-tensor scaling
- **FP6**: Intermediate precision
- **2x attention layer acceleration** (Blackwell Ultra)
- **208 billion transistors** (dual-die)
- **Evolution**: Sub-byte precision with maintained accuracy

### 3. General-Purpose vs. Specialized Balance
**85% specialized, inference-optimized**:
- Transformer Engine v2 dominates
- Decompression engine for data analytics
- RAS engine for reliability
- **Focus**: AI factories, not general HPC

### 4. Memory Hierarchy Evolution
**Dual-die memory system**:
- **HBM3e**: 192GB per GPU (2.4x H100)
- **Bandwidth**: 8 TB/s (2.7x H100)
- **Grace-Blackwell**: 900 GB/s CPU-GPU link
- **Decompression engine**: LZ4, Snappy, Deflate in hardware
- **Bottleneck addressed**: Inference memory capacity for long-context, MoE models

### 5. Parallel Execution & Scheduling
**NVL72 architecture**:
- 72 GPUs in single NVLink domain
- 130 TB/s GPU bandwidth in NVL72
- **SHARP FP8**: In-network reductions
- **Evolution**: Rack-scale as unit of computation

### 6. Numerical Precision Evolution
**FP4 for inference**:
- **NVFP4**: Training-level accuracy at 4-bit
- **Micro-tensor scaling**: Fine-grained per-tensor scaling
- **Justification**: 2x memory capacity, 2x throughput vs FP8
- Enables 400B parameter models in memory previously for 200B

### 7. Dominant Performance Bottleneck
**Inference latency for real-time AI**:
- Agent workflows need <100ms response
- Prefill-decode imbalance in long-context
- **Bottleneck**: First-token latency remains high

### 8. Scaling Metrics (GB200 NVL72)
- **Tensor (FP4)**: ~80 petaFLOPS (estimated, NVL72)
- **Tensor (FP8)**: 40 petaFLOPS
- **Memory BW**: 576 TB/s aggregate (NVL72)
- **NVLink**: 130 TB/s GPU-GPU
- **Ratio**: Extremely compute-bound

### 9. Dominant ML Workload Mapping
**Agentic AI and reasoning models**:
- GPT-4-level inference at 30x lower cost
- Mixture-of-experts (MoE) models
- Long-context (100K+ tokens)
- **Reasoning models**: 50x better performance

### 10. Architectural Invariants & New Bottlenecks
**Invariants**:
- Tensor Cores dominate
- Multi-chip as standard design
- NVLink as primary interconnect

**New bottleneck**:
- **Reasoning workloads**: Test-time compute scaling
- Need: Speculative decoding, better scheduling

---

## RUBIN (2026-2027) - Next Generation

### 1. Fundamental Workload Shift & Bottleneck Resolved (PROJECTED)
**Expected Workload**: 10T+ parameter models, real-time multimodal AI
**Bottlenecks to Solve**:
- Test-time compute scaling for reasoning
- Multimodal fusion (vision + language + audio)
- Energy efficiency at exascale

### 2. Dominant Unit of Computation (PROJECTED)
**Likely**:
- **Sixth-gen Tensor Cores**: FP2/FP3? Binary/ternary networks?
- **Specialized attention units**: Hardware FlashAttention
- **Multimodal cores**: Vision-language co-processors

### 3. General-Purpose vs. Specialized Balance (PROJECTED)
**90% specialized** - trend continues
- Model-specific primitives (MoE routing, attention variants)
- Less general-purpose CUDA core investment

### 4. Memory Hierarchy Evolution (PROJECTED)
**Expected**:
- **HBM4**: 10+ TB/s bandwidth
- **Capacity**: 256GB+ per GPU
- **Near-memory compute**: Processing-in-memory for attention?

### 5. Parallel Execution & Scheduling (PROJECTED)
- Rack-scale or pod-scale as programmable unit
- Hardware scheduling for MoE routing
- Speculative execution for inference

### 6. Numerical Precision Evolution (PROJECTED)
- **Sub-FP4**: Binary/ternary networks?
- **Adaptive precision**: Per-layer, per-token precision
- **Logarithmic formats**: For extreme dynamic range

### 7. Dominant Performance Bottleneck (PROJECTED)
**Energy efficiency**:
- Data center power limits hit physical constraints
- Performance-per-watt becomes primary metric
- Cooling becomes architectural constraint

### 8. Scaling Metrics (PROJECTED)
- Multi-exaFLOP per system
- 100+ TB/s memory bandwidth
- Petabyte-scale total memory

### 9. Dominant ML Workload Mapping (PROJECTED)
- Real-time AGI-level reasoning
- Multimodal understanding (video, 3D, time-series)
- Test-time scaling (o1-style inference)

### 10. Architectural Invariants & Future Bottlenecks
**Invariants that will persist**:
- Tensor Cores (or equivalent matrix units)
- Hierarchical memory
- High-bandwidth interconnects

**Next bottleneck**:
- **Physical limits**: Power density, cooling
- **Algorithm shifts**: Sparse MoE → ??? (state space models? new architectures?)
- **Efficiency paradox**: More compute enables larger models which need more compute

---

## CROSS-GENERATIONAL TRENDS

### Bottleneck Evolution Chain
1. **Fermi**: Memory bandwidth for HPC
2. **Volta**: Matrix multiply throughput for DL
3. **Turing**: Inference efficiency (INT8)
4. **Ampere**: Training throughput (sparsity, TF32)
5. **Hopper**: Transformer-specific (FP8, Transformer Engine)
6. **Blackwell**: Inference capacity (FP4, multi-chip)
7. **Rubin**: Energy efficiency, test-time compute

### Precision Trajectory
FP32 → FP16 → INT8 → TF32/BF16 → FP8 → FP4 → FP2/binary?

### Specialization Trajectory
General CUDA → Tensor Cores → Transformer Engine → Model-specific primitives

### Memory Bandwidth Growth
177 GB/s → 900 GB/s → 1.6 TB/s → 3 TB/s → 8 TB/s → ?

### Compute Growth (FP16/TF32 equivalent)
~20 TFLOPS → 125 TFLOPS → 312 TFLOPS → 1000 TFLOPS → ?

### Architectural Invariants Across All Generations
1. **SM-based organization**: Scales from 16 to 144 SMs
2. **Warp-level SIMT**: 32 threads/warp unchanged
3. **Three-level memory**: Registers → Shared/L1 → L2 → DRAM
4. **Thread hierarchy**: Thread → Block → Grid (+ Cluster in Hopper)
