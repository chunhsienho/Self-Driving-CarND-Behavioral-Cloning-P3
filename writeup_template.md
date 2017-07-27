#**Behavioral Cloning** 

##Writeup Template

###You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Behavioral Cloning Project**

The goals / steps of this project are the following:
* Use the simulator to collect data of good driving behavior
* Build, a convolution neural network in Keras that predicts steering angles from images
* Train and validate the model with a training and validation set
* Test that the model successfully drives around track one without leaving the road
* Summarize the results with a written report


[//]: # (Image References)

[image1]: ./examples/model.jpg "Model Visualization"
[image3]: ./examples/center.jpg "Center Image"
[image4]: ./examples/left.jpg "Left Image"
[image5]: ./examples/right.jpg "Right Image"
[image6]: ./examples/before_flipped.jpg "Before Flipped Normal Image"
[image7]: ./examples/flipped.jpg "Flipped Image"

## Rubric Points
###Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/432/view) individually and describe how I addressed each point in my implementation.  

---
###Files Submitted & Code Quality

####1. Submission includes all required files and can be used to run the simulator in autonomous mode

My project includes the following files:
* model.py containing the script to create and train the model
* drive.py for driving the car in autonomous mode
* model.h5 containing a trained convolution neural network 
* writeup_report.md or writeup_report.pdf summarizing the results
* video.py change the picture into video

####2. Submission includes functional code
Using the Udacity provided simulator and my drive.py file, the car can be driven autonomously around the track by executing 
```sh
python drive.py model.h5
```

####3. Submission code is usable and readable

The model.py file contains the code for training and saving the convolution neural network. The file shows the pipeline I used for training and validating the model, and it contains comments to explain how the code works.

###Model Architecture and Training Strategy

####1. An appropriate model architecture has been employed

My model consists of 6 convolution neural network with 4 fullt connected layers(3 max pooling layer and 2 dropout layer between layer). 2 dropout layer to prevent overfitting and 1 cropping layer to reduce the input dimension.

The model includes RELU layers to introduce nonlinearity (code line 20), and the data is normalized in the model using a Keras lambda layer (code line 18). 


####2. Attempts to reduce overfitting in the model

The model contains  2 dropout layers in order to reduce overfitting (model.py lines 177-179). 

The model was trained and validated on different data sets(80/20) to ensure that the model was not overfitting (code line 79). 

The model was tested by running it through the simulator and ensuring that the vehicle could stay on the track.

####3. Model parameter tuning

The model used an adam optimizer, so the learning rate was not tuned manually (model.py line 25).

####4. Appropriate training data

Training data was chosen to keep the vehicle driving on the road. 

I drive the vehicle for both clockwise and countercloskwise for several round so as to give enough turning data for turning right and left.

Also, I used a combination of center lane driving, recovering from the left and right sides of the road. 

For details about how I created the training data, see the next section. 

###Model Architecture and Training Strategy

####1. Solution Design Approach

The overall strategy for deriving a model architecture was to trail and error to find the best idea.

My first step was to create a model which is very similar to the NVidia model. To use CPU to train my model. I reduce the model size by using the following steps:

1. Add more convolution layer instead of fully connected layer 

2. Add some max pooling layer 

3. Resize the image by 1/4 by using average pooling layer

4. Crop sky, top, and bottom nad some other useless input images

After setting the model, i start to record the data.

My collecting data is not using the whole running data.

1. I record my driving data both clockwise and counterclockwise to prevent the bias turning left

2. Add recovery data using left/right camera images to teach my model to drive back to the center of the road

3. Collect less straight data and collect more turning data.

After using these data, I find that the vehicle would slight touch the line. I guess this is because of the vehicle speed is so high in drive.py. So I reduce the speed from 30 to 20 and it run smooth and good.

####2. Final Model Architecture

The final model architecture (model.py lines 147-184) looks like the following image.

Here is a visualization of the architecture (note: visualizing the architecture is optional according to the project rubric)

![alt text][image1]

####3. Creation of the Training Set & Training Process

To capture good driving behavior, I first recorded two laps on track one using center lane driving. Here is an example image of center lane driving:

![center image][image3]

Here is the recovery steering angle 0.1 and -0.1 (Left and Right image)


![Left image][image4]  ![Right image][image5]

Then I repeated this process on track two in order to get more data points.

To augment the data sat, I also flipped images and angles thinking that this would ... For example, here is an image that has then been flipped:

![Before Flipped][image6]    ![Flipped image][image7]



After the collection process, I had 2352 number of data points. 

I finally randomly shuffled the data set and put 20%(588) of the data into a validation set. 

I used this training data for training the model. The validation set helped determine if the model was over or under fitting. The ideal number of epochs was 10 since the loss does not decrease much after that. I used an adam optimizer so that manually training the learning rate wasn't necessary.
