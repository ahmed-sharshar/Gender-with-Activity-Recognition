# Gender-with-Activity-Recognition

## Overview

Regarding the booming of big data and HAR in the scientific world, we conducted a research project that addresses the recognition of activities performed in addition to the gender of the performer. In this work, we present two models (hierarchical model and joint distribution model) and compare between two datasets (MoVi and MotionSense), using only two IMU sensors on right and left hand and motion sense dataset using mobile phone, to predict gender with activity and see how every activity reflect on gender, and explore the efficiency on using autocorrelation function as a feature extractor and compare between three classifiers, Random Forest (RF), Support Vector Machine (SVM) and Convolution Neural Network (CNN).
The diagram below illustrates the overall process of data processing, model designing, and inference.  
<p align="center">
<img src="https://user-images.githubusercontent.com/61229902/170962144-66ba0511-db33-4c87-a7cd-1b8906058ed1.png" width="900" height="200" />
</p>

<!-- <p align="center">
<img src="https://user-images.githubusercontent.com/61229902/170965290-d462820d-0c0d-465b-8615-b68893f27bbb.png" width="200" height="300" />
</p> -->
## Datasets
We relied on two open source datasets. <br>
A. The <a href=https://www.biomotionlab.ca/movi/> MoVi </a> dataset <br>
MoVi is an open source dataset that contains
two types of data: IMU sensory data and optical motion capture (MoCap). We used the IMU data collected from 90 participants, 60 of them are females and 30 are males, which were chosen with a high degree of variation by means of height, weight, and age. We used data of 80 different subjects from two rounds. We used data from the two rounds for male subjects in order to compensate for the shortage in the males subjects. Eventually, we used data from 51 females and 51 males. The extracted and preprocessed data are from two IMU units only, right and left hands. As, some volunteers are left-handed, some are right-handed, while others use both, so we had to capture all data from both units. In our analysis we only used the acceleration and the angular velocity; these two particular modalities can represent the 3-axial Gyroscope and 3-axial Accelerometer which both are found in smart watches and mobile phones. The MoVi dataset is represented as data-files and corresponding videos of the subjects doing the activities; these videos are synchronized with the data-files. 

Data Visualization for MoVi data, 
<p align="center">
<img src="https://github.com/saeed1262/MoVi-Toolbox/blob/352621e742ff8f745c3ada417d3db22d0ddf31ae/demo.gif" />
</p>

- Feature Selection <br>

Using the synchronized videos, we monitored the beginning and the ending
time of each activity by watching the videos for each subject in order to be able to extract the raw data for that activity from the raw data of the dataset. In the raw dataset, every second is presented as 120 timesteps, as the IMU devices in 120 Hz, we converted the captured time into seconds and calculated the starting and ending time for each activity. Using a Matlab script, we extracted the data for each activity desired, given the starting and ending timesteps, and saved that data as a distinct data segment. The extracted activities are four common activities, which are walking, running, clapping and waving.

Afterwards, the extracted dataset was split into fixed-size samples. Each sample represents a 1.5-second signal with overlapping samples of 0.25 seconds. The sampling rate was at 120 Hz, in other words, each sample had 180 time step rows representing 1.5 sec of sensor readings and 12 feature columns representing the 3-axis acceleration and 3-axis angular velocity for both right and left hand. A scatter plot of the data is represented below.

<!-- <p align="center"> -->
| <img src="https://user-images.githubusercontent.com/61229902/171028549-f930f77a-0caf-4fca-8cec-101b479ade6a.png"  width="75%"  class="img-responsive"> |
|:---:|
| Scatter Plot of 5 time records for 4 acctivities of both genders |
<!-- </p> -->


The Figure features multiple data segments, each segment is a representation for the data extracted from the raw data files. Each point in the graph represents a time step, with its coordinates to be the 3-axis acceleration coordinates. The plot includes 8 different colors, each represents a different class, each class represents specific activity and gender. Thus, the closeness of points with the same colors, and the gap between each class points as a cluster reflects the ability of the classifier result in a reliable accuracy.




B. The <a href = "https://github.com/mmalekzadeh/motion-sense">MotionSense</a> Dataset <br>
The MotionSense dataset is an open source dataset that
contains IMU data collected from iPhone 6s mobile phone
kept in the participant’s front pocket. The dataset features
a sampling rate of 50 Hz. This dataset includes time-series
data generated by 3-axis gyroscope and 3-axis accelerometer
sensors. A total of 24 participants from both genders (10
females and 14 males), different ages, weights, and heights,
performed 6 activities in 15 trials in the same environment and
conditions. See the figure below. 

| <img src="https://github.com/mmalekzadeh/motion-sense/blob/master/materials/desc.png" class="img-responsive"> |
|:---:|
| Time-series correspond to Walking activity of data subject(code 3). There are 12-features. |

- Feature Selection <br>

We have chosen five activities from this dataset for 14 males
and 10 females: going downstairs, going upstairs, walking,
jogging, and sitting. We used the data segments of the long
round which lasts for 2 − 3 minutes duration. With total
samples of 3, 688 data segment, each segment represents 5
seconds, with no overlapping, for 3-axis accelerometer and
3-axis gyroscope sensors producing measurements such as
attitude, gravity, user acceleration, and rotation rate. The figure below
represents the 3-axis attitude where every color represents
one activity with one gender, therefore there are 10 different
colors which represent 5 activities for both genders. The Figure
shows closeness within the same cluster, and distinctive gap
between the clusters. 

| <img src="https://user-images.githubusercontent.com/61229902/171030205-2d3dbf35-2f77-423f-9e3f-adf37d19d3a4.png"  width="70%"  class="img-responsive"> |
|:---:|
| Scatter Plot of 5 Samples of Acceleration Data For All Combinations in MotionSense dataset |
### → It is worth to note that the cut and feature-extracted data are available upon request.

## Experinmental Setup

A. Feature Extraction <br>
In order to reduce the complexity of the training and testing processes, we used the ACF as a feature extractor. In general, the autocorrelation function takes an input value of the lag $h$ and outputs the autocorrelation value at that lag $\rho(h)$. Of course, given the data samples, we are only able to give an estimate of the autocorrelation function $\hat{\rho}(h)$. 

B. Joint Distribution Model  <br>
The joint recognition approach is based on jointly classify both the activity and gender simultaneously.
From a probabilistic perspective, joint recognition computes the joint probability of two random variables representing the activity and gender $P(A,G)$. This model is taking the whole dataset as an input and construct a learning model. 

C. Hierarchical Model <br>
The output of interest is described by two latent random variables $A$ for activity and $G$ for gender. It is easy to see that we can improve the accuracy of the classification by separating the classification into two stages. The first stage will be classification based on the activity and once it is determined we can proceed through the second stage and predicts the gender.

This two-stage model is expected to show higher accuracy than the joint predictive model, and this is evident using Bayes' theorem:
$$ P(G|A) = \frac{P(A,G)}{P(A)} $$

The joint predictive model is equivalent to computing the joint probability $P(A,G)$, whereas the hierarchical
model is equivalent to computing the conditional probability $P(G|A)$ which is generally higher than $P(A,G)$ as the latter is divided by $P(A)$.

| <img src="https://user-images.githubusercontent.com/61229902/171032304-4e762058-bb74-439f-9a9d-f234d81097d4.jpeg" width="500" height="400" class="img-responsive"> |
|:---:|
| Two stage recognition model for (activity,gender). |

## Results
We explored the accuracy and F-measure for both datasets using RF, SVM, and CNN, all in the two approaches, Joint approach and Hierarchical approach. The table below  summarizes all the results of these experiments.

| <img src="https://user-images.githubusercontent.com/61229902/171033620-854278a4-1f86-4655-987b-4c1e7638584d.jpeg" class="img-responsive"> |
|:---:|
| Summarizing the results of all experiments |

<br>


<!-- - The main.rar file includes the raw data extracted from the .mat files downloaded, while the FINAL.rar resembles the data overlapped preprocessed, and used for the training -->

