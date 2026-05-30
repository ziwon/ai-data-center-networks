# AI Data Center Systems

AI data center networking, LLM inference, training, storage, and AI systems performance engineering study notes.

<p align="center">
<img width="1024" height="341" alt="ai-datacenter-networks" src="https://github.com/user-attachments/assets/48ff6793-1d84-47d6-9b7c-7c11c233271d" />
</p>

## Study Tracks

| Track | Description |
| --- | --- |
| [AI Data Center Network](./ai-data-center-network/README.md) | AI 데이터센터 네트워크, RDMA, InfiniBand, RoCE, Clos fabric |
| [Efficient LLM Inference Systems](./efficient-llm-inference-systems/README.md) | LLM inference 성능, KV cache, batching, GPU profiling |
| [CME295 Lecture Notes](./cme295/README.md) | Stanford CME295 Transformer/LLM 강의 기반 한국어 lecture notes |
| [Deep Learning for Network Engineers](./deep-learning-for-network-engineers/README.md) | Deep learning model, training process, and network-related engineering topics |
| [AI Systems Performance Engineering](./ai-system-performance-engineering/README.md) | GPU, CUDA, PyTorch 기반 AI 시스템 성능 엔지니어링 |
| [Training](./training/README.md) | MLPerf Training, distributed training, LLM/MoE/LoRA/recommendation workload |
| [Storage](./storage/README.md) | AI workload storage, ZFS, checkpoint/data pipeline |

## CME295 Lecture Notes

한국어로 정리한 Stanford CME295 Transformer and LLM lecture notes다. Transformer 기초부터 LLM inference, training, preference tuning, reasoning까지 이어지는 흐름을 다룬다.

| Lecture | Topic | Notes |
| --- | --- | --- |
| 01 | Transformer 기초 | [lec-01](./cme295/lec-01/README.md) |
| 02 | Transformer-based models and tricks | [lec-02](./cme295/lec-02/README.md) |
| 03 | LLMs, decoding, prompting, and inference | [lec-03](./cme295/lec-03/README.md) |
| 04 | LLM training, fine-tuning, and efficient adaptation | [lec-04](./cme295/lec-04/README.md) |
| 05 | LLM tuning and human preferences | [lec-05](./cme295/lec-05/README.md) |
| 06 | LLM reasoning and GRPO | [lec-06](./cme295/lec-06/README.md) |

## Labs

- [Clos Fabric Lab Series](./ai-data-center-network/clos-ebgp-lab/README.md)
- [InfiniBand Packet Analysis](./ai-data-center-network/ib-packet-analysis/README.md)
- [RDMA Read/Write Examples](./ai-data-center-network/rdma-examples/README.md)
