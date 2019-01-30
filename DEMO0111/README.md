# Env. check

1. Nvidia Driver version: `nvidia-smi`, `nvidia-smi topo --matrix`, `nvidia-smi nvlink --status`
2. CUDA Version: `nvcc --version`
3. Cudnn Version: `cat /usr/lib/x86_64-linux-gnu/include/cudnn.h | grep CUDNN_MAJOR -A 2`


# Materials 

excel: https://docs.google.com/spreadsheets/d/10D_sx9bpKWZLHDb-Po7j_R4wqSqK68AjhnH1t2UZKjo/edit?usp=sharing
presentation:https://drive.google.com/open?id=1jJdUZRT7IV_4WxfBqyrNg4jkv9XsBi7ocwJPhyUvBy0

## Task1: LeaderGPU
```
git clone https://github.com/reedwm/benchmarks
cd benchmarks
git checkout num_steps_issue_1.10
cd scripts/tf_cnn_benchmarks/
```

### gpu:1 batch:32
```
python tf_cnn_benchmarks.py --num_gpus=1 --batch_size=32 --model=resnet50 --variable_update=parameter_server
```

### gpu:2 batch:128
```
python tf_cnn_benchmarks.py --num_gpus=2 --batch_size=128 --model=resnet50 --variable_update=parameter_server
```

## Task2: Tensorflow official
```
git clone https://github.com/uber/horovod
cd horovod/examples/
```

try 
```
python tensorflow_synthetic_benchmark.py
```

* NOTE: hvd.Compression.fp16 not work in this source code, just skip line:59 ! like this...
![img](https://snag.gy/qevcXm.jpg)

### run w/ mpi 
for 1 GPU
```
/usr/local/mpi/bin/mpirun -np 1 -H localhost:1 python tensorflow_synthetic_benchmark.py
```
for 8 GPU
```
/usr/local/mpi/bin/mpirun -np 8 -H localhost:8 python tensorflow_synthetic_benchmark.py
```