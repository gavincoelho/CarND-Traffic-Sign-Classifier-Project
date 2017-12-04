# **Traffic Sign Recognition**

The goal for this project was to build a traffic sign classifier by training a convolutional nerual network to identify  German traffic signs from the supplied data set.

The project Jupyter notebook can be found on [GitHub](https://github.com/gavincoelho/CarND-Traffic-Sign-Classifier-Project/blob/master/Traffic_Sign_Classifier.ipynb).

The report consists of the following topics:

* [Loading and Exploring The Dataset](#loading-and-exploring-the-dataset)
* [Designing, Training & Testing the Model Architecture](#designing,-training-&-esting-the-model-architecture)
* [Testing the Model with New Images](#testing-the-model-with-new-images)
* [Analyzing the Softmax Probabilities](#analyzing-the-softmax-probabilities)
* [Summary](#summary)


[//]: # (Image References)

[image1]: ./images/image1.png "Number of Training Images for Each Class"
[image2]: ./images/image2.png "Sample Images"
[image3]: ./images/image3.png "Initial and Normalised Image"
[image4]: ./images/image4.png "Accuracy and Loss Plot"
[image5]: ./images/image5.png "Traffic Sign 2"
[image6]: ./images/image6.png "Traffic Sign 3"
[image7]: ./images/image7.png "Traffic Sign 4"
[image8]: ./images/image8.png "Traffic Sign 5"

## Loading and Exploring The Dataset

### Loading the Data
Three seperate data files containing training, validation and test datasets were loaded and unpickled. The image data and labels were then extracted for each set. 

Next the data was analysed to determine the number of samples in each set, the image shape, the number of channels in each image and the total number of classes.

	Number of training examples   = 34799
	Number of validation examples = 4410
	Number of testing examples    = 12630
	Image data shape   = (32, 32, 3)
	Number of channels = 3
	Number of classes  = 43
	
	Mapping from Class ID to Sign Name
	--------------------------------------------------------------
	    ClassId                                           SignName
	0         0                               Speed limit (20km/h)
	1         1                               Speed limit (30km/h)
	2         2                               Speed limit (50km/h)
	3         3                               Speed limit (60km/h)
	4         4                               Speed limit (70km/h)
	5         5                               Speed limit (80km/h)
	6         6                        End of speed limit (80km/h)
	7         7                              Speed limit (100km/h)
	8         8                              Speed limit (120km/h)
	9         9                                         No passing
	10       10       No passing for vehicles over 3.5 metric tons
	11       11              Right-of-way at the next intersection
	12       12                                      Priority road
	13       13                                              Yield
	14       14                                               Stop
	15       15                                        No vehicles
	16       16           Vehicles over 3.5 metric tons prohibited
	17       17                                           No entry
	18       18                                    General caution
	19       19                        Dangerous curve to the left
	20       20                       Dangerous curve to the right
	21       21                                       Double curve
	22       22                                         Bumpy road
	23       23                                      Slippery road
	24       24                          Road narrows on the right
	25       25                                          Road work
	26       26                                    Traffic signals
	27       27                                        Pedestrians
	28       28                                  Children crossing
	29       29                                  Bicycles crossing
	30       30                                 Beware of ice/snow
	31       31                              Wild animals crossing
	32       32                End of all speed and passing limits
	33       33                                   Turn right ahead
	34       34                                    Turn left ahead
	35       35                                         Ahead only
	36       36                               Go straight or right
	37       37                                Go straight or left
	38       38                                         Keep right
	39       39                                          Keep left
	40       40                               Roundabout mandatory
	41       41                                  End of no passing
	42       42  End of no passing by vehicles over 3.5 metric ...

### Data Analysis
A graph was plotted showing the number of samples for each class in the training set. From the plot it can be seen that some signs have many more samples than others. The lack of data for certain signs could make it harder to train the network to recognize these signs. If however the number of samples of each sign corresponds to the probability of encountering that sign on the road then the we don't want a more equal sample number as this will not favor the more common signs. The data could be augmented to supply additional training samples to improve the results of the network.

![Bar-graph Showing the Number of Training Images for Each Class][image1]

### Viewing the Data
Finally a sample of each image class was plotted with it's corresponding label. From the sample images it can be seen that some images are very dark making it hard to identify the sign. Preprocessing the images by adjusting brightness and contrast could help the network perform better. 

![Sample of Images In the Training Dataset][image2]

## Designing, Training & Testing the Model Architecture

### Preprocessing
The data was preprocessed by normalizing it. The results of the normalisation process on the images can be seen below.

![Before and After Preprocessing][image3]

### Model Architecture
My final model consisted of the following layers:

| Layer         		|     Description	        					| 
|:---------------------:|:---------------------------------------------:| 
| Input         		| 32x32x3 RGB image   							| 
| Convolution 3x3     	| 1x1 stride, valid padding, outputs 28x28x6 	|
| RELU					|												|
| Max Pooling	      	| 2x2 stride, valid padding, outputs 14x14x6 	|
| Convolution 3x3     	| 1x1 stride, valid padding, outputs 10x10x16 	|
| RELU					|												|
| Max Pooling	      	| 2x2 stride, valid padding, outputs 5x5x16 	|
| Flatten				| outputs 400									|
| Fully Connected		| outputs 120									|
| RELU					|												|
| Fully Connected		| outputs 84									|
| RELU					|												|
| Fully Connected		| outputs 10									|

### Parameters
The following parameters where adjusted until an acceptable accuracy was achieved. The learning rate was initialy set at 0.005 but after 10 Epochs it was decreased by 20% each Epoch. 

|	Parameter         	|	Value			        					| 
|:---------------------:|:---------------------------------------------:| 
| mu		       		| 0				   								| 
| sigma		       		| 0.1				   							| 
| Batch Size       		| 250				   							| 
| Ephocs		     	| 30										 	|
| Learning Rate			| 0.005											| 

### Model Performance
The model achieved an accuracy of 0.996 on the training set and an accuracy of 0.954 on the validation set. This indicates that the model is overfitting the data. On the test set the model achieved an accuracy of 0.931.

![Accuracy and Loss Plot][image4]

## Testing the Model with New Images

### Loading New 

Five images containing German traffic signs were downloaded from the web. These images were cropped and resized to 32x32 pixels and then loaded using OpenCV and normalised.

![Normalised New Images][image5]

### Analysing Performance
The model achieved 100% validation accuracy. The predictions were as follows.

	Image 0: Children crossing
	Image 1: Slippery road
	Image 2: Go straight or left
	Image 3: Priority road
	Image 4: Right-of-way at the next intersection
	
	Validation Accuracy = 1.000

### Analysing the Softmax Probabilities
Finally the top five softmax probabilities were printed.

	Probabilities
	[[  1.00000000e+00   1.92169747e-09   4.02971683e-22   8.78265604e-23   1.16356194e-29]
	 [  1.00000000e+00   0.00000000e+00   0.00000000e+00   0.00000000e+00   0.00000000e+00]
	 [  1.00000000e+00   1.01680692e-21   9.69769272e-24   2.35175907e-30   7.10347746e-34]
	 [  1.00000000e+00   0.00000000e+00   0.00000000e+00   0.00000000e+00   0.00000000e+00]
	 [  1.00000000e+00   6.48320714e-18   1.85014703e-37   0.00000000e+00   0.00000000e+00]]

	Predictions
	[[28 29  5 24  1]
	 [23  0  1  2  3]
	 [37 39 40  4 35]
	 [12  0  1  2  3]
	 [11 30 18  0  1]]

## Summary

A convoluted neural network was created using a LeNet architecture to successfully classify German traffic signs.