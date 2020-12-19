# Computer Vision Master's Thesis - Towards the detection of Japanese Knotweed from Multi-spectral aerial imagery

## Introduction

In my final year of my Imperial Physics degree course I got interested in the developments happening in computer vision. It was extremely exciting and could be applied to lots of problems. I devoted my Master's thesis to pursuing it further.

The project specifically was about going some way towards building a tool that would automate the detection of an invasive alien plant species Japanese Knotweed. It causes £170 million worth of damage to the UK economy each year and is a leading cause of biodiversity loss.

<p align="center"><img src="https://github.com/thomas-waite/computer-vision-thesis/blob/master/pictures/knotweed.png" width="300px"/></p>

<p align="center"><img src="https://github.com/thomas-waite/computer-vision-thesis/blob/master/pictures/knotweed-distribution.png" width="300px"/></p>

## Approach

We sought to build an end to end solution consisting of a data capture platform followed by a data analysis procedure. The data capture device was a UAV, specifically a DJI Phantom 4, which had an onboard multispectral camera attached. This would image in RGB and NIR wavelengths, allowing us to calculate the NDVI vegetative index and produce maps of this. NDVI would be used to discriminate between plants and other objects. Specifically, we used this camera from Sentera: https://sentera.com/dji-ndvi-upgrade/

<p align="center"><img src="https://github.com/thomas-waite/computer-vision-thesis/blob/master/pictures/drone.png" width="300px"/></p>

Once the data had been captured and NDVI maps produced, we then used various machine learning algorithms to begin to identify the contents of the surveyed land.

## Samples of captured data and NDVI maps

1. Raw near infrared
<p align="center"><img src="https://github.com/thomas-waite/computer-vision-thesis/blob/master/pictures/nir.png" width="300px"/></p>

2. Calculated NDVI map
<p align="center"><img src="https://github.com/thomas-waite/computer-vision-thesis/blob/master/pictures/ndvi.png" width="300px"/></p>

## Machine learning

The best performing neural networks in computer vision are convolutional neural networks (CNNs). They use convolutions for feature extraction and end with a classification step, this makes a prediction as to what class the supplied image belongs to.

Initially, we did not have labelled training data and so pursued unsupervised learning approaches.

### Unsupervised learning

We used primarily used convolutional autoencoders. These use several convolutional and pooling layers to reduce an input image down to a core latent feature representation. This is followed by a reconstructive step which uses a series of unpooling and deconvolutional layers to attempt to reproduce the original image. The model is trained by minimising the loss between the original and reproduced image.

<p align="center"><img src="https://github.com/thomas-waite/computer-vision-thesis/blob/master/pictures/convolutional-autoencoder.png" width="300px"/></p>

The idea is that once the autoencoder is sufficiently trained such that the input and output images match, then the learned intermediate representation is accurate.

Specifically, we used a W-Net.

### Example

Input RGB image:

<p align="center"><img src="https://github.com/thomas-waite/computer-vision-thesis/blob/master/pictures/input-rgb.png" width="300px"/></p>

Segmentation map:

<p align="center"><img src="https://github.com/thomas-waite/computer-vision-thesis/blob/master/pictures/segmentation1.png" width="300px"/></p>

### Supervised learning

In addition, we sourced an existing dataset on aerial images of plants from https://ieeexplore.ieee.org/document/8115245. This dataset was labelled and distinguished plants from background. We adapted our convolutional autoencoder W-Net into a U-Net - a supervised variant - and trained the network on the labelled training data.

We then produced segmentation maps and detected crops with an accuracy of 61%.

### Example

Input NDVI image:

<p align="center"><img src="https://github.com/thomas-waite/computer-vision-thesis/blob/master/pictures/input-ndvi.png" width="300px"/></p>

Output segmentation map:

<p align="center"><img src="https://github.com/thomas-waite/computer-vision-thesis/blob/master/pictures/output-segmentation.png" width="300px"/></p>
