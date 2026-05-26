# MLPerf Storage Benchmark

[MLCommons Storage](https://github.com/mlcommons/storage)는 ML 워크로드를 지원하는 스토리지 시스템의 성능을 측정하기 위한 MLPerf Storage Benchmark Suite입니다. 단순 파일시스템 벤치마크가 아니라 학습 데이터 로딩, 체크포인트 저장/복구, LLM KV cache offload, VectorDB 검색처럼 ML 시스템에서 실제로 문제가 되는 I/O 패턴을 재현합니다.

## 읽을 자료

| 주제 | 링크 | ML 관점에서 볼 내용 |
| --- | --- | --- |
| 전체 개요 | [mlcommons/storage](https://github.com/mlcommons/storage) | 벤치마크 설치, `--file`/`--object` 백엔드 선택, training/checkpointing/vectordb/kvcache 카테고리 |
| 문서 인덱스 | [docs/README.md](https://github.com/mlcommons/storage/blob/main/docs/README.md) | 네 가지 벤치마크 워크로드와 관련 문서의 진입점 |
| 빠른 시작 | [docs/QUICK_START.md](https://github.com/mlcommons/storage/blob/main/docs/QUICK_START.md) | 로컬 파일시스템, S3 object storage, checkpointing, KV cache, VectorDB 실행 예제 |
| Training I/O | [training/README.md](https://github.com/mlcommons/storage/blob/main/training/README.md) | FLUX.1, RetinaNet, DLRMv2 학습 데이터 로딩 패턴, dataset sizing/datagen/run 절차 |
| Checkpointing | [checkpointing/README.md](https://github.com/mlcommons/storage/blob/main/checkpointing/README.md) | Llama3 8B/70B/405B/1T 체크포인트 쓰기/읽기, cache clear, shared/local storage 모드 |
| Streaming Checkpoint | [docs/Streaming-Chkpt-Guide.md](https://github.com/mlcommons/storage/blob/main/docs/Streaming-Chkpt-Guide.md) | 대형 체크포인트 생성과 쓰기를 streaming 방식으로 처리하는 구조 |
| KV Cache Benchmark | [kv_cache_benchmark/README.md](https://github.com/mlcommons/storage/blob/main/kv_cache_benchmark/README.md) | LLM inference에서 GPU VRAM, CPU RAM, NVMe 사이 KV cache offload 지연과 처리량 측정 |
| VectorDB Benchmark | [vdb_benchmark/README.md](https://github.com/mlcommons/storage/blob/main/vdb_benchmark/README.md) | Milvus 기반 DiskANN, HNSW, AISAQ 인덱스의 vector search storage 성능 |
| Object Storage Guide | [docs/OBJECT_STORAGE_GUIDE.md](https://github.com/mlcommons/storage/blob/main/docs/OBJECT_STORAGE_GUIDE.md) | S3-compatible object storage에서 `.env`, bucket, endpoint, `--object` 설정 |
| Storage Libraries | [docs/STORAGE_LIBRARIES.md](https://github.com/mlcommons/storage/blob/main/docs/STORAGE_LIBRARIES.md) | `s3dlio`, `minio`, `s3torchconnector` 비교 |
| DataLoader Architecture | [docs/DATALOADER_ARCHITECTURE.md](https://github.com/mlcommons/storage/blob/main/docs/DATALOADER_ARCHITECTURE.md) | object storage와 NVMe에서 map-style/iterable-style DataLoader, prefetch, O_DIRECT 차이 |
| Multi Endpoint | [docs/MULTI_ENDPOINT_GUIDE.md](https://github.com/mlcommons/storage/blob/main/docs/MULTI_ENDPOINT_GUIDE.md) | 여러 S3 endpoint로 I/O를 분산하는 방법 |
| Parquet Format | [docs/PARQUET_FORMATS.md](https://github.com/mlcommons/storage/blob/main/docs/PARQUET_FORMATS.md) | Parquet reader, row group, byte-range GET 기반 학습 데이터 포맷 실험 |
| 제출 규칙 | [Rules.md](https://github.com/mlcommons/storage/blob/main/Rules.md) | CLOSED/OPEN submission, power/RU normalized metric, benchmark compliance |

## 예제 실행

### 1. Training I/O: 로컬 NVMe/파일시스템

학습 워크로드에서 storage가 accelerator를 얼마나 잘 먹여주는지 보는 기본 실험입니다.

```bash
git clone https://github.com/mlcommons/storage.git
cd storage
uv sync

uv run mlpstorage training datagen \
  --model retinanet \
  --num-processes 4 \
  --open --file \
  --data-dir /mnt/nvme_data/retinanet \
  --params dataset.num_files_train=250000

uv run mlpstorage training run \
  --model retinanet \
  --num-accelerators 4 \
  --accelerator-type b200 \
  --client-host-memory-in-gb 64 \
  --open --file \
  --data-dir /mnt/nvme_data/retinanet \
  --params dataset.num_files_train=250000
```

### 2. Training I/O: S3-compatible Object Storage

데이터셋을 파일 경로가 아니라 object key prefix로 두고, object storage가 학습 데이터 로딩을 버틸 수 있는지 측정합니다.

```bash
cat > .env <<'EOF'
AWS_ENDPOINT_URL=http://127.0.0.1:9000
AWS_ACCESS_KEY_ID=minioadmin
AWS_SECRET_ACCESS_KEY=minioadmin
AWS_REGION=us-east-1
STORAGE_LIBRARY=s3dlio
BUCKET=mlp-retinanet
EOF

uv run mlpstorage training datagen \
  --model retinanet \
  --num-processes 4 \
  --open --object \
  --data-dir retinanet \
  --params dataset.num_files_train=250000

uv run mlpstorage training run \
  --model retinanet \
  --num-accelerators 4 \
  --accelerator-type b200 \
  --client-host-memory-in-gb 64 \
  --open --object \
  --data-dir retinanet \
  --params dataset.num_files_train=250000
```

### 3. Checkpointing: LLM 학습 장애 복구 I/O

대규모 LLM 학습에서 checkpoint write/read throughput과 가장 느린 rank의 tail latency를 봅니다.

```bash
uv run mlpstorage checkpointing run \
  --model llama3-8b \
  --num-processes 8 \
  --client-host-memory-in-gb 512 \
  --checkpoint-folder /mnt/checkpoint_test \
  --num-checkpoints-read=0

# read phase 전에 필요하면 OS page cache를 비웁니다.
sudo sh -c 'echo 3 > /proc/sys/vm/drop_caches'

uv run mlpstorage checkpointing run \
  --model llama3-8b \
  --num-processes 8 \
  --client-host-memory-in-gb 512 \
  --checkpoint-folder /mnt/checkpoint_test \
  --num-checkpoints-write=0
```

### 4. LLM KV Cache Offload

긴 context와 다중 사용자 inference에서 KV cache가 CPU/NVMe로 밀려날 때 storage tier의 지연을 측정합니다.

```bash
cd kv_cache_benchmark
pip install ".[full]"

python3 kv-cache.py \
  --config config.yaml \
  --model llama3.1-8b \
  --num-users 50 \
  --duration 120 \
  --gpu-mem-gb 0 \
  --cpu-mem-gb 4 \
  --cache-dir /mnt/nvme \
  --output results.json
```

### 5. VectorDB/RAG Storage

Milvus의 vector load, index build, query 단계에서 storage가 latency/throughput/recall에 미치는 영향을 봅니다.

```bash
cd storage
uv sync --extra vectordb
uv pip install -e ./vdb_benchmark

docker compose -f vdb_benchmark/docker-compose.yml up -d

./mlpstorage vectordb datasize \
  --dimension 1536 \
  --num-vectors 10000000 \
  --index-type DISKANN \
  --num-shards 10

./mlpstorage vectordb datagen \
  --host 127.0.0.1 \
  --port 19530 \
  --config default \
  --num-vectors 50000 \
  --dimension 1536 \
  --num-shards 1 \
  --force \
  --results-dir /tmp/vdb_results
```

## 실험 아이디어

- 같은 RetinaNet datagen/run을 로컬 NVMe, NFS, ZFS dataset, S3-compatible object storage에서 반복해 accelerator utilization과 read throughput을 비교합니다.
- `docs/DATALOADER_ARCHITECTURE.md`를 기준으로 map-style vs iterable-style DataLoader, O_DIRECT on/off, worker 수, prefetch depth를 sweep합니다.
- checkpointing은 write-only와 read-only phase를 분리하고, page cache를 비운 cold read와 비우지 않은 warm read를 비교합니다.
- KV cache benchmark는 `--gpu-mem-gb`, `--cpu-mem-gb`, `--cache-dir`를 바꿔 GPU/CPU/NVMe tiering 임계점을 찾습니다.
- VectorDB는 DiskANN처럼 disk-heavy한 인덱스와 HNSW처럼 memory-heavy한 인덱스를 같은 storage backend에서 비교합니다.
