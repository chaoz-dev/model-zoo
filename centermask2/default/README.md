# CenterMask2 (https://github.com/youngwanLEE/centermask2)
This document provides the instructions for running CenterMask2.
For more information on CenterMask2, see the linked original repository.

## Evaluation

### Requirements
As everything is run within a docker container, the only requirements are Docker and the latest Nvidia drivers. Everything else is installed as needed in the docker container. See the included DockerFile for full details.

#### Dataset
The benchmarks use the COCO 2017 dataset.

#### Dependencies
CenterMask2 uses Facebook's Detectron2 library.

### Benchmarking
1. Build the DockerFile.
```
docker bulid -t centermask2:latest .
```

2. Run the docker image (in non-interactive mode).
```
docker run --gpus all --rm centermask2:latest python train_net.py --config-file "<file>" --num-gpus 1 --eval-only MODEL.WEIGHTS <model>
```

Currently, only the following CenterMask2 models are included in the built image:
 - V2-57
 - V2-99

For example:
```
docker run --gpus all --rm centermask2:latest python train_net.py --config-file "configs/centermask/centermask_V_99_eSE_FPN_ms_3x.yaml" --num-gpus 1 --eval-only MODEL.WEIGHTS centermask2-V-99-eSE-FPN-ms-3x.pth
```

3. Check output.
```
[d2.evaluation.evaluator]: Total inference time: 0:15:08.878585 (0.181958 s / iter per device, on 1 devices)
[d2.evaluation.evaluator]: Total inference pure compute time: 0:13:08 (0.157947 s / iter per device, on 1 devices)
...
```
