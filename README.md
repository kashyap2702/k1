**Behavioral Cloning** 

### Writeup

**Behavioral Cloning Project**

The goals / steps of this project are the following:
* Use the simulator to collect data of good driving behavior
* Build, a convolution neural network in Keras that predicts steering angles from images
* Train and validate the model with a training and validation set
* Test that the model successfully drives around track one without leaving the road
* Summarize the results with a written report


[//]: # (Image References)

[image2]: ./examples/center.jpg "center"
[image3]: ./examples/l1.jpg "Recovery Image"
[image4]: ./examples/l2.jpg "Recovery Image"
[image5]: ./examples/l3.jpg "Recovery Image"
## Rubric Points
### Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/432/view) individually and describe how I addressed each point in my implementation.  

---
### Files Submitted & Code Quality

#### 1. Submission includes all required files and can be used to run the simulator in autonomous mode

My project includes the following files:
* model.py containing the script to create and train the model
* drive.py for driving the car in autonomous mode
* model.h5 containing a trained convolution neural network 
* writeup_report.md or writeup_report.pdf summarizing the results

#### 2. Submission includes functional code
Using the Udacity provided simulator and my drive.py file, the car can be driven autonomously around the track by executing 
```sh
python drive.py model.h5
```

#### 3. Submission code is usable and readable

The model.py file contains the code for training and saving the convolution neural network. The file shows the pipeline I used for training and validating the model, and it contains comments to explain how the code works.

### Model Architecture and Training Strategy

#### 1. An appropriate model architecture has been employed

My model consists of a convolution neural network with 5x5 and 3x3 filter sizes and depths between 24 and 64 (model.py lines 50-54) 

The model includes RELU layers to introduce non-linearity (code line 50-52), and the data is normalised in the model using a Keras lambda layer (code line 48). 

#### 2. Attempts to reduce over-fitting in the model

The training data was large enough so that model does not over-fit. 

The model was trained and validated on different data sets to ensure that the model was not over-fitting (code line 19,62). The model was tested by running it through the simulator and ensuring that the vehicle could stay on the track.

#### 3. Model parameter tuning

The model used an adam optimizer, so the learning rate was not tuned manually (model.py line 61).

#### 4. Appropriate training data

Training data was chosen to keep the vehicle driving on the road. I used a combination of center lane driving, recovering from the left and right sides of the road and driving in opposite direction ( clockwise and anti-clockwise ).

For details about how I created the training data, see the next section. 

### Model Architecture and Training Strategy

#### 1. Solution Design Approach

The overall strategy for deriving a model architecture was to train the model to drive in the center of the road.

My first step was to use a convolution neural network model similar to the nVidia model. I thought this model might be appropriate because it was created for similar task.

In order to gauge how well the model was working, I split my image and steering angle data into a training and validation set. I found that my first model had a low mean squared error on the training set but a high mean squared error on the validation set. This implied that the model was over-fitting. 

To combat the over-fitting, I used large training data comprise of different types of driving ( center-of-lane, recovery, opposite direction ).

The final step was to run the simulator to see how well the car was driving around track one. There were a few spots where the vehicle fell off the track such as sharp curve. To improve the driving behavior in these cases, I trained car again but for only curves and in slow speed so that I would have more data for curves.

At the end of the process, the vehicle is able to drive autonomously around the track without leaving the road.

#### 2. Final Model Architecture

The final model architecture (model.py lines 47-59) consisted of a convolution neural network with the following layers and layer sizes :

 1. Convolution layer with 2x2 stride, 5x5 filter, 24 depth and RELU layer
 2. Convolution layer with 2x2 stride, 5x5 filter, 36 depth and RELU layer
 3. Convolution layer with 2x2 stride, 5x5 filter, 48 depth and RELU layer
 4. Convolution layer with 5x5 filter, 64 depth and RELU layer
 5. Convolution layer with 3x3 filter, 64 depth and RELU layer
 6. Flatten layer
 7. Fully connected layer with 100 outputs
 8. Fully connected layer with 50 outputs
 9. Fully connected layer with 10 outputs
 10. output layer

#### 3. Creation of the Training Set & Training Process

To capture good driving behavior, I first recorded two laps on track one using center lane driving. Here is an example image of center lane driving:

![image2]

I then recorded the vehicle recovering from the left side and right sides of the road back to center so that the vehicle would learn to go back to center if deviates from center. These images show what a recovery looks like starting from left-of-lane to center-of-lane :

![image3]
![image4]
![image5]

After the collection process, I had 3 number of data points. I then preprocessed this data by normilizing (0-255 to -0.5-+0.5) image pixels.
I also cropped images to ignore top 50 pixels ( comprised of mostly 'sky' and hill-tops ) and bottom 20 pixels ( comprised of car hood ).

I used left and right camera images with changed steering value ( by deducting constant from angle for right and vice-versa for left ).

I finally randomly shuffled the data set and put 20% of the data into a validation set. 

I used this training data for training the model. The validation set helped determine if the model was over or under fitting. I used an adam optimizer so that manually training the learning rate wasn't necessary and 5 epochs.
