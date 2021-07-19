# CenterNet (https://github.com/xingyizhou/CenterNet)
This document provides the instructions for running CenterNet.
For more information on CenterNet, see the linked original repository.

## Evaluation

### Requirements
As everything is run within a docker container, the only requirements are Docker and the latest Nvidia drivers. Everything else is installed as needed in the docker container. See the included DockerFile for full details.

Note that forks of the original repositories are used here instead, as they include patches to enable forward compatibility with various libraries.

#### Dataset
The benchmarks use the COCO 2017 dataset.

#### Dependencies
CenterNet also depends on the DCNv2 library.

### Benchmarking
1. Build the DockerFile.
```
docker bulid -t centernet:latest .
```

2. Run the docker image (in non-interactive mode).
```
docker run --gpus all --rm centernet:latest python src/test.py ctdet --arch <arch> --exp_id <name> --keep_res --load_model <model>
```

Currently, only the following CenterNet models are included in the built image:
 - ctdet_coco_dla_1x
 - ctdet_coco_dla_2x
 - ctdet_coco_resdcn101
 - ctdet_coco_resdcn18

For example:
```
docker run --gpus all --rm centernet:latest python src/test.py ctdet --arch resdcn_101 --exp_id resdcn101 --keep_res --load_model ctdet_coco_resdcn101.pth
```

3. Check output.
```
...
DONE (t=10.87s).
 Average Precision  (AP) @[ IoU=0.50:0.95 | area=   all | maxDets=100 ] = 0.346
 Average Precision  (AP) @[ IoU=0.50      | area=   all | maxDets=100 ] = 0.530
 Average Precision  (AP) @[ IoU=0.75      | area=   all | maxDets=100 ] = 0.369
 Average Precision  (AP) @[ IoU=0.50:0.95 | area= small | maxDets=100 ] = 0.151
 Average Precision  (AP) @[ IoU=0.50:0.95 | area=medium | maxDets=100 ] = 0.399
 Average Precision  (AP) @[ IoU=0.50:0.95 | area= large | maxDets=100 ] = 0.506
 Average Recall     (AR) @[ IoU=0.50:0.95 | area=   all | maxDets=  1 ] = 0.299
 Average Recall     (AR) @[ IoU=0.50:0.95 | area=   all | maxDets= 10 ] = 0.473
 Average Recall     (AR) @[ IoU=0.50:0.95 | area=   all | maxDets=100 ] = 0.492
 Average Recall     (AR) @[ IoU=0.50:0.95 | area= small | maxDets=100 ] = 0.237
 Average Recall     (AR) @[ IoU=0.50:0.95 | area=medium | maxDets=100 ] = 0.552
 Average Recall     (AR) @[ IoU=0.50:0.95 | area= large | maxDets=100 ] = 0.726
```
