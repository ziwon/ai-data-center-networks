# Deep Learning for Network Engineers

A six-week engineering study of how deep learning workloads shape GPU communication, distributed training, and AI data center backend networks.

<img width="1672" height="941" alt="image" src="https://github.com/user-attachments/assets/c4f68d13-1942-436a-bbe0-35f360da5456" />

<br/>
This repository is not focused on building models for accuracy.  
It is focused on understanding what happens underneath modern AI workloads:

- Why large models require distributed GPU clusters
- Why gradient synchronization becomes a network problem
- Why NCCL collectives create bursty east-west traffic
- Why RDMA, RoCE, PFC, ECN, and DCQCN matter
- Why backend topology design directly affects training performance

The study follows *Deep Learning for Network Engineers* and interprets each chapter through the lens of MLOps, LLMOps, and AI infrastructure engineering.

## Study Direction

The focus of this study is not to become a model researcher or to build a production-grade LLM from scratch. Instead, the goal is to understand the infrastructure implications of modern AI workloads:

```text
Model size grows
→ single GPU becomes insufficient
→ distributed training is required
→ GPUs must synchronize data
→ collective communication creates bursty east-west traffic
→ backend network design becomes part of the AI training system
````

Each week is organized around one practical question:

> What does this deep learning concept mean for GPU utilization, network traffic, storage pressure, and AI cluster design?

## Six-Week Study Plan

* [Week 1: From Artificial Neuron to AI Training Fabric](./week01/README.md)
  Revisit artificial neurons, forward pass, backward pass, gradients, and weight updates from an infrastructure perspective. The key focus is understanding why training eventually leads to GPU memory pressure and distributed synchronization.

* [Week 2: Introduction to Large Language Models and Training](./week02/README.md)
  Study tokenization, embeddings, Transformer basics, attention, feed-forward layers, and next-token prediction. The goal is to understand why LLMs require large GPU memory, high compute throughput, and scalable distributed training.

* [Week 3: Parallelism Strategy and Communication Primitives](./week03/README.md)
  Study data parallelism, tensor parallelism, pipeline parallelism, and the communication patterns behind them. The focus is on how AllReduce, ReduceScatter, AllGather, and activation transfers affect GPU cluster performance.

* Week 4: RDMA, RoCE, and NCCL Communication
  Connect distributed training with RDMA-based GPU-to-GPU communication. The goal is to understand how NCCL uses high-speed backend networks and why low latency, high throughput, and lossless transport matter.

* Week 5: Congestion Control and AI Fabric Reliability
  Study the operational challenges of AI fabrics, including packet loss, congestion, PFC, ECN, DCQCN, head-of-line blocking, and hash polarization. The focus is on why AI training networks require careful congestion control and validation

* Week 6: Backend Network Topology and Design Review
  Study backend topology choices such as shared NIC, NIC-per-GPU, single-rail, dual-rail, and rail-optimized designs. The final goal is to produce an AI training fabric design review from the perspective of an MLOps/LLOps engineer.

## Weekly Output Format

Each week should produce a short engineering note with the following structure:

```text
1. Core Concepts
2. Why It Matters for MLOps / LLMOps
3. GPU, Network, and Storage Impact
4. Communication Pattern
5. Failure or Bottleneck Scenario
6. Observability and Validation Points
7. Practical Design Takeaways
```

## Final Deliverable

By the end of the six-week study, the expected output is not just a summary of the book, but a practical design-oriented document:

```text
AI Training Fabric Design Review

- Workload assumptions
- Model and GPU scaling requirements
- Parallelism strategy
- Collective communication patterns
- RDMA/RoCE or InfiniBand considerations
- Congestion control requirements
- Backend topology design
- Validation and observability checklist
```

This study is intended to bridge deep learning theory and real-world AI infrastructure design, especially for engineers working on GPU clusters, distributed training, inference platforms, and AI data center networking.
