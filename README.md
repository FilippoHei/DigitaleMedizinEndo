# DigitaleMedizinEndo

Multi-Class Segmentation of Cardio-Endoscopic Images

# Instruction

Step 1)
Check the requirements.txt file for dependencies. Install any required modules with pip or conda.


Step 2)
The neccessary data is available on Google Drive. Accessthe Drive via this [link](https://drive.google.com/drive/folders/1Y97hKWSQuv8j_5WADxtdRIvG8S__pHIr?usp=sharing)

Training images = './unet-nested-multiple-classification/data/images/images_training'\
Training masks = '../unet-nested-multiple-classification/data/masks/masks_grayscale'\
Training checkpoints = '../unet-nested-multiple-classification/checkpoints'


Step 3)
Resize the images to an appropriate size that your PC can handle well (see script: "./resizePNG.ipynb"). The dimensions should be divisible by 16. The original images were taken in 16:9 ratio. We chose the dimensions 768x432 to meet all requirements.


Step 4)
Some scenes were labelled with rectangular "bounding boxes". These are very complicated scenes where the instrument could not be clearly delimited in case of doubt. We decided to exclude these scenes from the training because they are not useful (see script: "./RemoveBoundingBoxImages.ipynb").


Step 5)
The masks must be conditioned for the Unet (see script: "./Convert2Grayscale.ipynb"). To do this, the classes are converted to grayscale values, and the classes are categorised hierarchically in ascending values (grayscale 0 = background; grayscale 1 = class 1; grayscale 2 = class 2 etc.). 

Since the grayscales are very low, they are not differentiable to the human eye and appear as black images. 

The classes are organised as follows:\
    Class 0 = Background\
    Class 1 = Atraum. Pinzette\
    Class 2 = Nadelhalter\
    Class 3 = Klappenschere\
    Class 4 = Knotenschieber\
    Class 5 = Nervhaken


Step 6)
In order to also integrate empty scenes, but to avoid the overrepresentation of empty images (in about 70% of the scenes no instruments are visible), we decided to also integrate empty masks for 20% of the scenes that actually have instruments in them. To do this, empty (black) masks must first be generated (see script: "./AddEmptyMasks.ipynb").


Step 7)
The previous steps have already been completed. Now you can start training (see script: "./Colab_Surgical_Instrument_Segmentation_MultiInstance.ipynb"). For this we have been inspired by zonasw's U-Net architecture for multi-class segmentation (see here: https://github.com/zonasw/unet-nested-multiple-classification), which itself builds on the findings of Ronneberger et al (see: U-Net: Convolutional Networks for Biomedical Image Segmentation - https://doi.org/10.1007/978-3-319-24574-4_28). 

The images are downloaded from the mentioned Google Drive. To adjust the hyperparameters (e.g. batchsize, number of classes), change entries in the config.py file.


Step 8)
The network can be tested afterwards for its performance. To do this, call the script "./TestNetwork.ipynb" and set the sessions of interest according to the two sessions that were not trained with. The script calculates the cross ENtropy of the network as well as averaged dice and intersection over union scores. 


Step 9)
To determine the inter-observer variability, call the script "./InterObserverVariability.ipynb". Two folders with the same images must be created ("./data/InterObserver/Observer_01" and "./data/InterObserver/Observer_02"). The script calculates the averaged dice and intersection over union scores. 

