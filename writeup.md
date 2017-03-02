#**Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

###1. Pipeline description

My pipeline consisted of following steps: 
* light normalization using CLAHE (adaptive histogram equalization)
* grayscale conversion
* suppressing noise using Gaussian blur algorithm 
* edge detection using Canny algorithm   
* lines detection using Hough transform
* thresholding and averaging found lines
* overlaying averaged lines with initial image

I didn't modify supplied _draw\_lines()_ function. Instead, i wrote following additional helper functions:
* _equalize\_light_ - normalizes scene illumination using CLAHE
* _reject\_slope\_outliers_ - filters lines by slopes
* _average\_lines_ - implements averaging algorith
Idea of averaging algorithm is to compute slopes of all lines, split them to left (negative slope) and right (positive slope), threshold both arrays to remove uninteresting lines (e.g. horisontal), for the interesting lines find the mean slope and leave only lines that slopes deviate just a little from the median, than use rest of lines to calculate mean slope without potential errors of outliers as an average slope. Then i just find top points of left and right lines, and using averaged slopes drow left and right lane lines.

This algorithm is simple and robust enough to work pretty well even on challenge clip.

###2. Potential shortcomings of current pipeline

Potential shortcoming is in slope thresholding. Very rough turns could lead to missing lines.  

###3. Possible improvements of pipeline

I have used simplest algorithm of averaging that come onto mind. There are another options, least squares is one of them.

Selection of region of interest can be more intelligent, for example one can detect the horizon and use it as upper bound.

Thinking about found lane lines as an sensor output, i'm coming to idea of some sort noise suppressing and stabilizing, maybe using Kalman filter... on a slopes for example. This approach can increase robustness of an algorithm, especially in bad cases as challenge one, but it will require frame to frame state transition and additional computational resources.