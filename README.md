[//]: # (Image References)
[image_0]: ./misc/rover_image.jpg
[![Udacity - Robotics NanoDegree Program](https://s3-us-west-1.amazonaws.com/udacity-robotics/Extra+Images/RoboND_flag.png)](https://www.udacity.com/robotics)
# Search and Sample Return Project


![alt text][image_0]

This project is modeled after the [NASA sample return challenge](https://www.nasa.gov/directorates/spacetech/centennial_challenges/sample_return_robot/index.html) and it will give you first hand experience with the three essential elements of robotics, which are perception, decision making and actuation.  You will carry out this project in a simulator environment built with the Unity game engine.  

## The Simulator
The first step is to download the simulator build that's appropriate for your operating system.  Here are the links for [Linux](https://s3-us-west-1.amazonaws.com/udacity-robotics/Rover+Unity+Sims/Linux_Roversim.zip), [Mac](	https://s3-us-west-1.amazonaws.com/udacity-robotics/Rover+Unity+Sims/Mac_Roversim.zip), or [Windows](https://s3-us-west-1.amazonaws.com/udacity-robotics/Rover+Unity+Sims/Windows_Roversim.zip).  

You can test out the simulator by opening it up and choosing "Training Mode".  Use the mouse or keyboard to navigate around the environment and see how it looks.

## Dependencies
You'll need Python 3 and Jupyter Notebooks installed to do this project.  The best way to get setup with these if you are not already is to use Anaconda following along with the [RoboND-Python-Starterkit](https://github.com/ryan-keenan/RoboND-Python-Starterkit).


Here is a great link for learning more about [Anaconda and Jupyter Notebooks](https://classroom.udacity.com/courses/ud1111)

## Recording Data
I've saved some test data for you in the folder called `test_dataset`.  In that folder you'll find a csv file with the output data for steering, throttle position etc. and the pathnames to the images recorded in each run.  I've also saved a few images in the folder called `calibration_images` to do some of the initial calibration steps with.  

The first step of this project is to record data on your own.  To do this, you should first create a new folder to store the image data in.  Then launch the simulator and choose "Training Mode" then hit "r".  Navigate to the directory you want to store data in, select it, and then drive around collecting data.  Hit "r" again to stop data collection.

## Data Analysis
Included in the IPython notebook called `Rover_Project_Test_Notebook.ipynb` are the functions from the lesson for performing the various steps of this project.  The notebook should function as is without need for modification at this point.  To see what's in the notebook and execute the code there, start the jupyter notebook server at the command line like this:

```sh
jupyter notebook
```

This command will bring up a browser window in the current directory where you can navigate to wherever `Rover_Project_Test_Notebook.ipynb` is and select it.  Run the cells in the notebook from top to bottom to see the various data analysis steps.  

The last two cells in the notebook are for running the analysis on a folder of test images to create a map of the simulator environment and write the output to a video.  These cells should run as-is and save a video called `test_mapping.mp4` to the `output` folder.  This should give you an idea of how to go about modifying the `process_image()` function to perform mapping on your data.  

## Navigating Autonomously
The file called `drive_rover.py` is what you will use to navigate the environment in autonomous mode.  This script calls functions from within `perception.py` and `decision.py`.  The functions defined in the IPython notebook are all included in`perception.py` and it's your job to fill in the function called `perception_step()` with the appropriate processing steps and update the rover map. `decision.py` includes another function called `decision_step()`, which includes an example of a conditional statement you could use to navigate autonomously.  Here you should implement other conditionals to make driving decisions based on the rover's state and the results of the `perception_step()` analysis.

`drive_rover.py` should work as is if you have all the required Python packages installed. Call it at the command line like this:

```sh
python drive_rover.py
```  

Then launch the simulator and choose "Autonomous Mode".  The rover should drive itself now!  It doesn't drive that well yet, but it's your job to make it better!  

**Note: running the simulator with different choices of resolution and graphics quality may produce different results!  Make a note of your simulator settings in your writeup when you submit the project.**

### Project Walkthrough
If you're struggling to get started on this project, or just want some help getting your code up to the minimum standards for a passing submission, we've recorded a walkthrough of the basic implementation for you but **spoiler alert: this [Project Walkthrough Video](https://www.youtube.com/watch?v=oJA6QHDPdQw) contains a basic solution to the project!**.


## Meeting Project Requirements

### Notebook Analysis
First Requirement:   
Under the section, Color Thresholding, I have a function called color_thresh and find_rocks that takes in an image and returns 2x2 matrix with the value 1 in coordinates where the ground and rocks are found respectively.  
The function identifies the location of the ground and rock by setting thresholds for the RGB values, and if the coordinates' RGB values meet or exceed the expectations, it is set to 1.

Second Requirement:  
The process_image function takes advantage of the functions defined previously in the lessons. First, it will take an image and  perform a perspective transformation to change the view from the Rover Camera to a top down view. After that, it will use filters to identify the navigable terrain, obstacles and the rocks. Once that is done, the function will gather the coordinates, change them to rover-centric coordinates, which we will ultimately use to plot the terrain, obstacles, and the rocks in the world view.
The output file can be found in output/test_mapping.mp4 from the root directory.

## Autonomous Navigation and Mapping
First Requirement:  
The perception_step() function takes advantage of the functions defined previously in the lessons. First, it will take an image and  perform a perspective transformation to change the view from the Rover Camera to a top down view. After that, it will use filters to identify the navigable terrain, obstacles and the rocks. Once that is done, the function will gather the coordinates, change them to rover-centric coordinates, which we will ultimately use to plot the terrain, obstacles, and the rocks in the world view. To determine the best angle to move, this function uses polar coordinates based on the navigable terrain of the filtered image. I have also adjusted threshold values of the navigable terrain in color_thresh because the previous value of 160,160,160 would cut off some shaded terrain.
In terms of the the decision_step, the function is unchanged from the original as it provides the basic movement and its functionality is documented in the code.  

Second Requirement:  
To further improve this project, I would have to consider which images are valid based on the roll and and pitch angles, and try to mitigate it. I could do this through the perception step by setting thresholds for the roll/pitch or I could try to make take less sharp turns or reduce the brake speed through the decision step. This will improve the map fidelity
Another thing that I would improve is the how it gets stuck in some locations. To get out of this situation, if the velocity is 0 for a period of time, I may consider changing the angle by 15 degrees or so until the velocity increases.
