# **Finding Lane Lines on the Road** 


**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images/challenge3.jpg "Challenge video dump"
[image2]: ./test_images/challenge3-masked.jpg "Region Masked"
[image3]: ./test_images/challenge3-colorfiltered.jpg "Color Filtered"
[image5]: ./test_images/challenge3-Drawlines_after_Hough_Transform.jpg "Hough Transform + extrapolation"
[image4]: ./test_images/challenge3-Gaussian-bilateralFilter-Smoothed.jpg "Smoothed"
[image6]: ./test_images/challenge3-Composite.jpg "Composite"

---

### Reflection

#####Pipeline description  and comments
The image processing pipeline consisted of the following steps.

 
1. Select the region of the image on which the next step to be applied, using a simple algorithm to estimate the region of interest, based on image width to height ratio and the realization that the camera view on the road is fixed.
2. Filter out Yellow-ish and white-ish colors in two steps.` cv2.inRange(image, lower_white, upper_white)` and `cv2.inRange(image, lower_yellow, upper_yellow)`
2. Apply Bilateral and Gaussian denoise `cv2.bilateralFilter(color_mask,9,75,75)` and `gaussian_blur(blur_mask, 7)`
3. Skip Canny line detection	
5. Apply Hough transform `cv2.HoughLinesP(...)`, with    
	6. 1 pixel distance resolution
	7. Pi/180  angular resolution
	8. 60 minimum number of intersections
	9. 18 minimum number of pixels making up a line
	10. 3 max_line_gap
11. draw the lines from Hough transform using parameters from the previous image to extrapolate lines

 
##### draw_lines()
In order to draw a single line on the left and right lanes, I modified the draw_lines() 

    `draw_lines(img, lines, prev_image_params, mask, color=[0, 0, 255], thickness=2):`

The input parameter `prev_image_params` carries info from the previous image.

The new `draw_lines()` function process data as following

1. Analyze Hough transform lines, separate left and right lanes
2. Use histogram analysis to clean out outliers from the detected lines (this is prone to skewed results if one particular image has few detected lines only,however this can be solved by carrying over information from one to the next image, hence the `prev_image_params` input parameter
	1. Extract x and y coordinates from the Hough lines, 
	2. shape into vertices 
	3. sort coordinates into ascending values 
	4. use coordinates to draw segment of lines after extrapolation of missing segments
Using segment rather than one single linear extrapolation increases stability and follows the actual lane marking lines with less visible errors.  

###Pipeline images 
![][image1]
![][image2]
![][image3]
![][image4]
![][image5]
![][image6]

### 2. Identify potential shortcomings with your current pipeline


#####Shortcomings

The algorithm relies on a linear model of the lane markings along the road, which cannot describe adequately  the complexity of assigning meaning to the detected lines. Rephrased, lines a linear, but on the road there is a lot more than lines to be seen and this model fails to eliminate detected lines that are not the lane markings and cannot filter noise adequately. 

#####Other issues
The algorithm thresholds are not adaptive and therefore the quality of the lane markings will impact the effectiveness of the lane detection, which makes the algorithm unreliable for real world use.   
Color filtering is prone massive errors when lighting conditions change abruptly ... say at night, entering and exiting tunnels during daylight  etc etc

### 3. Suggest possible improvements to your pipeline

#####Improvements

Find a non linear model to separate the detected lines between left and right lanes and ignore noise and other markings on the road.

The concept of lane has to be extended to include pavement detection rather than just lane markings. 
Lanes are often without marking, yet a vehicle must be able to use other clues to recognize where the actual lane is.   
