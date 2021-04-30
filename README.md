# ECE-884-group6

# 1.Introduction

## 1.1 Background
Robotic weed/plant control, which is to use a camera-based machine vision system to detect crop plants and weeds. It helps to reduce both the labor cost and herbicide usage. The ability to conduct fast and accurate method in differentiating weed and corp seedlings can help farmers to efficiently maintain environment of farm. 

## 1.2 Dataset
Two good examples of such application are _DeepWeeds_ and _Plant seedlings_.
The _DeepWeeds_ is a balanced dataset (_https://www.tensorflow.org/datasets/catalog/deep_weeds_) that has 17,509 labelled images of 8 nationally significant weed species native to 8 locations across northern Australia. 

_Plant seedlings_ is a unbalanced dataset (_https://www.kaggle.com/c/plant-seedlings-classification_) including images of approximately 960 unique plants belonging to 12 species at several growth stages (4750 images in total). 

## 1.3 Analysis of _Plant seedlings_
### 1.3.1 Exploratory analysis
We preprocess the 4750 images by resizing (150 x 150), masking, transforming to greyscale and feature standardization. By using PCA, each image was transformed to a 180-dim array, which was further reduced to 2D plot using t-SNE with parameter [perplexity=30]
### 1.3.1 Oversampling minority classes

### 1.3.2 Data augmentation

### 1.3.3 Classifier
To achieve the goal of correctly classifying an unseen plant/weed to one of 12/8 species, we build image classifiers that can label various types of plant/weed images correctly. In this study, we will consider two sets of image datasets, one balanced and one unbalanced.

## 1.4 Analysis of _DeepWeed_


# Usage


# Results

The results show that classification accuracy of both CNN and inception-ResNetv2 decreased after applying data augmentation technique. The reason for this disappointing result is likely due to bias of augmented images, which could be possibly corrected by increase number of augmented images or using WeMix[1].

# References
[1] Xu. et al, “WeMix: How to Better Utilize Data Augmentation” arXiv:2010.01267 [cs], Oct. 2020.



