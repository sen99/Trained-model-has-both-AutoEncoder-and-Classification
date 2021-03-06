# Trained model has both AutoEncoder and Classification

# Introduction
### Build a network that combines supervised and unsupervised architectures in one model to achieve a classification task.

# Data
### Use 50% of the following classes for training (bird, deer and truck) while you can use any percentage you think is appropriate for the other classes.

# Architecture:
### Learning in 2 steps. First, I learned autoencoder(encoder&decoder) module. Next, the detector was added in place of the decoder, and the detector was learned At this time. set requires_grad = false so as not to affect the encoder module.
![arch](https://user-images.githubusercontent.com/59045457/91385956-ebe53300-e86c-11ea-82b6-ec8166ea8aad.png)

# Performance(arranged Darknet)
### Loss of Autoencoder
#### Loss(Train_data): 0.0038, Loss(Test_data): 0.0041
![autoencoder](https://user-images.githubusercontent.com/59045457/91306546-9e72b280-e7e7-11ea-99e7-c85201f2f7f5.png)
### Autoencoder output
#### Test_data_image
![Test_data_image](https://user-images.githubusercontent.com/59045457/91306668-d2e66e80-e7e7-11ea-915c-2fa4b75bcaff.png)
#### Autoencoder_output
![Autoencoder_output](https://user-images.githubusercontent.com/59045457/91306749-f14c6a00-e7e7-11ea-80c2-59072ec1e4f5.png)

### Detector output
#### Detector loss & accuracy
#### Loss(Train_data): 0.2997, Loss(Test_data): 0.7255
![Detecter_loss](https://user-images.githubusercontent.com/59045457/91307499-fe1d8d80-e7e8-11ea-8f6e-767228119757.png)
#### Accuracy(Train_data): 0.8950, Accuracy(Test_data): 0.8095
![Detecter_acc](https://user-images.githubusercontent.com/59045457/91307502-feb62400-e7e8-11ea-914e-3f1bb58898a5.png)
#### Accuracy of each class
![accuracy of each class](https://user-images.githubusercontent.com/59045457/91306890-2c4e9d80-e7e8-11ea-9a3e-7907b94b7c6b.png)

## Why choose 2step?
### Before trained 2 step model , I trained autoencoder and detection separately. it turns out that the two modules do not need the same feature quantity.
### The detector only needs to look at the feature amount of the subject, but the auto encoder needs to learn including the background.
### In other words, the detector only needs to look at the feature amount of the subject, but the auto encoder needs to learn including the background.
### Therefore, in order to make an end-to-end system with high performance, I thought that the size of the model would be quite large.
### If it is 2 steps model, the model size will be relatively small, and at least the auto encoder module will give the same score as individual auto encoder model

## About Dataset
### Use 50% of the all classes for trainin, accuracy dropped.
### Double the size by adding augmentation while keeping the bird, deer and truck image at 50%, the accuracy increased by about 1%.

## About Data Augmentation
### It's better not to add augmentation such as random clock in auto encoder. The cause is probably like this. Crops and rotations create black areas (no image).
### The black areas is generated regardless of the surrounding image and occupies a certain proportion. This prevents loss decrease.
### In this case, the image after the auto encoder is darker than the input.
### However, since the subject has been correctly decoded, there is a possibility that it can be used for classification even as it is.
### This time I chose the one with the better loss.
### On the contrary, it is better to add augmentation in classification.
### If you do not add it, overfitting will occur at an early stage

## About Model
### I tried 4models, Dence, SimpleCNN , Darknet and arranged Darknet
### The score was arranged Darknet > Darknet> SimpleCNN > Dence
### I think that if use a larger model, the accuracy will increase, so I will do it in the future.
### But it may not reach individual learning score.
