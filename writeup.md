
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
[image2]: ./output_images/windows_far.png
[image3]: ./output_images/windows_mid_far.png
[image4]: ./output_images/windows_mid.png
[image5]: ./output_images/windows_close.png
[image6]: ./output_images/windows.png
[image7]: ./output_images/processed_image.png

[image8]: ./output_images/heat_map.jpg
# [video1]: ./project_video_output.mp4



### Feature Extractor

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
I used the same technique as it was mentioned in teh course. I created 4 different types of Windows:

* Window_far: Smallest windos to bound cars in the far distance
* Window_mid_far: Which are smaller bounding windows to cover teh vehicles in mid_far distance
* Windoe_mid: Bigger windoes that cover mid distance vehicles 
* Window_close :These are the biggest windows that I uesed which cover the closer areas to the ego car.

Here are pictures related to each type of windows:

![alt text][image2]
![alt text][image3]
![alt text][image4]
![alt text][image5]
![alt text][image6]

I have used my sliding windoes and classifier on test pictures and here are the results;


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


### Discussion

Here are some challenges or issues that I encountered during the development of the vehicle detector:
- Format of pctures (RGB vs BGR) introduced a bug initially. It's very important to track the image format that we use since the pipeline is RGB and cv2.imread returns BGR.
- After fine tuning the size of windows and threshold, I was able to detect vehicles. As a future work, using tracking algorithm will improve the performance.
- I have used 4 size of boxes to search for vehicles. A smarter way of using sliding boxes should be used based on the information from tracking.
