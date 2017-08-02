
# Vehicle Detection Project

The goals / steps of this project are the following:

* Develop HOG feature extractor
* Develop and train a Linear SVM classifier
* Implement a sliding-window technique to search for vehicles in images.
* Multiple Detections & False Positives
* Develop a pipeline
* Run your pipeline on a video stream 
* Estimate a bounding box for vehicles detected.

[//]: # (Image References)
[image1]: ./examples/car_not_car.png
[image2]: ./examples/windows_far.jpg
[image3]: ./examples/windows_mid.jpg
[image4]: ./examples/windows_close.jpg
[image5]: ./examples/test_picture1.jpg
[image6]: ./examples/test_picture2.jpg
[image7]: ./examples/test_picture3.jpg
[image8]: ./examples/heat_map.jpg
[image5]: ./examples/bboxes_and_heat.png
[image6]: ./examples/labels_map.png
[image7]: ./examples/output_bboxes.png
[video1]: ./project_video.mp4



###Feature Extractor

The code for this step is contained in the first code cell of the IPython notebook. I created 3 functions to extract "binned color features",
 "color histogram features" and "HOG features". All these features are appended into one list in 'extract_feature()' function.

### Develop and train a Linear SVM classifier
I started by reading in all the `vehicle` and `non-vehicle` images.  Here is an example of one of each of the `vehicle` and `non-vehicle` classes:

![alt text][image1]

I then used my 'extract_feature()' to extract the features and train an Leaner SVM classifer. I used 80% of my data set for training and 20% 
for validation. To increase the efficiency of my classifier, I augmented the dataset by flipping the images. Here is the validation results of my classifier:


* Number of Feature : 6108
* Test Accuracy  0.9921

### Sliding window technique
I used the same technique as it was mentioned in teh course. I created 3 different types of Windows:

* Window_far: Which are smaller and based on perspective, they will cover the far distance
* Windoe_mid: Bigger windoes that cover mid distance from the ego car. 
* Window_close :These are the biggest windows that I uesed which cover the closer areas to the ego car.

Here are pictures related to each type of windows:

![alt text][image2]
![alt text][image3]
![alt text][image4]

I have used my sliding windoes and classifier on test pictures and here are the results;

![alt text][image5]
![alt text][image6]
![alt text][image7]

### Multiple Detections & False Positives
I used the same technique described in teh course to create and heatmap and then apply a treashhold to filter out multiple detections 
and false positives. Here are a screenshot of heatmap out of example pictures:

![alt text][image8]


### Develop a pipeline
I implemented my pipeline as follow:
* Apply sliding window technique on the image
* Use my classifier to pick the windows that include vehicles
* Apply heatmap method to filter out multiple detections and false positives
* Add bounding box around hot regions


### video stream 
Here's a [link to my video result](./project_video_output.mp4)


###Discussion

####1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Here I'll talk about the approach I took, what techniques I used, what worked and why, where the pipeline might fail and how I might improve it if I were going to pursue this project further.  
