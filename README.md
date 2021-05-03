# ORB-SLAM3 SVE
ORB-SLAM3 SVE (or ORB-SLAM3 with Scene Visbility Estimation) is a modified version of the original ORB-SLAM3 SLAM framework. The original GitHub repository is located [here](https://github.com/UZ-SLAMLab/ORB_SLAM3). 

The modifications were made in support of my final year research project in partial fulfilment of the requirements for my degree at the University of Sheffield. The research project is titled 'Online Scene Visibility Estimation as a Complement to SLAM in UAVs', and can be found here.

The full repository containing supplementary code for dataset packaging and processing is located [here](https://github.com/rdehaggart1/sceneVisibilityInSLAM)

NOTE: In my experience, ORB-SLAM3 still struggles with a number of bugs that can significantly affect the operation of the algorithm. If you are looking to test with Monocular V-SLAM I would recommend ORB-SLAM2 instead. ([ORB-SLAM2](https://github.com/raulmur/ORB_SLAM2), [ORB-SLAM2 SVE](https://github.com/rdehaggart1/ORB_SLAM2_SVE)).

# 1. Original Authors and Decription

**Authors:** [Carlos Campos Martinez](https://scholar.google.es/citations?user=QI9psw0AAAAJ&hl=es), Richard Elvira, [Juan J. Gómez Rodríguez](https://scholar.google.com/citations?user=jOKMlwsAAAAJ), [José María Martínez Montiel](http://webdiis.unizar.es/~josemari/), [Juan Domingo Tardós Solano](http://webdiis.unizar.es/~jdtardos/)

ORB-SLAM3 is the first real-time SLAM library able to perform Visual, Visual-Inertial and Multi-Map SLAM with monocular, stereo and RGB-D cameras, using pin-hole and fisheye lens models. In all sensor configurations, ORB-SLAM3 is as robust as the best systems available in the literature, and significantly more accurate.

# 2. Dependencies

Please see the [original ORB-SLAM2 repository](https://github.com/raulmur/ORB_SLAM2) and ensure that before using this repository, you have all the dependincies required by the original installed on your machine.

# 3. Modifications

The set modifications made to the original source code in support of this project is described below:

<i>Frame.cc</i>
- Calculate SVE_a by dividing the number of extracted features by the user-defined maximum.
- Calculate SVE_b by segmenting each frame into a number of bins, and counting the number of features extracted in each. Use this distribution to calculate the chi-square histogram distribution statistic against a homogenous distribution of points.

<i>Tracking.cc</i>
- Count how many of the existing points in the local map theoretically are located within the frustum of the camera, then calculate SVE_c by working ut how many of these map points have been successfully tracked in each frame.

<i>System.cc</i>
- Combine the 3 metrics into a single value, and for each frame, store all components with a timestamp in vectors. 
- A new function then accesses these vectors and writes all values to a file 'SceneVisibilityEstimation.txt' for analysis.

<i>Extra</i>
- The project is renamed to `ORB_SLAM2_SVE` so the ROS package is distinct from the original. Similar adjustments are made throughout for build support and readability. 
- .yaml setup files are included for the datasets tested through this project in `../Examples/Monocular/`.
