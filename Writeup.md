# **Project 1: Finding Lane Lines on the Road** 

## **Project Summary**

This project aims to detect lane lines on the road. A pipeline which finds the lines will be presented below.

### **Content:**

**1.** Pipeline Explanation

**2.** Potential Shortcomings

**3.** Possible Improvement Points

[//]: # (Image References)

[image1]: ./examples/Initial_image.png "Image"
[image2]: ./examples/Grayscale_image.png "Grayscale"
[image3]: ./examples/masked_edges.png "Canny"
[image4]: ./examples/region_interest.png "Region"
[image5]: ./examples/hough_image.png "hough1"
[image6]: ./examples/all_lines.png "lines"
[image7]: ./examples/curve_fit.png "curve"
[image8]: ./examples/curve_fitEnd.png "curvefit"
[image9]: ./examples/result_image.png "result"

---

## **Reflection**

### **1. Pipeline explanation**

The pipeline consists of X steps. 

The images are converted to grayscale to use Canny edge detector. By the grayscale conversion, there is only 2-dimensional matrix left representing the image. This allows to detect the strong edge pixels by taking the derivative of the matrix. Additionally, Gaussian smoothing is applied to the grayscale image before Canny edge detection in order to filter the noises.

Grayscale converion and Gaussian smoothing can be seen below.

![alt text][image1] ![alt text][image2]

Then Canny edge detection algorithm is used to detect edges in the image. Additionally, a trapezoid shaped region of interest is determined and the image is masked.

![alt text][image3] ![alt text][image4]

Hough transform method was used on the masked image to detect lines. It was observed that the lines might be dashed/dotted based on the Hough transform parameters.

![alt text][image5]

*draw_lines* function was updated to complete the lines on the image. 

*Steps of draw_lines():*

1- (x1,y1,x2,y2) coordinates of all lines are examined. Right and left lines (corresponding to the middle of the image) are classified by using slope formula: (y2-y1)/(x2-x1). Classification also includes minimum/maximum slope value comparison to eliminate vertical and horizontal lines.

![alt text][image6]

It is expected that the right line should exist of lines with positive slope. Same is valid for the left side by considering negative slope.

2- Average values of both negative and positive slope values are calculated. Average values are used as thresholds to eliminate the lines, which are considered as not fitting the pattern.

3- (x1,y1,x2,y2) cooridantes of negative and positive lines  (based on slope sign) are determined. There might be some lines which as negative slope by their coorinates belongs to right side of the image. Same case is valid for the lines with positive slope.

In order to eliminate those lines, all x coordinates are checked and compared by the middle of the image.

4- Finally, curve fit functions are used to fit lines (y=mx+b) on both left side and right side (x,y) points. Inverse function of the fitted line (x=(y-b)/m), is used to draw the lines starting from bottom to top of the "region of interest".

![alt text][image7] ![alt text][image8]

Fitted lines are shown on the image below.

![alt text][image9]

### 2. Potential Shortcomings

It is possible to detect some edges away from the middle of the image if the hough parameters are not adjusted very well. In this case, draw_lines() function may take those edges(lines) into the calculation if their slope values are reasonable. The (x,y) coordinates of those lines may result in rotating the fitted curve. 

### 3. Possible Improvement Points

The (x,y) coordiantes of the lines can be used to check the distance from the most common positions of all points (expectedly where the real line is). Too far points can be elimianted to solve the potential issue explained above.
