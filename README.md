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
The first deep learning method, DPSN [[paper](https://openaccess.thecvf.com/content_ICCV_2017_workshops/w9/html/Santo_Deep_Photometric_Stereo_ICCV_2017_paper.html), [Code](https://github.com/hiroaki-santo/deep-photometric-stereo-network)] makes the order of illuminations and the number of input images unchanged, by a seven-layer fully-connected network. To handle a varying number of inputs during training and testing, many methods were then proposed. In this section, we classify these methods based on how they process the inputs.
<br>

## Per-pixel methods
The per-pixel strategy is first implemented using the observation map in CNN-PS [[paper](https://openaccess.thecvf.com/content_ECCV_2018/html/Ikehata_CNN-PS_CNN-based_Photometric_ECCV_2018_paper.html), [code](https://github.com/satoshi-ikehata/CNN-PS-ECCV2018)].
<br>
<p align="center">
    <img src='imgs/om.png' width="300" >
</p>
The observation map aggregates the corresponding pixels from each input image into a fixed-size observation map using the 2D coordinates of the projected normalized lighting directions (along the axis-z direction). Here, a, b, and c stand for the number of input images (lights), while 1, 2, and 3 stand for the index of pixel position. 
<br>

### Problem of sparse input
Some works were proposed to solve the sparse input images problem, such as SPLINE-Net [[paper](https://openaccess.thecvf.com/content_ICCV_2019/html/Zheng_SPLINE-Net_Sparse_Photometric_Stereo_Through_Lighting_Interpolation_and_Normal_Estimation_ICCV_2019_paper.html), [code](https://github.com/yiming-j/SPLINE-Net)] and LMPS [[paper](https://openaccess.thecvf.com/content_CVPR_2019/html/Li_Learning_to_Minify_Photometric_Stereo_CVPR_2019_paper.html), [code]([https://github.com/satoshi-ikehata/CNN-PS-ECCV2018](https://github.com/junxuan-li/Learning-to-Minify-Photometric-Stereo)https://github.com/junxuan-li/Learning-to-Minify-Photometric-Stereo)]. These two methods adopt opposite strategies to solve the sparse inputs. SPLINE-Net proposes a lighting interpolation network to generate dense lighting observation maps when the input is sparse. LMPS reduces the demands on the number of images by only learning the critical illumination conditions via a connection table.
<br>
### Problem of global information

Original per-pixel methods designed to represent a single pixel, don't explicitly incorporate information from the surrounding pixel neighborhood. PX-Net [[paper](https://openaccess.thecvf.com/content/ICCV2021/html/Logothetis_PX-NET_Simple_and_Efficient_Pixel-Wise_Training_of_Photometric_Stereo_Networks_ICCV_2021_paper.html), [code]()] proposed an observation map-based method that considers global illumination effects, such as self-reflections, surface discontinuity, and ambient light, which enables global information to be embedded in the per-pixel generation process.
<br>

## All-pixel methods
All-pixel methods keep all the pixels together, having the advantage of exploring intra-image intensity variations across an entire input image. The original all-pixel method was introduced in PS-FCN [[paper](https://openaccess.thecvf.com/content_ECCV_2018/html/Guanying_Chen_PS-FCN_A_Flexible_ECCV_2018_paper.html), [code](https://github.com/guanyingc/PS-FCN)] through the use of a max-pooling layer, which operates in the channel dimension and fuses features from an arbitrary number of inputs.

### Problem of spatially varying BRDF 
Since all-pixel methods leverage convolutional networks to process input in a patch-based manner, they may have difficulties in dealing with steep color changes caused by surfaces with spatially varying materials. PS-FCN (Norm.)  [[paper](https://ieeexplore.ieee.org/abstract/document/9127824/), [code](https://github.com/guanyingc/PS-FCN)] proposed an observation normalization method to eliminate the impact of changing albedo. NormAttention-PSN [[paper](https://link.springer.com/article/10.1007/s11263-022-01684-8), [code](https://github.com/Kelvin-Ju/NormAttention-PSN)] further solved the normalization problem under strong non-Lambertian surfaces.

### Problem of blurry details
All-pixel methods may cause blurred reconstructions in complex-structured regions mainly because the widely used Euclidean-based loss functions can hardly constrain the high-frequency (i.e., complex-structured) representations, because of the “regression-to-the-mean” problem. Attention-PSN [[paper](https://www.ijcai.org/proceedings/2020/0097), [code](https://github.com/Kelvin-Ju/NormAttention-PSN)] and NormAttention-PSN [[paper](https://link.springer.com/article/10.1007/s11263-022-01684-8), [code](https://github.com/Kelvin-Ju/NormAttention-PSN)] proposed an attention-weighted loss to produce detailed reconstructions, which learn an adaptive weight of detail-preserving gradient loss for high-frequency regions.

### Problem of fusion efficiency
The fusion mechanism of all-pixel methods,i.e., max-pooling, discard a large number of features from the input, reducing the utilization of information and affecting the estimation accuracy. MF-PSN [[paper](https://www.sciencedirect.com/science/article/abs/pii/S0262885621002730), [code](https://github.com/Kelvin-Ju/MF-PSN)] introduces a multi-feature fusion network, utilizing max-pooling operations at different feature levels in both shallow and deep layers to capture richer information. CHR-PSN [[paper](https://link.springer.com/article/10.1007/s41095-021-0223-y), [code]()] extend max-pooling at various scales with different receptive fields, rather than the depth. HPS-Net [[paper](https://ieeexplore.ieee.org/abstract/document/10095806), [code]()] introduces a bilateral extraction module that outputs positive and negative information before aggregation to better preserve useful data.

## Hybrid methods
Hybrid approaches that combine these strategies may have the benefits of both per-pixel and all-pixel techniques. MT-PS-CNN [[paper](https://www.sciencedirect.com/science/article/pii/S0143816621003080?casa_token=UKkfqakA8DwAAAAA:ifZPYxiGdsowHGRYO8-DuV5mXq7_2xzdLaQ2CoSHaq4OHxIa15SSfpDonuZCzUzGe9G9ZHee5G8), [code]()] proposes a two-stage photometric stereo model to construct inter-frame (per-pixel) and intra-frame (all-pixel) representations. 
HT21 [[paper](https://ieeexplore.ieee.org/abstract/document/9665914?casa_token=67ZyDkNQYJ8AAAAA:YUtQzbNdkUVeBwaMCR2R_zZIXhGJV80rsK3wfUj_UxNAGCvNSOSZ_0DQ_cq5crd2N4TKZnOW9io), [code]()] built upon the observation maps but incorporated spatial information using 2D and 4D separable convolutions to better capture global effects. Similarly, PSMF-PSN [[paper](https://ieeexplore.ieee.org/abstract/document/10301617?casa_token=uRVoAXmSjnEAAAAA:4KtyctkCPrLJi0Kr9oQTNPS3k3-XwaKWZ0sSpYoC4I_U0Q8D6uuZYLCAVbwfgGElWlvp07EmS1E), [code]()] introduced a tandem manner for per-pixel and all-pixel feature extraction. GPS-Net [[paper](https://proceedings.neurips.cc/paper/2020/hash/7503cfacd12053d309b6bed5c89de212-Abstract.html), [code](https://github.com/ZhuokunYao/GPS_NET)] introduced a structure-aware graph convolutional network \cite{chang2018structure} to establish connections between an arbitrary number of observations per pixel.

## Categorization Based on Network Architectures

Most of the deep learning-based calibrated photometric stereo networks are based on convolutional networks, some advanced architectures are widely used, such as ResNet, DenseNet, and HR-Net. In recent years, Transformer with a self-attention module was also employed in the context of photometric stereo. SPS-Net [[paper](https://ieeexplore.ieee.org/abstract/document/9310261?casa_token=4HSHz4ve0V8AAAAA:1y56NDUqc23qLi1ql-izOTesbntsgmdkf13RB50TS659iP2nGmpmk0hzxMWH0QoUsv9fa1XzeKQ), [code]()] is the first to propose a self-attention photometric stereo network, which aggregates photometric information through a self-attention mechanism.  PS-Transformer  [[paper](https://www.bmvc2021-virtualconference.com/assets/papers/0319.pdf), [code](https://github.com/satoshi-ikehata/PS-Transformer-BMVC2021)]  then designed a dual branch to explore pixel and image-wise feature for sparse photometric stereo images.
