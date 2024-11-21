<a href=https://arxiv.org/abs/2312.07381><img src="https://img.shields.io/badge/arxiv-2312.07381-orange?logo=arxiv&logoColor=white"/></a>
<a href="https://huggingface.co/spaces/halleewong/ScribblePrompt"><img alt="Spaces" src="https://img.shields.io/badge/%F0%9F%A4%97%20Hugging%20Face-Spaces-blue"></a>
<a href="https://colab.research.google.com/drive/14ExpVy3PjCCp4VzgTo27Yh_aLBafK8cX?usp=sharing"><img alt="Colab" src="https://colab.research.google.com/assets/colab-badge.svg"></a>

## Overview

**ScribblePrompt** is an interactive segmentation tool that enables users to segment *unseen* structures in medical images using scribbles, clicks, *and* bounding boxes. 

![](https://github.com/halleewong/ScribblePrompt/blob/website/assets/gifs/total_segmentator.gif)
![](https://github.com/halleewong/ScribblePrompt/blob/website/assets/gifs/wbc.gif)
![](https://github.com/halleewong/ScribblePrompt/blob/website/assets/gifs/drive.gif)
![](https://github.com/halleewong/ScribblePrompt/blob/website/assets/gifs/buid.gif)
![](https://github.com/halleewong/ScribblePrompt/blob/website/assets/gifs/hipxray.gif)
![](https://github.com/halleewong/ScribblePrompt/blob/website/assets/gifs/acdc.gif)

## Try ScribblePrompt

* Interactive [online demo](https://huggingface.co/spaces/halleewong/ScribblePrompt) on Hugging Face Spaces
* See [Installation](https://github.com/halleewong/ScribblePrompt?tab=readme-ov-file#installation) and [Getting Started](https://github.com/halleewong/ScribblePrompt?tab=readme-ov-file#getting-started) for how to run the Gradio demo locally
* Jupyter notebook [colab tutorial](https://colab.research.google.com/drive/14ExpVy3PjCCp4VzgTo27Yh_aLBafK8cX?usp=sharing)

## Models

We provide [checkpoints](https://www.dropbox.com/scl/fo/zl12obhnsqc2mq7ulviq9/h?rlkey=suaj632fd9aqd6c2gtajz1ywc&dl=0) for two versions of ScribblePrompt: 

* **ScribblePrompt-UNet** with an efficient fully-convolutional architecture  

* **ScribblePrompt-SAM** based on the [Segment Anything Model](https://github.com/facebookresearch/segment-anything)

Both models have been trained with iterative **scribbles, click, and bounding box interactions** on a diverse collection of 65 medical imaging datasets with both real and synthetic labels. 

## MedScribble Dataset

We release MedScribble, a dataset of multi-annotator scribble annotations for diverse biomedical image segmentation tasks, under [`./MedScribble`](https://github.com/halleewong/ScribblePrompt/tree/main/MedScribble). See [the readme](https://github.com/halleewong/ScribblePrompt/tree/main/MedScribble/README.md) for more info and [`./MedScribble/tutorial.ipynb`](https://github.com/halleewong/ScribblePrompt/tree/main/MedScribble/tutorial.ipynb) for a preview of the data.  

## Installation

You can install `scribbleprompt` in two ways:

* **With pip**:

```
pip install git+https://github.com/halleewong/ScribblePrompt.git
```

* **Manually**: cloning it and installing dependencies
```
git clone https://github.com/halleewong/ScribblePrompt
python -m pip install -r ./ScribblePrompt/requirements.txt
export PYTHONPATH="$PYTHONPATH:$(realpath ./ScribblePrompt)"
```

The following optional dependencies are necessary for the local demo:
```
pip install gradio==3.40.1
```

## Getting Started

First, download the model [checkpoints](https://www.dropbox.com/scl/fo/zl12obhnsqc2mq7ulviq9/h?rlkey=suaj632fd9aqd6c2gtajz1ywc&dl=0) to `./checkpoints`.

To run an interactive demo locally: 
```
python demos/app.py
```

To instantiate ScribblePrompt-UNet and make a prediction:
```
from scribbleprompt import ScribblePromptUNet

sp_unet = ScribblePromptUNet()

mask = sp_unet.predict(
    image,        # (B, 1, H, W) 
    point_coords, # (B, n, 2)
    point_labels, # (B, n)
    scribbles,    # (B, 2, H, W)
    box,          # (B, n, 4)
    mask_input,   # (B, 1, H, W)
) # -> (B, 1, H, W) 
```

To instantiate ScribblePrompt-SAM and make a prediction:
```
from scribbleprompt import ScribblePromptSAM

sp_sam = ScribblePromptSAM()

mask, img_features, low_res_logits = sp_sam.predict(
    image,        # (B, 1, H, W) 
    point_coords, # (B, n, 2)
    point_labels, # (B, n)
    scribbles,    # (B, 2, H, W)
    box,          # (B, n, 4)
    mask_input,   # (B, 1, 256, 256)
) # -> (B, 1, H, W), (B, 16, 256, 256), (B, 1, 256, 256)

```
For best results, `image` should have spatial dimensions $(H,W) = (128,128)$ and pixel values min-max normalized to the $[0,1]$ range. 

For ScribblePrompt-UNet, `mask_input` should be the logits from the previous prediction. For ScribblePrompt-SAM, `mask_input` should be `low_res_logits` from the previous prediction. 

## To Do

- [x] Release Gradio demo 
- [x] Release model code and weights
- [x] Release jupyter notebook tutorial
- [x] Release scribble simulation code
- [x] Release MedScribble dataset
- [ ] Release training code
- [ ] Release segmentation labels collected using ScribblePrompt






