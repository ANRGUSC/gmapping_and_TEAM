# GMapping and Trilateration for Exploration and Mapping

GMapping:  
source code: https://github.com/ros-perception/slam_gmapping  
documentation: https://wiki.ros.org/gmapping?distro=kinetic  
approach: https://openslam-org.github.io/gmapping.html  

TEAM:  
Lillian Clark, Charles Andre, Joseph Galante, Bhaskar Krishnamachari, Konstantinos Psounis, “TEAM: Trilateration for Exploration and Mapping with Robotic Networks.” Submitted to IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS), Las Vegas, NV, USA October 2020. (pending review)

TurtleNet is a project out of the Autonomous Networks Research Group at the University of Southern California investigating multi-robot exploration and mapping via ultrawide-band trilateration.

See also:   
https://github.com/ANRGUSC/TurtleNet  
https://github.com/ANRGUSC/gmapping_and_TEAM  

Our testbed is comprised of several Turtlebot3 Burgers integrated with Pozyx Anchors.

Turtlebot3 Burgers  
specs: http://emanual.robotis.com/docs/en/platform/turtlebot3/specifications/#specifications  
source code: https://github.com/ROBOTIS-GIT/turtlebot3  

Simulating in Gazebo  
tutorial: http://emanual.robotis.com/docs/en/platform/turtlebot3/simulation/#turtlebot3-simulation-using-gazebo  

Pozyx Anchor  
specs: https://www.pozyx.io/shop/product/creator-anchor-69  
python library: https://pypozyx.readthedocs.io/en/develop/  

### This repository

This respository contains a wrapper around the gmapping package which provides SLAM capabilities. The gmapping package itself has been augmented to also perform TEAM, a novel algorithm for multi-robot systems which uses trilateration to localize. Trilateration is performed separately (see https://github.com/ANRGUSC/TurtleNet).

To use this repository, review the SLAM tutorial here:  
http://emanual.robotis.com/docs/en/platform/turtlebot3/slam/#ros-1-slam  
Packages related to gmapping have already been installed if you've followed the instructions here:  
http://emanual.robotis.com/docs/en/platform/turtlebot3/pc_setup/#install-dependent-ros-packages  

Remove the already installed openslam_gmapping and slam_gmapping (likely in /opt/ros/kinetic/share) such that you don't have two conflicting versions.

In your catkin_ws/src directory, git clone openslam_gmapping:  
https://github.com/ros-perception/openslam_gmapping  
And git clone this repository.  

Rebuild your workspace.

#### gmapping
`slam_gmapping.cpp` contains all the code to perform GMapping or the mapping code to perform TEAM, depending on the input `policy`.

By default `policy` = `slam`, and this ROS Node will initialize a particle filter. Each time the map is updated, the most probably particle history is the assumed location history.

If `policy` = `clam` (for cooperative/collaborative localization and mapping), this ROS Node will subscribe to position estimates given by trilateration on the `/pozyx_position` topic. Each time the map is updated, the position estimates is the assumed location.

If `policy` = `odom`, this ROS Node will subscribe to the `odom` topic. Each time the map is updated, the odometry estimate (plus a non-zero initial position if given) is the assumed location.

Under each policy, the map (an Occupancy Grid) and map metadata are published over the `map` and `map_metadata` topics.
