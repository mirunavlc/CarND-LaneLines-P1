# **Finding Lane Lines on the Road** 
Overview
---

When we drive, we use our eyes to decide where to go.  The lines on the road that show us where the lanes are act as our constant reference for where to steer the vehicle.  Naturally, one of the first things we would like to do in developing a self-driving car is to automatically detect lane lines using an algorithm.

This project detects lane lines in images using Python and OpenCV.  OpenCV means "Open-Source Computer Vision", which is a package that has many useful tools for analyzing images.  

### Prerequisites

For running this project having [Miniconda](https://docs.conda.io/en/latest/miniconda.html) helps. For short, Miniconda is a free and open-source distribution of the Python programming languages for scientific computing, that aims to simplify package management and deployment. Package versions are managed by the package management system conda. 

After having it, open a conda command prompt and create the proper environment for this project:

`conda env create -f environment.yml `

This environment contains Python 3 and the necessary packages. Do not forget to activate it:

`conda activate carnd-term1`

The last dependency needed is [Jupyter Notebook](https://jupyter.org). This can be downloaded with conda also:

`conda install -c jupyter `

Now, from the root of the project run the Jupyter command to launch the server:

`jupyter notebook P1.ipynb`

This will launch the default browser with the notebook open. 
 
 
### Goal

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

---

### 1. Description

The tools used are color selection, region of interest selection, grayscaling, Gaussian smoothing, Canny Edge Detection and Hough Tranform line detection. 

The proposed pipeline consists of 7 main steps, applied in the following order:

* **grayscale**          - applies a grayscale filter over a given color image
* **gaussian_blur**      - blurs the given image using a Gaussian filter, thus preserving the edges
* **canny**              - applies a Canny Transform in order to obtain the edges
* **region_of_interest** - maintains intact the specified region and fills the rest with black 
* **hough_lines**		 - applies the Hough Transform to the given image so that the lines found in image are found
* **draw_lines**         - [called by hough_lines] draws the given lines onto the given image with the specified color and thickness
* **weighted_img**       - overlaps 2 images, one with a higher grade of opacity and one with a lower grade of opacity 

In order to draw a single line on the left and right lanes, `draw_lines()` takes the lines provided by the `cv2.HoughLinesP()` and computes them as it follows: 

- for every pair of vertices `(x1,y1)`, `(x2,y2)` representing the ends of a line, it takes the individual points and splits them into right_lane/ left_lane categories using the `slope = (y1 - y2) / (x1 - x2)` equation 

- with the obtained vectors of points and the `np.polyfit()`, it obtains the line equations for the right and left lanes 

- start and end vertices are needed to define the lines aimed to be drawn: so the Ys are fixed to max/min and, using the previous obtained equations, the Xs are calculated


### 2. Potential shortcomings and suggestions

The provided solution follows the correct direction of the lanes but the red-drawn lines flicker. A better parameter tuning should be able to fix that.

From the challenge exercise it is obvious that this solution is custom-made for straight roads and further adjustments need to made for curves to be followed correctly. Possible directions of this adjustments are: modifying the slopes of the drawn-lines by each frame and fitting up the region_of_interest to match the provided video.


