# ANN DreaMS benchmark

A benchmark for approximate nearest neighbor seacrh within large-scale DreaMS embeddings using `matchms` embedding similarity backends.

## Setup

### Environment prepeation

The environment requires a matchms installation from this fork: https://github.com/roman-bushuiev/matchms/tree/embedding_similarity and a couple of additional libraries `pip install tqdm`, `pip install h5py`.

### Data download

```
# 1k query DreaMS embeddings
wget -P data https://huggingface.co/datasets/roman-bushuiev/ANN_DreaMS_benchmark/resolve/main/data/MassSpecGym_DreaMS_rand1k.npy

# 50k reference DreaMS embeddings
wget -P data https://huggingface.co/datasets/roman-bushuiev/ANN_DreaMS_benchmark/resolve/main/data/GeMS_A1_DreaMS_rand50k.npy
wget -P data https://huggingface.co/datasets/roman-bushuiev/ANN_DreaMS_benchmark/resolve/main/data/GeMS_A1_DreaMS_rand50k.benchmark.npy

# 500k reference DreaMS embeddings
wget -P data https://huggingface.co/datasets/roman-bushuiev/ANN_DreaMS_benchmark/resolve/main/data/GeMS_A1_DreaMS_rand500k.npy
wget -P data https://huggingface.co/datasets/roman-bushuiev/ANN_DreaMS_benchmark/resolve/main/data/GeMS_A1_DreaMS_rand500k.benchmark.npy

# 5M reference DreaMS embeddings
wget -P data https://huggingface.co/datasets/roman-bushuiev/ANN_DreaMS_benchmark/resolve/main/data/GeMS_A1_DreaMS_rand5M.npy
wget -P data https://huggingface.co/datasets/roman-bushuiev/ANN_DreaMS_benchmark/resolve/main/data/GeMS_A1_DreaMS_rand5M.benchmark.npy
```

## Running a benchmark

To run a benchmark specify an ANN backend and a benchmarking dataset:

```python3
python3 benchmark.py --ann_backend pynndescent --dataset_name GeMS_A1_DreaMS_rand50k
```

Expected output:

```
Benchmark results:
index_backend: pynndescent
dataset_name: GeMS_A1_DreaMS_rand50k
peak_memory [MB]: 1310.5156
execution_time [s]: 30.0428
Recall @ 1 mean: 0.9370
Recall @ 1 std: 0.2430
Recall @ 10 mean: 0.8958
Recall @ 10 std: 0.1460
Query time mean [s]: 0.0169
Query time std [s]: 0.5275
```

### Implementing and benchmarkign new `matchms` ANN backend

`benchmark.py` code executes `BaseEmbeddingSimilarity(similarity="cosine", index_backend={ann_backend})` from matchms.
So, to evaluate a new backend one needs to implement it within a `BaseEmbeddingSimilarity` class [here](https://github.com/roman-bushuiev/matchms/blob/cb125728aa1580c49eff060a8da93fedd4426d12/matchms/similarity/BaseEmbeddingSimilarity.py#L113).

## TODO list:
- [] Evaluate FAISS.
- [] Evaluate annoy.
- [] Evaluate Voyager.
- [] Evaluate pynndescent (the only backend implemented in matchms so far).
- [] Explore other packages.
