# Survey-DLCPS

## Deep Learning Methods for Calibrated Photometric Stereo and Beyond: A Survey

## Introduction
<p align="center">
    <img src='imgs/ps.png' width="300" >
</p>
Photometric stereo methods obtain detailed shape reconstructions from multiple images under different illuminations. The orange box shows the general non-Lambertian surface reflectance. To solve the non-Lambertian surface, many methods have addressed non-Lambertian photometric stereo. This paper focuses on deep learning-based calibrated photometric stereo methods and provides a comprehensive review. 

**The purpose of this project is to collect and update deep learning-based calibrated photometric stereo methods continuously.**
<br>

## Categorization Based on Input Processing
The first deep learning method, DPSN [[paper](pdfs/1DPSN.pdf), [Code](https://github.com/hiroaki-santo/deep-photometric-stereo-network)] makes the order of illuminations and the number of input images unchanged, by a seven-layer fully-connected network. To handle a varying number of inputs during training and testing, many methods were then proposed. In this section, we classify these methods based on how they process the inputs.
<br>

## Per-pixel methods
The per-pixel strategy is first implemented using the observation map in CNN-PS [[paper](pdfs/CNNPS.pdf), [code](https://github.com/satoshi-ikehata/CNN-PS-ECCV2018)].
<br>
<p align="center">
    <img src='imgs/om.png' width="300" >
</p>
The observation map aggregates the corresponding pixels from each input image into a fixed-size observation map using the 2D coordinates of the projected normalized lighting directions (along the axis-z direction). Here, a, b, and c stand for the number of input images (lights), while 1, 2, and 3 stand for the index of pixel position. 
<br>

### Problem of sparse input
Some works were proposed to solve the sparse input images problem, such as SPLINE-Net [[paper](pdfs/SPLINE.pdf), [code](https://github.com/yiming-j/SPLINE-Net)] and LMPS [[paper](pdfs/LMPS.pdf), [code]([https://github.com/satoshi-ikehata/CNN-PS-ECCV2018](https://github.com/junxuan-li/Learning-to-Minify-Photometric-Stereo)https://github.com/junxuan-li/Learning-to-Minify-Photometric-Stereo)]. These two methods adopt opposite strategies to solve the sparse inputs. SPLINE-Net proposes a lighting interpolation network to generate dense lighting observation maps when the input is sparse. LMPS reduces the demands on the number of images by only learning the critical illumination conditions via a connection table.
<br>
### Problem of global information

Original per-pixel methods designed to represent a single pixel, don't explicitly incorporate information from the surrounding pixel neighborhood. PX-Net [[paper](pdfs/PXNET.pdf), [code]()] proposed an observation map-based method that considers global illumination effects, such as self-reflections, surface discontinuity, and ambient light, which enables global information to be embedded in the per-pixel generation process.
<br>

## All-pixel methods
All-pixel methods keep all the pixels together, having the advantage of exploring intra-image intensity variations across an entire input image. The original all-pixel method was introduced in PS-FCN [[paper](pdfs/PSFCN.pdf), [code](https://github.com/guanyingc/PS-FCN)] through the use of a max-pooling layer, which operates in the channel dimension and fuses features from an arbitrary number of inputs.

### Problem of spatially varying BRDF 
Since all-pixel methods leverage convolutional networks to process input in a patch-based manner, they may have difficulties in dealing with steep color changes caused by surfaces with spatially varying materials. PS-FCN (Norm.)  [[paper](pdfs/PSFCNN.pdf), [code](https://github.com/guanyingc/PS-FCN)] proposed an observation normalization method to eliminate the impact of changing albedo. NormAttention-PSN [[paper](pdfs/NAPSN.pdf), [code](https://github.com/Kelvin-Ju/NormAttention-PSN)] further solve the normalization problem under strong non-Lambertian surfaces.

### Problem of blurry details
All-pixel methods may cause blurred reconstructions in complex-structured regions mainly because the widely used Euclidean-based loss functions can hardly constrain the high-frequency (i.e., complex-structured) representations, because of the “regression-to-the-mean” problem. Attention-PSN [[paper](pdfs/ATTPSN.pdf), [code](https://github.com/Kelvin-Ju/NormAttention-PSN)] and NormAttention-PSN [[paper](pdfs/NAPSN.pdf), [code](https://github.com/Kelvin-Ju/NormAttention-PSN)] proposed an attention-weighted loss to produce detailed reconstructions, which learn an adaptive weight of detail-preserving gradient loss for high-frequency regions.

### Problem of fusion efficiency
The fusion mechanism of all-pixel methods,i.e., max-pooling, discard a large number of features from the input, reducing the utilization of information and affecting the estimation accuracy. MF-PSN [[paper](pdfs/MF-PSN.pdf), [code](https://github.com/Kelvin-Ju/MF-PSN)] introduces a multi-feature fusion network, utilizing max-pooling operations at different feature levels in both shallow and deep layers to capture richer information. CHR-PSN [[paper](pdfs/CHRPSN.pdf), [code]()] extend max-pooling at various scales with different receptive fields, rather than the depth. HPS-Net [[paper](pdfs/HPSNET.pdf), [code]()] introduces a bilateral extraction module that outputs positive and negative information before aggregation to better preserve useful data.

## Hybrid methods
Hybrid approaches that combine these strategies may have the benefits of both per-pixel and all-pixel techniques. MT-PS-CNN [[paper](pdfs/MTPSCNN.pdf), [code]()] proposes a two-stage photometric stereo model to construct inter-frame (per-pixel) and intra-frame (all-pixel) representations. 
HT21 [[paper](pdfs/HT21.pdf), [code]()] built upon the observation maps but incorporated spatial information using 2D and 4D separable convolutions to better capture global effects. Similarly, PSMF-PSN [[paper](pdfs/HT21.pdf), [code]()] introduced a tandem manner for per-pixel and all-pixel feature extraction. GPS-Net [[paper](pdfs/GPSNET.pdf), [code](https://github.com/ZhuokunYao/GPS_NET)] introduced a structure-aware graph convolutional network \cite{chang2018structure} to establish connections between an arbitrary number of observations per pixel.
