# ECE-884-group6

# 1.Introduction

## 1.1 Background
Robotic weed/plant control, which is to use a camera-based machine vision system to detect crop plants and weeds. It helps to reduce both the labor cost and herbicide usage. The ability to conduct fast and accurate method in differentiating weed and corp seedlings can help farmers to efficiently maintain environment of farm. 

## 1.2 Dataset
Two good examples of such application are `DeepWeeds` and `Plant seedlings`.

[DeepWeeds](https://www.tensorflow.org/datasets/catalog/deep_weeds)[1] is a balanced dataset that has 17,509 labelled images of 8 nationally significant weed species native to 8 locations across northern Australia. 

[Plant seedlings](https://www.kaggle.com/c/plant-seedlings-classification)[2] is a unbalanced dataset including images of approximately 960 unique plants belonging to 12 species at several growth stages (4750 images in total). 

## 1.3 Analysis of _Plant seedlings_
### 1.3.1 Exploratory analysis
The original 4750 images was randomly splitted into train and test set with ratio 8:2. Test set was  We preprocess the training set images by resizing (150 x 150), masking, transforming to greyscale and feature standardization. By using PCA, each image was transformed to a 180-dim array, which was further reduced to 2D plot using t-SNE with parameter `perplexity=30` and can be visualized as below
<p align="center">
<img src="doc/tSNE_result.png" width="600">
</p>

From the resulting plot above, it can be observed that train set images with 12 labels are well mixed and hard to differentiate by simply projecting images to low dimension space.

### 1.3.2 Oversampling minority classes
Below is the plot of sample sizes of all 12 classes
<p align="center">
<img src="doc/sample_sizes_nooversample.png" width="600">
</p>

It can be observed from the plot that the dataset is unbalanced. With more than 600 images in class _Loose silky bent_ while only around 200 images in classes such as _Malze_.

To overcome this unbalancing issue, we tried to balance the dataset by oversampling 40~60 images from minority classes (e.g. plant _Malze_) and apply each of the following image transformations:
- Keep only red channel
- Keep only green channel
- Keep only blue channel
- Unsharp

After oversampling, the dataset is balanced as shown below:
<p align="center">
<img src="doc/sample_sizes_oversample.png" width="600">
</p>

### 1.3.3 Data augmentation
Due to limited training set of only 3800 images, we decided to enlarge the dataset with augmented images. Therefore, following the _Oversampling_ step, the dataset was enlarged with augmented images acquired by randomly drawing 300 images from each class and randomly apply one of the following transformations:
- Rotation
- Flip
- Add Gaussian noise


### 1.3.4 Classifier
We build two image classifiers `CNN` and `Inception-ResNet v2` that can label various types of plant/weed images with the highest accuracy being 93.77%.

##### CNN
`epoch=100` and `batch size=32` was used in training `CNN` model with the following structure:
<p align="center">
<img src="doc/CNN.png" width="600">
</p>

##### Inception-ResNet v2
`epoch=100` and `batch size=32` was used in training `Inception-ResNet v2` model with the following structure:
<p align="center">
<img src="doc/Inception-ResNet v2.png" width="600">
</p>

### 1.3.5 Result
Method | Test accuracy
--- | ---
CNN | 88.38%
Inception-ResNet v2 | 93.77%
CNN + Oversample | 85.21%
Inception-ResNet v2 + Oversample | 92.93%
CNN + Oversample + Data augmentation | 84.58%
Inception-ResNet v2 + Oversample + Data augmentation | 90.71%

The results shown above is anti-intuitive since performance of both `CNN` and `Inception-ResNet v2` decrease as we applied several conventional techniques to resolve unbalancing and limited sample size issues. The reason for this disappointing result is likely due to the bias of oversampled training set, based on which augmented images were generated. Such bias could be circumvented in **Oversampling** step by choosing more rigorous technique such as Synthetic Minority Oversampling Technique (SMOTE) and in **Data augmentation** step by increasing number of augmented images or using state-of-the-art algorithms such as [GAN](https://doi.org/10.1016/j.neucom.2018.09.013)[3] and [WeMix](https://arxiv.org/abs/2010.01267)[4].

## 1.4 Analysis of _DeepWeed_
### 1.4.1 Exploratory analysis
The 'DeepWeed' data set has total of 9 classes, including negatives. There are 10504 images for training, 3502 images for validation, and 3503 images for testing. The images were originally sized to 256*256, they are corped to 224*224 during the training, like what is done in the 'DeepWeed' paper
### 1.4.2  CNN balanced dataset setup
For the balance dataset, we used the same model set up with the unbalanced data set. There are total of 100 epochs was done. The batch size was seteed to 32.
<p align="center">
<img src="doc/Deep_CNN4.PNG" width="600">
<b>model summary</b>
</p>

### 1.4.3 CNN balanced dataset result
<p align="center">
<img src="doc/Deep_CNN1.PNG" width="600" ><br>
![image](https://user-images.githubusercontent.com/33790914/116741914-a3b27d80-a9c4-11eb-9718-af0bb3a26189.png)
</p>
<p align="center">
<img src="doc/Deep_CNN2.PNG" width="600"><br>
<b>accuracy test result</b>
</p>
<p align="center">
<img src="doc/Deep_CNN3.PNG" width="600"><br>
<b>scores & confusion matrix</b>
</p>
Overall, the result of the CNN classifier trained on the DeepWeed data set is not bad.

### 1.4.4 Inception balanced dataset setup
For the training using Inception-ResNet v2 on the balanced dataset 'DeepWeed'. We performed 50 epochs, and the batch size was seted to 32.

### 1.4.5 Inception balanced dataset result
<p align="center">
<img src="doc/Deep_INC1.PNG" width="600" ><br>
<b>accuracy & val_accuracy vs epochs</b>
</p>
<p align="center">
<img src="doc/Deep_INC2.PNG" width="600"><br>
<b>accuracy test result</b>
</p>
<p align="center">
<img src="doc/Deep_INC3.PNG" width="600"><br>
<b>scores & confusion matrix</b>
</p>
The result of Inception-ResNet v2 model is significanlty better than the CNN model.

### 1.4.6 Conclusion
The result is the same with our expectation. The pretrained model Inception-v2 shows significantly improvement over the CNN model. There is a 13% accuracy increase. At the same time, the Inception_v2 only trained for 50 epochs. To achieve a same or better accuracy, fine tune a pre-trained model is less time consuming than train from scrach.


# 2. Usage
Python code implementation for `DeepWeeds` dataset can be found in `Deep_CNN.ipynb` and `Deep_inception.ipynb`, for `Plant seedlings` dataset can be found in `Plant_seedling.ipynb`.


# References
[1] ???A. Olsen, D. A. Konovalov, B. Philippa, P. Ridd, J. C. Wood, J. Johns, W. Banks, B. Girgenti, O. Kenny, J. Whinney, B. Calvert, M. Rahimi Azghadi, and R. D. White, ???DeepWeeds: A Multiclass Weed Species Image Dataset for Deep Learning,??? Scientific Reports, vol. 9, no. 2058, 2 2019. [Online]. Available: https://doi.org/10.1038/s41598-018-38343-3 ???

[2] T. M. Giselsson, R. N. J??rgensen, P. K. Jensen, M. Dyrmann, and H. S. Midtiby, ???A Public Image Database for Benchmark of Plant Seedling Classification Algorithms,??? arXiv.org, 15-Nov-2017. [Online]. Available: https://arxiv.org/abs/1711.05458.

[3] Frid-Adar, M., Diamant, I., Klang, E., Amitai, M., Goldberger, J., Greenspan, H., 2018. "GAN-based synthetic medical image augmentation for increased CNN performance in liver lesion classification," arXiv:1803.01229. [Online]. Available: https://doi.org/10.1016/j.neucom.2018.09.013

[4] Yi Xu, Asaf Noy, Ming Lin, Qi Qian, Hao Li, Rong Jin, "WeMix: How to Better Utilize Data Augmentation," arXiv:2010.01267. [Online]. Available: https://arxiv.org/abs/2010.01267





