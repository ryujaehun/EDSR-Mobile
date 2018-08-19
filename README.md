# EDSR with depthwise separable convolution

![](/figs/main.png)
![](https://i1.wp.com/deepmi.me/wp-content/uploads/2017/12/unnamed-file.jpeg)
## About
This repository is the repository that applies the Depthwise Separable convolution to EDSR.
The size of the feature map and the size of the residual block are significantly reduced to accommodate the number of parameters.
Since the MSE loss is better than the L1 loss from the time when the parameter is reduced, MSE is adopted.
## Dependencies
* Python (Tested with 3.6)
* PyTorch >= 0.4.0
* numpy
* **imageio**
* matplotlib
* tqdm


## Code
Clone this repository into any place you want.
```bash
git clone https://github.com/ryujaehun/EDSR-PyTorch
cd EDSR-PyTorch
```



## Quick start (Demo)
You can test our super-resolution algorithm with your own images. Place your images in ``test`` folder. (like ``test/<your_image>``) We support **png** and **jpeg** files.

Run the script in ``code`` folder. Before you run the demo, please uncomment the appropriate line in ```demo.sh``` that you want to execute.
```bash
cd code       # You are now in */EDSR-PyTorch/code
sh demo.sh
```


| Model | Scale | Dataset | Parameters |Loss | Filter | ResBlock |****PSNR** |
|  ---  |  ---  | ---       | ---     | --- | ---    | --- |---  |
| **EDSR** | 2 | Set5 | 3.5K | MSE | 8 | 2 | 36.119 dB |
| **EDSR** | 2 | Set14 | 3.5K | MSE | 8 | 2 | 32.178 dB |
| **EDSR** | 2 | B100 | 3.5K | MSE | 8 | 2 | 31.084 dB |
| **EDSR** | 2 | Urban100 | 3.5K | MSE | 8 | 2 | 28.884 dB |
| **EDSR** | 2 | Set5 | 3.5K | L1 | 8 | 2 | 35.222 dB |
| **EDSR** | 2 | Set14 | 3.5K | L1 | 8 | 2 | 31.515 dB |
| **EDSR** | 2 | B100 | 3.5K | L1 | 8 | 2 | 30.853 dB |
| **EDSR** | 2 | Urban100 | 3.5K | L1 | 8 | 2 | 27.945 dB |
| **EDSR** | 2 | Set5 | 62.1K | MSE | 32 | 8 | 37.19 dB |
| **EDSR** | 2 | Set14 | 62.1K | MSE | 32 | 8 | 32.873 dB |
| **EDSR** | 2 | B100 | 62.1K | MSE | 32 | 8 | 31.694 dB |
| **EDSR** | 2 | Urban100 | 62.1K | MSE | 32 | 8 | 30.353 dB |
| **EDSR** | 2 | Set5 | 62.1K | L1 | 32 | 8 | 37.246 dB |
| **EDSR** | 2 | Set14 | 62.1K | L1 | 32 | 8 | 32.854 dB |
| **EDSR** | 2 | B100 | 62.1K | L1 | 32 | 8 | 31.662 dB |
| **EDSR** | 2 | Urban100 | 62.1K | L1 | 32 | 8 | 30.329 dB |

*Baseline models are in ``experiment/model``. Please download our final models from [here](https://cv.snu.ac.kr/research/EDSR/model_pytorch.tar) (542MB)
**We measured PSNR using DIV2K 0801 ~ 0900, RGB channels, without self-ensemble. (scale + 2) pixels from the image boundary are ignored.

You can evaluate your models with widely-used benchmark datasets:

[Set5 - Bevilacqua et al. BMVC 2012](http://people.rennes.inria.fr/Aline.Roumy/results/SR_BMVC12.html),

[Set14 - Zeyde et al. LNCS 2010](https://sites.google.com/site/romanzeyde/research-interests),

[B100 - Martin et al. ICCV 2001](https://www2.eecs.berkeley.edu/Research/Projects/CS/vision/bsds/),

[Urban100 - Huang et al. CVPR 2015](https://sites.google.com/site/jbhuang0604/publications/struct_sr).

For these datasets, we first convert the result images to YCbCr color space and evaluate PSNR on the Y channel only. You can download [benchmark datasets](https://cv.snu.ac.kr/research/EDSR/benchmark.tar) (250MB). Set ``--dir_data <where_benchmark_folder_located>`` to evaluate the EDSR and MDSR with the benchmarks.

## How to train EDSR and MDSR
We used [DIV2K](http://www.vision.ee.ethz.ch/%7Etimofter/publications/Agustsson-CVPRW-2017.pdf) dataset to train our model. Please download it from [here](https://cv.snu.ac.kr/research/EDSR/DIV2K.tar) (7.1GB).

Unpack the tar file to any place you want. Then, change the ```dir_data``` argument in ```code/option.py``` to the place where DIV2K images are located.

We recommend you to pre-process the images before training. This step will decode all **png** files and save them as binaries. Use ``--ext sep_reset`` argument on your first run. You can skip the decoding part and use saved binaries with ``--ext sep`` argument.

If you have enough RAM (>= 32GB), you can use ``--ext bin`` argument to pack all DIV2K images in one binary file.

You can train EDSR and MDSR by yourself. All scripts are provided in the ``code/demo.sh``. Note that EDSR (x3, x4) requires pre-trained EDSR (x2). You can ignore this constraint by removing ```--pre_train <x2 model>``` argument.

```bash
cd code       # You are now in */EDSR-PyTorch/code
sh demo.sh
```

## TODO
* visdom

**Update log**
* Aug 19, 2018
* Application of Depthwise Separable Convolution and Prediction of Trainng Time
