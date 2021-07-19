ARG TAG="21.05-py3"
FROM nvcr.io/nvidia/pytorch:$TAG

ENV DATASETS_DIR=/var/datasets
ENV LIB_DIR=/opt

# Install basics.
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update -y \
 && apt-get install -y neovim parallel tmux unzip wget

# Generate data.
WORKDIR $LIB_DIR
RUN git clone https://github.com/chaoz-dev/random-image-generator.git
RUN python3 random-image-generator/generate_images.py -n 50 -o $DATASETS_DIR/custom/ -wd 3840 -ht 2160

# Install Detectron2.
WORKDIR $LIB_DIR

ENV DETECTRON2_DATASETS=$DATASETS_DIR
RUN git clone https://github.com/facebookresearch/detectron2.git \
 && python -m pip install -e detectron2

# Install CenterMask2.
WORKDIR $LIB_DIR
ENV CENTERMASK2_DIR=$LIB_DIR/centermask2

ARG VOVNET2_MODELS="\
 https://dl.dropbox.com/s/lw8nxajv1tim8gr/centermask2-V-57-eSE-FPN-ms-3x.pth \
 https://dl.dropbox.com/s/c6n79x83xkdowqc/centermask2-V-99-eSE-FPN-ms-3x.pth"

RUN git clone https://github.com/chaoz-dev/centermask2.git --single-branch --branch custom-dataset

WORKDIR $CENTERMASK2_DIR
RUN parallel --will-cite --jobs 0 'wget -nv -nc {}' ::: $VOVNET2_MODELS
