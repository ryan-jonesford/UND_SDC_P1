# **Finding Lane Lines on the Road**
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images_output/grayscale.png "Grayscale"
[image2]: ./test_images_output/grayscale_blur.png "Grayscale with Blur"
[image3]: ./test_images_output/canny.png "Canny Filter"
[image4]: ./test_images_output/roi.png "ROI"
[image5]: ./test_images_output/raw_hough.png "Hough lines"
[image6]: ./test_images_output/lanes.png "Lanes Detected"

---

## Reflection

### *Lane detection pipeline description.*

My pipeline consisted of 6 steps: 
1. **Convert the images to grayscale**
![GrayScale][image1]
2. **Smooth out the edges with gaussian blur**
![GrayScale with blur][image2]
3. **Detect the edges with a Canny filter**
![Canny Filter][image3]
4. **Define a region of interest and disregard input that is outside of it**
![Region of Interest defined][image4]
5. **Run the Hough Lines process on it to detect the lines**
![Raw Hough Lines][image5]
6. **Draw the Lane lines** 
![Lanes Detected][image6]


In order to draw a single line on the left and right lanes, I modified the draw_lines() function by:
1. Filtering out hough found lines with slopes less than 45deg and seperating out the lines with a positive and negative slope
2. I averaged out the slopes and intercepts of the lines on the left and right in a single image
3. I used a window average of 30 samples to smooth out the slopes and intecepts for video 
    * I chose 30 samples by figuring that video is probably around 30fps, giving a 1 second windowed average. 
4. Using the smoothed slopes and intercepts I set my y1 values to be the bottom of the video and y2 to to be the end of my region of interest. The x values where computed from these y values.


### *Potential shortcomings with this pipeline*
1. Because of the 30 sample window average, fast lane changes may not be properly detected
2. A vehicle that enters the Region of interest could cause problems
3. The region of interest is rather restrictive
4. I filter out horizontal lines, but we might want to detect them in the case of a crosswalk or stop line
5. Values chosen for the gaussian filter, Canny filter, region of interest, and hough line detection are all fixed. Changes in weather or lighting could make these values not work. 


### *Possible improvements to this pipeline*

1. More image pre-processing to better detect edges on low contrast lines (such as the yellow line in the challenge video). Maybe I could run a Canny filter on a color filtered image and average that with the Canny filter ran on the grayscale
2. Hard coded values should be dynamic based on the changing conditions of the images/videos. 
