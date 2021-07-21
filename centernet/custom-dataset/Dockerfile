ARG TAG="21.05-py3"
FROM nvcr.io/nvidia/pytorch:$TAG

ENV FORCE_CUDA=1

ENV DATASETS_DIR=/var/datasets
ENV LIB_DIR=/opt

# Install basics.
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update -y \
 && apt-get install -y libgl1-mesa-glx neovim parallel tmux unzip wget

# Generate data.
WORKDIR $LIB_DIR
RUN git clone https://github.com/chaoz-dev/random-image-generator.git
RUN python3 random-image-generator/generate_images.py -n 50 -o $DATASETS_DIR/custom/ -wd 3840 -ht 2160

# Install COCO API.
WORKDIR $LIB_DIR
ENV COCOAPI_DIR = $LIB_DIR/cocoapi

RUN git clone https://github.com/cocodataset/cocoapi.git \
 && cd cocoapi \
 && python -m pip install -e PythonAPI

# Install DCNv2.
WORKDIR $LIB_DIR
ENV DCNV2_DIR = $LIB_DIR/DCNv2

RUN git clone https://github.com/chaoz-dev/DCNv2.git \
 && python -m pip install -e DCNv2

# Install CenterNet.
WORKDIR $LIB_DIR
ENV CENTERNET_DIR=$LIB_DIR/CenterNet

ARG CENTERNET_MODELS="\
 1r89_KNXyDyvUp8NggduG9uKQTMU2DsK_ \
 1pl_-ael8wERdUREEnaIfqOV_VF2bEVRT \
 1bTJCbAc1szA9lWU-fvVw52lqR3U2TTry \
 1b-_sjq1Pe_dVxt5SeFmoadMfiPTPZqpz"
 # 1cNyDmyorOduMRsgXoUnuyUiF6tZNFxaG \
 # 1-5bT5ZF8bXriJ-wAvOjJFrBLvZV2-mlV \
 # 1PFcEqN0KjFuq9XaqzB7TkVD3pvXZx04e"

RUN git clone https://github.com/chaoz-dev/CenterNet.git --single-branch --branch custom-dataset

WORKDIR $CENTERNET_DIR
RUN pip install -r requirements.txt \
 # Link DCNv2.
 && ln -s $DCNV2_DIR $CENTERNET_DIR/src/lib/models/networks/ \
 # Build libs.
 && (cd src/lib/external && make) \
 # Download models.
 && python -m pip install gdown \
 && parallel --will-cite --jobs 0 'gdown --id {}' ::: $CENTERNET_MODELS

