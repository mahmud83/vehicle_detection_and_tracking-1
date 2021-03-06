---
# Project 5 : Vehicle Detection and Tracking
---

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector. 
* Note: for those first two steps don't forget to normalize your features and randomize a selection for training and testing.
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

[//]: # (Image References)
[image1]: ./output_images/1.png
[image2]: ./output_images/2.png
[image3]: ./output_images/3.png
[image4]: ./output_images/4.png
[image5]: ./output_images/5.png
[image6]: ./output_images/6.png
[image7]: ./output_images/7.png


[image8]: ./output_images/8.png
[image9]: ./output_images/9.png
[image10]: ./output_images/10.png
[image11]: ./output_images/11.png
[image12]: ./output_images/12.png
[image13]: ./output_images/13.png
[image14]: ./output_images/14.png

[video1]: ./output_images/8.png

## [Rubric](https://review.udacity.com/#!/rubrics/513/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Vehicle-Detection/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

### Histogram of Oriented Gradients (HOG)

#### 1. Explain how (and identify where in your code) you extracted HOG features from the training images.

The code for this step is contained in the first code cell of the IPython notebook (or in lines # through # of the file called `some_file.py`).  

I started by reading in all the `vehicle` and `non-vehicle` images.  Here is an example of one of each of the `vehicle` and `non-vehicle` classes:

![alt text][image1]

I then explored different color spaces and different `skimage.hog()` parameters (`orientations`, `pixels_per_cell`, and `cells_per_block`).  I grabbed random images from each of the two classes and displayed them to get a feel for what the `skimage.hog()` output looks like.

Here is an example using the `YCrCb` color space and HOG parameters of `orientations=9`, `pixels_per_cell=(8, 8)` and `cells_per_block=(2, 2)`:


![alt text][image2]

#### 2. Explain how you settled on your final choice of HOG parameters.

I tried various combinations of parameters and selected the following parameters as optimal parameters:
Configurations:
{
 Orientations: 9 
 Pixels per cell :  8 
 Cells per block 2 
}

#### 3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

I trained a linear SVM using HOG feature and color histogram feature. while using the HOG feature only the accuracy was 97.02% and using both feature the accuracy was 96.82%. 

### Sliding Window Search

#### 1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?

As we need to detect the cars only, I selected the region of interest from the middile of the image and discarded the pixels at the bottom of the image. Then I ran the sliding window search with the boxes. The boxes drawn in the picture as below:

![alt text][image3]

#### 2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?

Ultimately I searched on two scales using YCrCb 3-channel HOG features plus spatially binned color and histograms of color in the feature vector, which provided a nice result.  Here are some example images:

![alt text][image4]
![alt text][image5]
![alt text][image6]
![alt text][image7]
![alt text][image8]
![alt text][image9]

### Video Implementation

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)

Here's the video output:

[![IMAGE ALT TEXT](http://img.youtube.com/vi/jlGjbjQ4kCM/0.jpg)](https://www.youtube.com/watch?v=jlGjbjQ4kCM "Vehicle Detection and Tracking Output")



#### 2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

I recorded the positions of positive detections in each frame of the video.  From the positive detections I created a heatmap and then thresholded that map to identify vehicle positions.  I then used `scipy.ndimage.measurements.label()` to identify individual blobs in the heatmap.  I then assumed each blob corresponded to a vehicle.  I constructed bounding boxes to cover the area of each blob detected.  

Here's an example result showing the heatmap from a series of frames of video, the result of `scipy.ndimage.measurements.label()` and the bounding boxes then overlaid on the last frame of video:

### Here are six frames and their corresponding heatmaps, the output of `scipy.ndimage.measurements.label()` on the integrated heatmap from all six frames and the resulting bounding boxes are drawn onto the last frame in the series:

![alt text][image10]
![alt text][image11]
![alt text][image12]
![alt text][image13]
![alt text][image14]

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

During the implementation the there are few false positiving results generated and those were able to be removed by heatbox. Overall ther output is able to track down the vehcile properly. In the video SVM might fail if the dataset is not trained properly. Another approach is to use YOLO algorithm to detect in a more fast and reliable way. SVM is much slower comapred to YOLO and other CNN approaches. I would like to build the next model with YOLO algorithm next. 
