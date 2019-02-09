## Project: Perception Pick & Place

---


## [Rubric](https://review.udacity.com/#!/rubrics/1067/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  

You're reading it!

### Exercise 1, 2 and 3 pipeline implemented
#### 1. Complete Exercise 1 steps. Pipeline for filtering and RANSAC plane fitting implemented.
- Convert the incoming ROS cloud to PCL format.
- Statistical Outlier Filtering is done with std_dev=0.3 to reduce noise. My experiment shows it works.
- Statistical Outlier Filtering with LEAF_SIZE=0.005. It shows good balance of reducing computational effort without the loss of too much detail.
- PassThrough Filter on z-axis is to remove other parts of PCL except the area of interest. Additional Filter on y-axis is to remove drop-boxes from cloud.
- RANSAC Plane Segmentation is to identify the table. Hence, it splits the cloud into (1) the table by itself, and (2) the objects on the table.

#### 2. Complete Exercise 2 steps: Pipeline including clustering for segmentation implemented.  
- Euclidean Clustering to do partitioning. Then, apply color to visualize each cluster.
- Next, convert PCL data back to ROS messages, then, publish to various topics for RViz.

#### 2. Complete Exercise 3 Steps.  Features extracted and SVM trained.  Object recognition implemented.
- compute_color_histograms and compute_normal_histograms are implemented as suggestion from lesson with the range [0,255] for color, and [-1,1] for normal. #bins are 32.
- Output of SVM training

![accuracy](images/training_svm_accuracy.png)

![world 3 - confusion matrix](images/figure_1_world3.png)

![world 3 - normalized confusion matrix](images/figure_2_world3.png)

Finally, I use model in percaption pipeline to identify individual object clusters, then, publish the label to RViz

### Pick and Place Setup

#### 1. For all three tabletop setups (`test*.world`), perform object recognition, then read in respective pick list (`pick_list_*.yaml`). Next construct the messages that would comprise a valid `PickPlace` request output them to `.yaml` format.

As all the objects are recognized, pr2_mover() then creates the output *.yaml.
- After grouping parameters into individual variables, here comes the main loop to iterate through the pick list. The pick_pose is get from the list of detected object whose label matches.
- Next, the simple rule based on group color of 'red' or 'green' is used to assign the arm.
- Finally, the test_scene_num is used to produce correct output file name.

I didn't have time to complete the challenge of pick and place. Will spend some time to re-visit after completing the term.