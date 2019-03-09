---
layout: single
title: "Masking Microbes"
excerpt: "Using Mask R-CNN to find microbe colonies."
header:
  image: /assets/images/microbes/microbe-header.png
author_profile: true
sidebar:
  - text: "<a href='https://github.com/ZoeKoch/microbe-masking/blob/master/inspect_balloon_model.ipynb'>View the project's code</a>"
toc: true
tags:
  - neural network
  - image analysis
  - instance segmentation
  - Python
petrigallery:
  - image_path: /assets/images/microbes/macconkey.png
  - image_path: /assets/images/microbes/blood.png
labelgallery:
  - image_path: /assets/images/microbes/labeling.png
  - image_path: /assets/images/microbes/labeled.png
masksgallery:
  - image_path: /assets/images/microbes/mask1.png
  - image_path: /assets/images/microbes/mask2.png 
  - image_path: /assets/images/microbes/mask3.png 
---

# Instance Segmentation in Urine Cultures

## Introduction

Urinary tract infections are extremely common, with a lifetime risk estimated to be as high as 1 in 2. Roughly 150 million UTIs occur world wide yearly, costing about $6 billion in healthcare expenditures. However, the symptoms of urinary tract infections (namely, pain upon urination) are not unique to UTIs, alone. Many of the symptoms associated with UTIs, which are caused primarily by e.coli, are also common with yeast infections, chlamydia, herpes, gonorrhea, or vaginitis. These diseases require their own specific medication in order to be effectively treated.[^1] Prescribing the appropriate medication is also particularly important to limit the growth and spread of drug-resistant strains of harmful microorganisms. 
Currently, urine cultures can be used to screen for all of the mentioned pathogens. The patient submits a clean-catch urine sample, which is deposited on a petri dish, and cultured for at least 24 hours. Then a trained medical professional must analyze the sample and identify the bacteria and/or yeast strains that have grown by comparing them to referential images. After identifying the microbe type, the number of colonies on a small section of the Petri dish are measured in order to produce an estimate of how many bacteria exist in the urine, and, correspondingly, the severity of the infection.
Here, we show that Mask R-CNN has potential for use in not only classifying the type of microbe growing in a Petri dish and the number of colonies, but also finding where the colonies exist on a pixel level.

## Problem Definition and Algorithm 

### Task Definition

The University of Vermont Medical Center has taken 83 images of urine cultures grown on both MacConkey and blood agar, with microbe types classified by a trained medical professional. Images are taken using a bucket of light approach for better quality.[^2]  

{% include gallery id="petrigallery" caption = "Examples of images in the dataset. The MacConkey agar is pink, while the blood is red."%}

Each Petri dish picture was split 8x8 to make 64 smaller images, for easier annotation and to reduce the ratio between colony size and image size. Images were then annotated by hand using VGG Image Annotator.

{% include figure image_path="/assets/images/microbes/labeling.png" caption = "The VGG Image Annotator interface."%}
{% include figure image_path="/assets/images/microbes/labeled.png" caption = "Creating masks to use for training."%}

Although, ultimately, only the microbe type and microbe count are used for medical diagnoses, it may be valuable in future research to also be able to segment each instance of a microbe colony. This data can then be used to model how bacteria grow and compete for resources in vitro. Therefore, this is not only a classification or counting problem, but an instance segmentation problem.

We wish to use machine learning techniques to classify each microbe colony based on organism type, place bounding boxes around each colony, and predict at the pixel level where each colony is located in relation to the background agar.

### Algorithm Definition

Mask R-CNN, developed by Kaiming He, Georgia Gkioxari, Piotr Dollar, and Ross Girshick, can accomplish all three of our goals for instance segmentation (classification, bounding boxes, and masking).[^3] Mask R-CNN extends Faster R-CNN by adding a branch for predicting segmentation masks on each region of interest, in parallel with the existing branch for classification and bounding box regression. Classification doesn’t depend on mask predictions, but happens in parallel. The mask branch is a small fully connected network applied to each RoI, predicting a segmentation mask in a pixel-to-pixel manner. This type of architecture is also highly flexible, and results may be improved by changing the underlying architecture.

{% include figure image_path="/assets/images/microbes/maskrcnn.png"%}

## Experimental Evaluation 

### Methodology

Each Petri dish picture was split into 64 smaller images, for easier annotation and to reduce the ratio between colony size and image size. Half of each culture picture is used for training, a quarter for validation, and a quarter for testing. Due to sparseness of data and time constraints, only MacConkey-grown cultures of the 4 types acinetobacter baumanni complex, enterobacter cloacae complex, escherichia coli, and klebsiella pneumoniae were used. Pixel-by-pixel labels of where microbe colonies are are labeled by hand. The boxes around each microbe colony are generated from those annotations. I then used Matterport’s Mask R-CNN model, starting with weights pretrained on the COCO dataset.[^4] Since this model was already quite well trained, a slow learning rate was used.

{% include figure image_path="/assets/images/microbes/microbes-table.png"%}

### Results

Accurately classifying and counting the number of colonies is the most important task, and fortunately, also the easiest to measure. A simple precision metric can be used for both. It’s important to note that the pixel-by-pixel mask that is used to train the model is generated by a person, and therefore is, itself, fairly inaccurate. Densely packed microbe colonies were difficult to separate into individual colonies, even for hulmans. Comparing the mask that the model generates to that generated by a person therefore would not be the best way to measure accuracy. 
Currently, this methodology is not classifying up to a medical standard. The classification success is less than 85%, making it worse than patient data such as age and gender at predicting microbe type. Therefore, multiple models were trained, specific to each microbe class. Average precision scores for these models ranged from 62 to 75, while average error count per image was quite low. However, many of these images had no microbe colonies at all, which likely deflated the error count. 

{% include figure image_path="/assets/images/microbes/mask1.png"%}
{% include figure image_path="/assets/images/microbes/mask2.png"%}
{% include figure image_path="/assets/images/microbes/mask3.png" caption = "Examples of the masks produced" %}

### Discussion
Classification of microbes is a difficult task for current CNNs. They function on the premise that the visible region of the instance of interest contains the necessary information to classify it. However, for individual microbe colonies, the surrounding colonies are often just as important in identifying what they are. Microbes vary in structure based on stress, crowdedness, etc. These images were taken after being grown in a temperature-controlled, nutrient-rich environment for 48 hours, and therefore have more uniformity than is typical for microbes, but were still difficult to classify. Human beings can visually choose the most distinctively easy to classify sections of the Petri dish, and make judgments from there. Algorithms that take context into account, such as proximity to particular classes known to be frequently nearby, may have more success. Microbial classification, counting, and segmentation can be automated with machine learning techniques. Mask R-CNN has much potential in this area.

## References

[^1]: Paolo Andreini, Simone Bonechi, Monica Bianchini, Andrea Garzelli, Alessandro Mecocci, Automatic image classification for the urinoculture screening, In Computers in Biology and Medicine, Volume 70, 2016, Pages 12-22, ISSN 0010-4825, https://doi.org/10.1016/j.compbiomed.2015.12.025. Free access on http://www.sciencedirect.com/science/article/pii/S0010482516000020
  
[^2]: J.S. Parkinson. A “Bucket of Light” for viewing bacterial colonies in soft agar. Methods 	Enzymol, 423 (2007), pp. 432-435

[^3]:  K. He, G. Gkioxari, P. Dollar, and R. Girshick. Mask R-CNN. arXiv:1703.06870, 2017.

[^4]: https://engineering.matterport.com/splash-of-color-instance-segmentation-with-mask-r-cnn-and-tensorflow-7c761e238b46

[^5]: Redmon, J., Divvala, S., Girshick, R., Farhadi, A.: You only look once: Unified, real-time object detection. In: CVPR. (2016)
