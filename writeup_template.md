# **Behavioral Cloning** 

## Writeup Template
---

**Behavioral Cloning Project**

The goals / steps of this project are the following:
* Use the simulator to collect data of good driving behavior
* Build, a convolution neural network in Keras that predicts steering angles from images
* Train and validate the model with a training and validation set
* Test that the model successfully drives around track one without leaving the road
* Summarize the results with a written report


[//]: # (Image References)

[model_plot]: ./model_plot.png "Model Architecture"
[recovery_1]: ./examples/recovery_1.png ""
[recovery_2]: ./examples/recovery_2.png ""
[recovery_3]: ./examples/recovery_3.png ""
[recovery_4]: ./examples/recovery_4.png ""
[center_1]: ./examples/center_1.png ""
[center_2]: ./examples/center_2.png ""
[center_3]: ./examples/center_3.png ""
[center_4]: ./examples/center_4.png ""
[center_5]: ./examples/center_5.png ""
[flipping_1]: ./examples/flipping_1.png ""
[flipping_2]: ./examples/flipping_2.png ""
[center]: ./examples/center.png ""
[left]: ./examples/left.png ""
[right]: ./examples/right.png ""

## Rubric Points
### Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/432/view) individually and describe how I addressed each point in my implementation.

---
### Files Submitted & Code Quality

#### 1. Submission includes all required files and can be used to run the simulator in autonomous mode

My project includes the following files:
* model.ipynb containing the notebook to create and train the model
* drive.py for driving the car in autonomous mode
* model.h5 containing a trained convolution neural network 
* writeup_report.md summarizing the results
* video.mp4 showing how the car drives

#### 2. Submission includes functional code
Using the Udacity provided simulator and my drive.py file, the car can be driven autonomously around the track by executing 
```sh
python drive.py model.h5
```

#### 3. Submission code is usable and readable

The model.ipynb file contains the code for training and saving the convolution neural network. The file shows the pipeline I used for training and validating the model.

### Model Architecture and Training Strategy

#### 1. An appropriate model architecture has been employed

My model is heavily inspired by [DAVE-2](https://devblogs.nvidia.com/deep-learning-self-driving-cars/) architecture.

#### 2. Attempts to reduce overfitting in the model

The model was trained and validated on different data sets to ensure that the model was not overfitting. The first time, validation error was mich higher then the train one. So I used left-right image flip to augment the dataset, which solved the issue.
The model was tested by running it through the simulator and ensuring that the vehicle could stay on the track.

#### 3. Model parameter tuning

The model used an adam optimizer, so the learning rate was not tuned manually.

#### 4. Appropriate training data

Training data was chosen to keep the vehicle driving on the road. I used a combination of center lane driving, recovering from the left and right sides of the road.

For details about how I created the training data, see the next section.

### Model Architecture and Training Strategy

#### 1. Solution Design Approach

The idea was to take an appropriate existing architecture and reduce it to my input size, plus make local training on CPU faster.
Thus I picked DAVE-2 architecture, removed the 1164 fully connected layers and used fewer CNNs layers. In addition I used cropping of the image, to cut out less informative parts with the car itself and the sky.

In order to gauge how well the model was working, I split my image and steering angle data into a training and validation set. I found that my first model had a low mean squared error on the training set but a much higher mean squared error on the validation set. This implied that the model was overfitting.

To combat the overfitting, I have augmented the dataset by flipping the images horizontally.

At the end of the process, the vehicle is able to drive autonomously around the track without leaving the road.

#### 2. Final Model Architecture

Here is a visualization of the architecture:

![Model Architecture][model_plot]

#### 3. Creation of the Training Set & Training Process

To capture good driving behavior, I recorded the vehicle going straight along the road, as well as recovering from the left side and right sides of the road back to center so that the vehicle would learn to stay within the driving area of the road.

Going straight:
![Center 1][center_1]
![Center 2][center_2]
![Center 3][center_3]
![Center 4][center_4]
![Center 5][center_5]
Recovering:
![Recovery 1][recovery_1]
![Recovery 2][recovery_2]
![Recovery 3][recovery_3]
![Recovery 4][recovery_4]

Then I repeated this process in the opposite direction of the track.

To augment the data sat, I used left-right flipping of images, as well as left and rights cameras' views.

Flipping:
![Flipping 1][flipping_1]
![Flipping 2][flipping_2]
Left, Center and Right cameras:
![Center][center]
![Left][left]
![Right][right]

After the collection process, I had 3600 data points. I then used cropping as part of the model to cut off the sky and the cat itself.

I finally randomly shuffled the data set and put 20% of the data into a validation set.

I used this training data for training the model. The validation set helped determine if the model was over or under fitting. The ideal number of epochs was 5 as further training was overfitting as can be seen in the model.ipynb notebook. I used an adam optimizer so that manually training the learning rate wasn't necessary.
