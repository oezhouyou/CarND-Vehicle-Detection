##Writeup Template

---

**Vehicle Detection Project**

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector. 
* Note: for those first two steps don't forget to normalize your features and randomize a selection for training and testing.
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

[//]: # (Image References)
[image0]: ./output_images/car.png
[image1]: ./output_images/not_car.png
[image2]: ./examples/HOG_example.jpg
[image3]: ./examples/sliding_windows.jpg
[image4]: ./output_images/sliding_window.png
[image5]: ./examples/bboxes_and_heat.png
[image6]: ./examples/labels_map.png
[image7]: ./examples/output_bboxes.png
[video1]: ./output_video.mp4

## [Rubric](https://review.udacity.com/#!/rubrics/513/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Vehicle-Detection/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

### Histogram of Oriented Gradients (HOG)

#### 1. Explain how (and identify where in your code) you extracted HOG features from the training images.

At first,  all `vehicle` or`non-vehicle` images are loaded.  Here is an example of one of each of the `vehicle` and `non-vehicle` classes:

![alt text][image0]
![alt text][image1]

I then explored different color spaces and different `skimage.hog()` parameters (`orientations`, `pixels_per_cell`, and `cells_per_block`). In my final setup, `YCrCb` color space is used and HOG parameters of `orientations=8`, `pixels_per_cell=(8, 8)` and `cells_per_block=(2, 2)`. Please refer to the hog_features( ) in my notebook for details.


#### 2. Explain how you settled on your final choice of HOG parameters.

I tried various combinations by sweeping the HOG parameters and the final choice provide the best recognition result and acceptable computing time.

#### 3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

I trained a linear SVM using sklearn.svm LinearSVC( ) library by feeding HOG features generated by vehicle/non-vehicle image data. The data is also scaled by using StandardScaler( ) function from sklearn. A small mount of the data is left out as testing data.

Please check class HOGClassifier in my notebook for more details.

### Sliding Window Search

#### 1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?

In my jupyter notebook search_vehicles( ) function, a sliding window search is implemented. Instead of percentage of overlapping, the cells_per_step is defined to generate the steps. The window is 64 x 64 , then number of blocks per window could be calculated. The cells_per_step could be chosen slightly differently, resulting similar detection, leaving some room for fine tuning.

#### 2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?

YCrCb 3-channel HOG features, spatially binned color and histograms of color are included in the final feature vector. Here are some example images:

![alt text][image4]

---

### Video Implementation

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)
Here's a [link to my video result](./output_video.mp4)

#### 2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

In my process_image( ) function, the positive detections of each frame generate a heatmap and then the map is thresholded to output a likely vehicle positions.  Afterwards, cv2.findContours( ) and cv2.boundingRect( ) to generate bounding boxes from heatmap contours.

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Some vehicle types which didn't in the training data may not get picked correctly, naturally. By adding other object detection algorithm and fuse the results will make it more robust.

