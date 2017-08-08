
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
[image8]: ./output_images/heat_map.png



### Feature Extractor

I created 3 functions to extract the following features from images:

* binned color features
* color histogram features
* HOG features

All these features are appended into one list in 'extract_feature()' function. Here are the parameters that used in each function:

#### binned color features
I used 'cv2.resize().ravel()' to create the feature vector. I tried "(16, 16)" and "(32, 32)" and at the end "(16, 16)" provided a good performance. I implemented the code of binned color extraction in 'def bin_spatial(img, size=(32, 32))' function.

#### color histogram features
I used 'np.histogram()' to compute color histogram of each channel seperatly and at the end, I concatenayed them into one vector. I used 16 bin for the histogram and used 0 to 256 as the range. I implemented the code in the 'def color_hist()' function.

#### HOG features
I used 'hog()' function to extract HOG features. Getting HOG features was the trickies part since it required more tries and error.  At the end, the follwoing settings provided a good performance to extract HOG features that work well with my classifier in teh next section:

* orient = 9  # HOG orientations
* pix_per_cell = 8 # HOG pixels per cell
* cell_per_block = 2 # HOG cells per block

The code for HOG feature extraction is implemented in 'def get_hog_features()' function.

### Develop and train a Linear SVM classifier
I started by reading in all the `vehicle` and `non-vehicle` images.  Here is an example of one of each of the `vehicle` and `non-vehicle` classes:

![alt text][image1]

I then used my 'extract_feature()' to extract the features and train an Leaner SVM classifer:

* I used 80% of my data set for training and 20% for validation.
* I augmented the dataset by flipping the images to increase the efficiency of my classifier.
* I also tried different color space and 'HSV' provided a better accuracy. 

Here is the validation results of my classifier:


* Number of Feature : 6108
* Test Accuracy  0.9921

### Sliding window technique
I used the same technique as it was mentioned in teh course. I created 4 different types of Windows:

* Window_far: Smallest windos to bound cars in the far distance
* Window_mid_far: Which are smaller bounding windows to cover teh vehicles in mid_far distance
* Windoe_mid: Bigger windoes that cover mid distance vehicles 
* Window_close :These are the biggest windows that I uesed which cover the closer areas to the ego car.

Here are pictures related to each type of windows:

![small windoes][image2]

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

- Format of pctures (RGB vs BGR) introduced a bug initially. It's very important to track the image format that we use since the pipeline is RGB and cv2.imread returns BGR. I implemented the code in the way that the impot images are always RGB and then, I changed the color space if it was required inside the feature_extractor file. 
- Color Space: I tried different color space to create a classifier. Although the performance was quite similar, but HSV provided a petter accuracy.
- After fine tuning the size of windows and threshold, I was able to detect vehicles. As a future work, using tracking algorithm will improve the performance.
- I have used 4 size of boxes to search for vehicles. A smarter way of using sliding boxes should be used based on the information from tracking.
- I tried to limit my searching area (windows location) to reduce the number of windows. However, I made some assumptions such as limits of road curvature. My algorithms will fail in more general cases where the radius of curvature is less than my assumptions. Combining lane detection and vehicle detection might help.

