# Fruit Quality Check Training Pipeline

This is a part the final project of Hanna Kondrashova at the University of London.

This repository contains the training pipeline to produce a computer vision model 
based on YOLOv5 that can process the following classes:
- Rotten apple
- Good apple
- Storage containing a large amount of apples that are difficult to distinguish

__NOTE__: the HTTP service using this model is in
[QualityCheck](https://github.com/hannakond/QualityCheck) repository.

## Prerequisites.

1. Setup CUDA >= 11.0 following [the official guide](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/).
Ultimately, you need an AWS or Google Cloud instance with a GPU having ~7GB of memory to run the service.
__NOTE__: the GPU memory size for training is more than for inference! From pragmatic point of view,
it's better to have a powerful GPU for training, training on CPU takes a lot of time.

2. Clone the repository:

```bash
$ git clone git@github.com:hannakond/QualityCheckTrain.git
```

After that, clone submodules and LFS objects:

```bash
$ git lfs pull
$ git submodule init
$ git submodule update
```

3. Activate virtual environment and install dependencies. Under Ubuntu, run:

```bash
$ sudo apt-get install virtualenv
$ virtualenv venv
$ source venv/bin/activate
$(venv) pip install -r yolov5/requirements.txt
```

You need to have python >= 3.8 to run the training. If it is not installed system-wide, you need
```bash
$ virtualenv venv --python=<path to python >= 3.8>
```

## Training

Run the following commands in the virtual environment:

```bash
$(venv) cd yolov5
$(venv) python train.py --data ../config/fruits.yaml
```

Training will take a while depending on your resources.
The output will appear in `yolov5/runs/train/exp...` directory.

The model trained on AWS instance given `datasets/fruits` training set is saved
as `model/best.pt`.

## Detection

Run the following commands in the virtual environment:

```bash
$(venv) cd yolov5
$(venv) python train.py --weights ../model/best.pt --source 0
```

This will launch the detection with the `model/best.pt` and the webcam as an input.

You may want to use another source of images/video:

|command line|comment|
|------------|-------|
|img.jpg|image|
|vid.mp4|video|
|screen|screenshot|
|path/|directory|
|list.txt|list of images|
|list.streams|list of streams|
|path/*.jpg|glob|
|https://youtu.be/...|YouTube|
|rtsp://example.com/media.mp4|RTSP, RTMP, HTTP stream|