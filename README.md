# Institute Technical Summer Project 2018
Name of Team - Trace Elements
<br>
Name of Project - Stone-Paper-Scissor Champion

## Table of contents
1. [Description of the Project](#description)
2. [Dependencies](#dependencies)
	- [OpenCV](#opencv)
	- [NumPy](#numpy)
3. [Algorithms](#algorithms)
4. [Implementation](#implementation)
5. [Issues](#issues)

### <a name="description"></a>1. Description of the Project
The aim of our project is to make a program which would play stone - paper - scissor and should be capable of winning every time. So, we used [OpenCV](https://opencv.org/) on [Python](https://www.python.org/) for in order to identify the hand gestures. _(Gestures of paper, scissor and rock.)_ The output is shown on a 10x10 LED grid with the help of [Arduino Uno](http://www.arduino.cc/).

### <a name="dependencies"></a>2. Dependencies
Python 3 (preferably Jupyter notebook) is required to run the project with following dependencies:
- <a name="opencv"></a>OpenCV
> OpenCV is a library of programming functions mainly aimed at real-time computer vision.

  __Installation__<br>
  `pip install opencv-python`

- <a name="numpy"></a>NumPy
> NumPy is a Python library, which adds support for large, multi-dimensional arrays and matrices, along with a large collection of high-level mathematical functions to operate on these arrays.

  __Installation__<br>
  `pip install numpy`

### <a name="algorithms"></a>3. Algorithms
While carrying out the project, it was a possibility that the machine learning approach decided earlier may not be fast enough. So, we decided to use multiple algorithms. Below is the list of implemented algorithms:
- **Algorithm 1:**
[By drawing the convex hull and finding convexity defects](#algorithm1)
- //TODO **Algorithm 2:**
[Using ends of fingers as markers](#algorithm2)
- //TODO **Algorithm 3:**
[Using Machine Learning](#algorithm3)

### <a name="implementation"></a>4. Implementation

Below is the explanation of how we implemented our algorithms:

1. **Algorithm 1:** <a name="algorithm1"></a>By drawing the convex hull and finding convexity defects
	- It detects the skin colour (hand in our case) from camera input and separates out that part of window.
	- For reducing noise in the separated image it dilates and blurs that image. Thus, contour detection becomes more reliable.
	- After contour detection, it find the contour of maximum area which will be our hand in this case.
	- Using a detected contour of maximum area it creates a convex hull around hand.
	- Then it finds the defects in convex hull. To ensure it only counts the defects between the fingers we use angle between fingers as parameter. If angle is less than 90, then only it counts that defect, ignoring obtuse angled cases. For measuring angle we used cosine rule as length of sides is known.
	- Another parameter used for comparison is the area ratio defined as:
	<a href="https://www.codecogs.com/eqnedit.php? latex=\dpi{120}&space;\small&space;area.ratio&space;=&space;\frac{area.of.convex.hull-area.of.contour}{area.of.contour}\times&space;100" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\small&space;area.ratio&space;=&space;\frac{area.of.convex.hull-area.of.contour}{area.of.contour}\times&space;100" title="\small area.ratio = \frac{area.of.convex.hull-area.of.contour}{area.of.contour}\times 100" /></a>
	<br>
	Area ratio finds the area in convex hull not covered by hand.<br>
	Finally using no of defects point and area ratio it determines the gesture:
Defect points | Area Ratio | Result
--------------|------------|-------
0 | less than 12 | rock
1 | between 12 and 20 | scissor
4 | greater than 20 | paper

### <a name="issues"></a>5. Issues
Issues faced in the project:
**1. In detection of skin colour:** It is not possible to find exact hsv colour range for skin colour as skin colour varies from person to person. Still if one tries to widen the range of skin colour it starts detecting colours like red and yellow too.<br>
**Fix:** At the moment,
