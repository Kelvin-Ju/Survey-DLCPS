# Survey-DLCPS

## Deep Learning Methods for Calibrated Photometric Stereo and Beyond: A Survey

## Introduction
<p align="center">
    <img src='imgs/ps.png' width="400" >
</p>
Photometric stereo methods obtain detailed shape reconstructions from multiple images under different illuminations. The orange box shows the general non-Lambertian surface reflectance. To solve the non-Lambertian surface, many methods have addressed non-Lambertian photometric stereo. This paper focuses on deep learning-based calibrated photometric stereo methods and provides a comprehensive review. 

**The purpose of this project is to continuously collect and update deep learning-based calibrated photometric stereo methods.**
<br>

## Categorization Based on Input Processing
The first deep learning method, DPSN [[paper](pdfs/1DPSN.pdf), [Code](https://github.com/hiroaki-santo/deep-photometric-stereo-network)] makes the order of illuminations and the number of input images unchanged, by a seven-layer fully-connected network. To handle a varying number of inputs during training and testing, many methods were then proposed. In this section, we classify these methods based on how they process the inputs.
<br>

## Per-pixel methods
The per-pixel strategy is first implemented using the observation map in CNN-PS [[paper](pdfs/CNNPS.pdf), [code](https://github.com/satoshi-ikehata/CNN-PS-ECCV2018)], via the observation map.


