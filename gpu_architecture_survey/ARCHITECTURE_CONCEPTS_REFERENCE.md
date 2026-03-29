# Architecture Concepts Reference Guide

**Purpose**: Foundation concepts for understanding GPU architecture evolution
**Audience**: Readers of the GPU Architecture Survey and Quiz Materials
**Last Updated**: 2026-03-29

---

## Table of Contents

1. [General Computer Architecture Fundamentals](#general-computer-architecture-fundamentals)
2. [NVIDIA-Specific Architecture Concepts](#nvidia-specific-architecture-concepts)
3. [Performance Metrics and Analysis](#performance-metrics-and-analysis)
4. [ML-Specific Hardware Concepts](#ml-specific-hardware-concepts)

---

## General Computer Architecture Fundamentals

### Memory Hierarchy

#### **Cache Memory**
- **Definition**: Fast, small memory close to the processor
- **Purpose**: Reduce average memory access latency
- **Levels**: L1 (fastest, smallest), L2, L3 (slower, larger)
- **Cache Line**: Unit of data transfer (typically 64-128 bytes)
- **Cache Hit/Miss**: Data found in cache (hit) vs must fetch from DRAM (miss)
- **Spatial Locality**: Nearby addresses accessed together
- **Temporal Locality**: Same address accessed repeatedly

**Example**: GPU L1 cache (Fermi: 16KB-48KB per SM, configurable)

#### **Memory Bandwidth**
- **Definition**: Rate of data transfer between memory and processor
- **Unit**: GB/s or TB/s
- **Importance**: Bottleneck for data-intensive operations
- **Formula**: Bandwidth = Data Size / Time

**Example**: Hopper HBM3: 3.35 TB/s

#### **Memory Latency**
- **Definition**: Time delay between requesting data and receiving it
- **Unit**: Clock cycles or nanoseconds
- **DRAM Latency**: Typically 100-300 cycles
- **Cache Latency**: L1: ~4 cycles, L2: ~20 cycles

#### **Virtual Memory**
- **Definition**: Abstraction that maps virtual addresses to physical memory
- **Page**: Fixed-size memory block (typically 4KB)
- **TLB (Translation Lookaside Buffer)**: Cache for address translations

---

### Parallelism Models

#### **SIMD (Single Instruction, Multiple Data)**
- **Definition**: One instruction operates on multiple data elements simultaneously
- **Example**: Vector instructions (AVX, SSE on CPUs)
- **Width**: Number of data elements processed (e.g., 128-bit SIMD = 4×32-bit floats)

**GPU Context**: Foundation for warp execution

#### **SIMT (Single Instruction, Multiple Thread)**
- **Definition**: NVIDIA's model - multiple threads execute same instruction
- **Difference from SIMD**: Threads can diverge (conditional branching)
- **Warp**: Group of threads executing in lockstep (32 threads in NVIDIA GPUs)

**Key Concept**: GPU's fundamental execution model

#### **Data Parallelism**
- **Definition**: Same operation applied to different data elements
- **Example**: Apply filter to every pixel in an image
- **GPU Strength**: Thousands of threads process different data

#### **Task Parallelism**
- **Definition**: Different operations executed simultaneously
- **Example**: CPU cores running different programs

---

### Compute Primitives

#### **ALU (Arithmetic Logic Unit)**
- **Definition**: Circuit that performs arithmetic and logic operations
- **Operations**: ADD, SUB, MUL, DIV, AND, OR, XOR
- **Integer vs Floating-Point**: Separate units typically

#### **FPU (Floating-Point Unit)**
- **Definition**: Specialized circuit for floating-point arithmetic
- **Precision**: FP64 (double), FP32 (single), FP16 (half)
- **Operations**: FADD, FMUL, FDIV, FSQRT

#### **FMA (Fused Multiply-Add)**
- **Definition**: Single instruction: `result = (a × b) + c`
- **Advantage**: More accurate (one rounding), faster (one operation)
- **FLOPS Counting**: Counts as 2 operations (multiply + add)

**Example**: Core of matrix multiplication

#### **Matrix Multiply-Accumulate (MMA)**
- **Definition**: Hardware unit for small matrix multiplication
- **Example**: 4×4×4 matrix multiply in one operation
- **GPU Use**: Tensor Cores implement MMA

---

### Performance Concepts

#### **Throughput vs Latency**
- **Throughput**: Operations completed per unit time (ops/sec)
- **Latency**: Time to complete one operation (seconds)
- **Trade-off**: GPUs optimize for throughput, CPUs balance both

#### **Bandwidth vs Compute**
- **Compute-Bound**: Limited by computational capacity (FLOPS)
- **Memory-Bound**: Limited by memory bandwidth
- **Balanced**: Compute and memory bandwidth matched

#### **Arithmetic Intensity**
- **Definition**: FLOPs performed per byte of data moved
- **Formula**: FLOPs / Bytes
- **Unit**: FLOPs/byte
- **Importance**: Determines if operation is compute-bound or memory-bound

**Example**: Hopper: 597 FLOPs/byte (highly compute-bound)

#### **Roofline Model**
- **Definition**: Performance visualization showing compute vs memory limits
- **X-axis**: Arithmetic intensity (FLOPs/byte)
- **Y-axis**: Performance (GFLOPS)
- **Two Limits**: Memory bandwidth ceiling, compute ceiling

---

### Instruction Set Architecture (ISA)

#### **Instruction**
- **Definition**: Single operation the processor can execute
- **Components**: Opcode (operation), operands (inputs/outputs)

#### **Register**
- **Definition**: Small, fast storage inside processor
- **Types**: General-purpose, special-purpose (PC, SP)
- **GPU Registers**: Each thread has private registers

#### **Instruction Pipeline**
- **Definition**: Breaking instruction execution into stages
- **Stages**: Fetch → Decode → Execute → Memory → Write-back
- **Purpose**: Process multiple instructions simultaneously
- **Hazard**: When pipeline must stall (data dependency, branch)

---

### Synchronization and Coherence

#### **Barrier**
- **Definition**: Synchronization point where threads wait for all to arrive
- **Purpose**: Ensure all threads complete before proceeding
- **GPU Use**: `__syncthreads()` in CUDA

#### **Atomic Operation**
- **Definition**: Operation that completes without interruption
- **Example**: Atomic add, compare-and-swap
- **Purpose**: Prevent race conditions in parallel code

#### **Cache Coherence**
- **Definition**: Ensuring all caches see consistent data
- **Problem**: Multiple caches may have different values for same address
- **Solution**: Coherence protocols (MESI, MOESI)

---

## NVIDIA-Specific Architecture Concepts

### Core GPU Architecture

#### **SM (Streaming Multiprocessor)**
- **Definition**: Independent processing unit with own compute cores and memory
- **Components**: CUDA cores, Tensor Cores, shared memory, L1 cache, registers
- **Count**: Varies by GPU (Volta: 80 SMs, Hopper: 132 SMs)
- **Independence**: Each SM can execute different warps

**Analogy**: SM is like a CPU core, but much more parallel

#### **CUDA Core**
- **Definition**: Scalar ALU + FPU for general-purpose computation
- **Operations**: FP32, INT32 arithmetic
- **Evolution**:
  - Fermi-Pascal: Primary compute unit
  - Volta+: Supplemented by Tensor Cores for matrix ops

#### **Tensor Core**
- **Definition**: Specialized hardware for mixed-precision matrix multiplication
- **Operation**: `D = A × B + C` where A, B, C, D are 4×4 matrices
- **Evolution**:
  - Gen 1 (Volta): FP16 input, FP32 accumulate
  - Gen 2 (Turing): +INT8, +INT4
  - Gen 3 (Ampere): +TF32, +BF16, +FP64, +sparsity
  - Gen 4 (Hopper): +FP8 (E4M3, E5M2)
  - Gen 5 (Blackwell): +FP4

**Key Insight**: 10-100x faster than CUDA cores for matrix ops

#### **RT Core (Ray Tracing Core)**
- **Definition**: Hardware for ray-triangle intersection and BVH traversal
- **Purpose**: Real-time ray tracing (graphics)
- **First Appeared**: Turing (2018)

---

### Thread Hierarchy

#### **Thread**
- **Definition**: Smallest unit of execution
- **Properties**: Private registers, local memory
- **GPU Threads**: Lightweight, millions can exist

#### **Warp**
- **Definition**: Group of 32 threads executing in SIMT fashion
- **Lockstep Execution**: All threads execute same instruction
- **Divergence**: When threads take different branches (serialized execution)
- **Warp Scheduling**: SM can have 32-64 warps resident, switch rapidly

**Critical Concept**: Fundamental execution unit in NVIDIA GPUs

#### **Thread Block (Block)**
- **Definition**: Group of threads (up to 1024) that can cooperate
- **Shared Memory**: Block-local fast memory
- **Synchronization**: Threads in block can sync via `__syncthreads()`
- **Mapping**: One block executes on one SM

#### **Grid**
- **Definition**: Collection of thread blocks
- **Kernel Launch**: Grid of blocks launched to execute kernel
- **Independence**: Blocks execute independently (no inter-block sync during kernel)

**Hierarchy**: Grid → Blocks → Warps → Threads

#### **Thread Block Cluster (Hopper)**
- **Definition**: Group of thread blocks that can synchronize across SMs
- **Purpose**: Enable multi-SM cooperation
- **Benefit**: Distributed shared memory across SMs (7x faster than global)

---

### Memory Hierarchy (GPU-Specific)

#### **Register File**
- **Definition**: Per-thread private fast storage
- **Size**: Typically 64KB-256KB per SM
- **Access**: 1 cycle latency
- **Limitation**: Limited registers → fewer active threads

#### **Shared Memory**
- **Definition**: Per-block programmable cache
- **Size**: 48KB-228KB per SM (configurable with L1)
- **Access**: ~20 cycles latency
- **Purpose**: Explicit data sharing within thread block
- **Bank Conflicts**: When multiple threads access same bank

**Key Use**: Manual optimization for data reuse

#### **L1 Cache**
- **Definition**: Per-SM cache for global memory accesses
- **Size**: 16KB-128KB (often configurable with shared memory)
- **Access**: Automatic (hardware-managed)

#### **L2 Cache**
- **Definition**: Shared across all SMs
- **Size**:
  - Fermi: 768KB
  - Volta: 6MB
  - Ampere: 40MB
  - Hopper: 50MB
- **Purpose**: Reduce global memory traffic
- **Residency Control**: Programmer hints (Ampere+)

#### **Global Memory (HBM)**
- **Definition**: Main GPU memory (DRAM)
- **Technology**: HBM (High Bandwidth Memory), stacked DRAM
- **Size**: 16GB-192GB
- **Bandwidth**: 900 GB/s (Volta) → 8 TB/s (Blackwell)
- **Latency**: ~400-800 cycles

**Evolution**: HBM2 → HBM2e → HBM3 → HBM3e → HBM4 (Rubin)

#### **Texture Memory**
- **Definition**: Read-only memory with spatial locality cache
- **Purpose**: Optimized for 2D/3D spatial access patterns
- **Use**: Graphics textures, image processing

#### **Constant Memory**
- **Definition**: Read-only memory cached per-SM
- **Size**: 64KB
- **Purpose**: Broadcast same value to all threads

---

### CUDA Programming Model

#### **Kernel**
- **Definition**: GPU function executed by many threads in parallel
- **Launch**: `kernel<<<grid, block>>>(args)`
- **Execution**: Grid of blocks, each with multiple warps

#### **Host vs Device**
- **Host**: CPU and its memory
- **Device**: GPU and its memory
- **Transfer**: Data copied between host and device via PCIe/NVLink

#### **Occupancy**
- **Definition**: Ratio of active warps to maximum possible warps per SM
- **Factors**: Registers per thread, shared memory per block, block size
- **Goal**: High occupancy = good latency hiding

#### **Latency Hiding**
- **Definition**: Switching to other warps while one waits for memory
- **Requirement**: Many active warps (high occupancy)
- **Trade-off**: Resource usage vs occupancy

---

### NVIDIA-Specific Features

#### **Warp Scheduling**
- **Independent Thread Scheduling (Volta+)**: Threads can diverge and reconverge flexibly
- **Pre-Volta**: Lockstep execution with reconvergence at branch join
- **Benefit**: Better handling of complex control flow

#### **MIG (Multi-Instance GPU)** (Ampere+)
- **Definition**: Partition GPU into isolated instances
- **Partitions**: Up to 7 instances with guaranteed QoS
- **Resources**: Each gets dedicated SMs, memory, L2 cache
- **Purpose**: Multi-tenant cloud deployments

#### **TMA (Tensor Memory Accelerator)** (Hopper)
- **Definition**: Hardware unit for asynchronous tensor data transfers
- **Purpose**: Automate data movement from global to shared memory
- **Benefit**: Single thread manages transfer, others do compute
- **Result**: Complete overlap of data movement and computation

#### **Transformer Engine** (Hopper)
- **Definition**: Hardware + software for automatic FP8/FP16 precision selection
- **Components**: FP8 Tensor Cores + dynamic scaling library
- **Purpose**: Optimize transformer model training/inference
- **Benefit**: 9x speedup over Ampere for transformers

#### **Structured Sparsity** (Ampere+)
- **Definition**: 2:4 sparsity pattern (2 non-zeros per 4 values)
- **Hardware**: Tensor Cores skip zero computations
- **Speedup**: 2x throughput
- **Constraint**: Must train model to have 2:4 structure

---

### Interconnect Technologies

#### **PCIe (Peripheral Component Interconnect Express)**
- **Definition**: Standard bus connecting CPU and GPU
- **Bandwidth**: PCIe 4.0: 32 GB/s, PCIe 5.0: 64 GB/s
- **Limitation**: Bottleneck for GPU-CPU data transfer

#### **NVLink**
- **Definition**: NVIDIA's high-speed GPU-GPU interconnect
- **Evolution**:
  - V2 (Volta): 300 GB/s bidirectional
  - V3 (Ampere): 600 GB/s
  - V4 (Hopper): 900 GB/s
  - V5 (Blackwell): 1.8 TB/s
  - V6 (Rubin, projected): Optical, 10+ TB/s
- **Purpose**: Multi-GPU systems, enable GPU-to-GPU communication

#### **NVLink Switch System** (Hopper)
- **Definition**: External switch connecting up to 256 GPUs
- **Bandwidth**: 57.6 TB/s aggregate
- **Purpose**: Supercomputer-scale training

#### **NVL72** (Blackwell)
- **Definition**: 72-GPU rack with unified memory space
- **Bandwidth**: 130 TB/s inter-GPU
- **Programming**: Entire rack as single GPU

---

## Performance Metrics and Analysis

### FLOPS (Floating-Point Operations Per Second)

#### **Definition and Units**
- **FLOPS**: Single operation per second
- **GFLOPS**: 10^9 FLOPS
- **TFLOPS**: 10^12 FLOPS
- **PFLOPS**: 10^15 FLOPS

#### **Peak vs Sustained**
- **Peak FLOPS**: Theoretical maximum (all units active)
- **Sustained FLOPS**: Achieved in practice
- **Efficiency**: Sustained / Peak (typically 50-80%)

#### **Mixed Precision**
- **FP64**: Scientific computing, 1/2 to 1/32 of FP32 rate
- **FP32**: General purpose
- **FP16**: Deep learning training
- **TF32/BF16**: Automatic mixed precision (Ampere+)
- **FP8**: Transformer training (Hopper+)
- **FP4**: Inference (Blackwell+)

**Example**: Hopper: 2000 TFLOPS (FP8), 1000 TFLOPS (FP16), 60 TFLOPS (FP32)

---

### Memory Performance Metrics

#### **Bandwidth Utilization**
- **Formula**: (Achieved Bandwidth / Peak Bandwidth) × 100%
- **Target**: >80% for memory-bound kernels

#### **Memory Throughput**
- **Definition**: Actual data transfer rate achieved
- **Measurement**: Bytes transferred / time
- **Tools**: NVIDIA Nsight Compute

#### **FLOPs/byte (Arithmetic Intensity)**
- **Definition**: Operations performed per byte of memory traffic
- **Low (<10)**: Memory-bound (element-wise ops)
- **Medium (10-100)**: Balanced
- **High (>100)**: Compute-bound (matrix multiply)

**Example Workloads**:
- Vector addition: ~0.25 FLOPs/byte (memory-bound)
- Matrix multiply: ~100+ FLOPs/byte (compute-bound)

---

### Roofline Analysis

#### **Roofline Model Components**
1. **Compute Ceiling**: Peak FLOPS (horizontal line)
2. **Memory Ceiling**: Peak bandwidth × arithmetic intensity (diagonal line)
3. **Achieved Performance**: Actual kernel performance

#### **Interpretation**
- **Below compute ceiling**: Not using all compute units
- **Below memory ceiling**: Not saturating memory bandwidth
- **At ceiling**: Fully utilizing that resource

#### **Optimization Guidance**
- Hit memory ceiling → Increase arithmetic intensity (data reuse)
- Hit compute ceiling → More parallelism or better algorithm

---

## ML-Specific Hardware Concepts

### Numerical Precision

#### **IEEE 754 Formats**
- **FP64 (Double)**: 1 sign, 11 exp, 52 mantissa = 64 bits
- **FP32 (Single)**: 1 sign, 8 exp, 23 mantissa = 32 bits
- **FP16 (Half)**: 1 sign, 5 exp, 10 mantissa = 16 bits

#### **Non-Standard Formats**

**TF32 (TensorFloat-32)** (Ampere+)
- **Bits**: 1 sign, 8 exp, 10 mantissa = 19 bits (in 32-bit container)
- **Purpose**: FP32 range with FP16 precision
- **Use**: Automatic replacement for FP32 in matrix ops
- **Benefit**: 10x faster than FP32, negligible accuracy loss

**BF16 (Brain Float 16)**
- **Bits**: 1 sign, 8 exp, 7 mantissa = 16 bits
- **Purpose**: FP32 range in 16 bits
- **Use**: Gradient accumulation in training
- **Benefit**: Same dynamic range as FP32

**FP8 (Hopper)**
- **E4M3**: 1 sign, 4 exp, 3 mantissa (activations, weights)
- **E5M2**: 1 sign, 5 exp, 2 mantissa (gradients, wider range)
- **Dynamic Scaling**: Per-tensor scaling factors
- **Use**: Transformer training/inference

**FP4 (Blackwell)**
- **Bits**: 4 bits total
- **Use**: Inference only
- **Scaling**: Requires per-element or block scaling
- **Benefit**: 2x throughput vs FP8, 4x vs FP16

#### **Integer Formats**
- **INT8**: 8-bit integer (-128 to 127)
- **INT4**: 4-bit integer (-8 to 7)
- **Quantization**: Converting FP weights to INT for inference

---

### Matrix Operations

#### **GEMM (General Matrix Multiply)**
- **Operation**: `C = α(A × B) + βC`
- **Complexity**: O(n³) for n×n matrices
- **Optimization**: Tiling, data reuse
- **GPU Advantage**: Embarrassingly parallel

#### **Convolution**
- **Definition**: Sliding window operation
- **ML Use**: CNNs (image processing)
- **Implementation**: Often converted to GEMM via im2col
- **Tensor Cores**: Accelerate via GEMM transformation

#### **Attention Mechanism**
- **Operation**: `Attention(Q,K,V) = softmax(QK^T/√d)V`
- **Components**: 2 matrix multiplies + softmax
- **Bottleneck**: Softmax (memory-bound)
- **Hopper Optimization**: Fused attention in Transformer Engine

---

### Model Parallelism Strategies

#### **Data Parallelism**
- **Definition**: Same model replicated, different data batches
- **Synchronization**: Gradient averaging across replicas
- **Scaling**: Nearly linear up to ~100 GPUs

#### **Model Parallelism (Tensor Parallel)**
- **Definition**: Model split across GPUs (layer-wise)
- **Communication**: Activations exchanged between GPUs
- **Use**: When model doesn't fit single GPU

#### **Pipeline Parallelism**
- **Definition**: Model stages on different GPUs
- **Execution**: Pipeline different micro-batches
- **Trade-off**: Bubble time (idle GPUs)

---

### Training vs Inference

#### **Training Characteristics**
- **Operation**: Forward + backward pass
- **Precision**: FP16/BF16 with FP32 accumulation
- **Memory**: Needs gradients, optimizer states
- **Batch Size**: Large (64-1024)

#### **Inference Characteristics**
- **Operation**: Forward pass only
- **Precision**: FP16/INT8/FP4 (aggressive quantization)
- **Memory**: Only weights + activations
- **Batch Size**: Small (1-8) for latency-sensitive
- **Throughput**: Large batches for offline inference

---

## Quick Reference Tables

### Memory Hierarchy Speed Comparison

| Memory Type | Latency | Bandwidth | Size |
|-------------|---------|-----------|------|
| Register | 1 cycle | ~TB/s | 64-256KB/SM |
| Shared Memory | ~20 cycles | ~TB/s | 48-228KB/SM |
| L1 Cache | ~20 cycles | ~TB/s | 128KB/SM |
| L2 Cache | ~200 cycles | 100s GB/s | 6-50MB |
| Global (HBM) | ~400 cycles | 0.9-8 TB/s | 16-192GB |

### Precision Comparison

| Format | Bits | Range | Precision | Use |
|--------|------|-------|-----------|-----|
| FP64 | 64 | ±10^308 | ~15 digits | Scientific |
| FP32 | 32 | ±10^38 | ~7 digits | General |
| TF32 | 19 | ±10^38 | ~3 digits | DL (auto) |
| BF16 | 16 | ±10^38 | ~2 digits | DL gradients |
| FP16 | 16 | ±10^4 | ~3 digits | DL forward |
| FP8 | 8 | ±448 | ~1 digit | Transformers |
| INT8 | 8 | ±127 | Integer | Inference |
| FP4 | 4 | Limited | <1 digit | Inference |

### GPU Memory Bandwidth Evolution

| Generation | Year | Memory | Bandwidth |
|------------|------|--------|-----------|
| Fermi | 2010 | GDDR5 | 177 GB/s |
| Volta | 2017 | HBM2 | 900 GB/s |
| Ampere | 2020 | HBM2e | 1.6-2.0 TB/s |
| Hopper | 2022 | HBM3 | 3.35 TB/s |
| Blackwell | 2024 | HBM3e | 8 TB/s |
| Rubin | 2026 | HBM4 (est) | 10+ TB/s |

---

## Glossary of Abbreviations

- **ALU**: Arithmetic Logic Unit
- **API**: Application Programming Interface
- **ASIC**: Application-Specific Integrated Circuit
- **BVH**: Bounding Volume Hierarchy
- **CFD**: Computational Fluid Dynamics
- **CUDA**: Compute Unified Device Architecture
- **DL**: Deep Learning
- **DMA**: Direct Memory Access
- **DRAM**: Dynamic Random-Access Memory
- **ECC**: Error-Correcting Code
- **FLOPS**: Floating-Point Operations Per Second
- **FMA**: Fused Multiply-Add
- **FPU**: Floating-Point Unit
- **GEMM**: General Matrix Multiply
- **GPU**: Graphics Processing Unit
- **HBM**: High Bandwidth Memory
- **HPC**: High-Performance Computing
- **ISA**: Instruction Set Architecture
- **MESI**: Modified, Exclusive, Shared, Invalid (cache coherence)
- **MIG**: Multi-Instance GPU
- **ML**: Machine Learning
- **MMA**: Matrix Multiply-Accumulate
- **NVLink**: NVIDIA Link (interconnect)
- **PCIe**: Peripheral Component Interconnect Express
- **QoS**: Quality of Service
- **RT**: Ray Tracing
- **SIMD**: Single Instruction, Multiple Data
- **SIMT**: Single Instruction, Multiple Thread
- **SM**: Streaming Multiprocessor
- **SRAM**: Static Random-Access Memory
- **TC**: Tensor Core
- **TLB**: Translation Lookaside Buffer
- **TMA**: Tensor Memory Accelerator

---

## How to Use This Reference

### For Quiz Preparation:
1. Review general concepts first (memory hierarchy, parallelism)
2. Understand NVIDIA-specific terms (SM, warp, Tensor Core)
3. Focus on concepts mentioned in quiz materials
4. Use tables for quick fact checking

### For Paper Reading:
1. Look up unfamiliar terms as you encounter them
2. Understand the evolution of concepts across generations
3. Connect general principles to NVIDIA implementations

### For Deep Understanding:
1. Read concept definitions
2. Study examples from GPU architectures
3. Trace concept evolution (e.g., precision: FP32→FP16→FP8→FP4)

---

## Additional Resources

### Official NVIDIA Documentation:
- CUDA Programming Guide
- GPU Architecture Whitepapers (per generation)
- CUDA C++ Best Practices Guide

### Academic Resources:
- Computer Architecture: A Quantitative Approach (Hennessy & Patterson)
- Parallel Computer Architecture (Culler, Singh, Gupta)

### Online Resources:
- NVIDIA Developer Blog
- CUDA Toolkit Documentation
- GPU Architecture courses (Udacity, Coursera)

---

*This reference guide complements the GPU Architecture Survey paper and quiz materials. For architecture-specific details, refer to the main survey documents.*

**Related Documents:**
- GPU Architecture Survey (paper/gpu_architecture_survey_optionA.tex, optionB.tex)
- QUIZ/QUIZ_STUDY_GUIDE.md
- QUIZ/ONE_PAGE_CHEATSHEET.md
