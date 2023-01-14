### Overview
This folder holds python scripts for GRAVL

### Files

#### `circle_drive.py`
Drives Kubo in a circle at a slow speed to determine wheel slippage. To run, first start a roscore by running the command `roscore` in a new Terminal.
Then, in another Terminal, start the state machine controller by running the command `rosrun tractor State`. Finally, in a new Terminal, execute
the circle driving script by running the command `circle_drive.py`. Make sure that Kubo is set up and running by following the instructions on the homepage
of the Github GRAVL wiki.

#### `ConvertToAckermann.py`
Converts from `/cmd_twist` geometry_msgs/Twist topic to `/cmd_vel` ackermann_msgs/AckermannDrive topic. Interpolates velocity from [-1.0, 1.0] to [-2.0, 2.0]. Interpolates angle from [-1.0, 1.0] to [-45.0, 45.0]. Run separately through `rosrun gravl ConvertToAckermann.py` - must be running to connect state controller with tractor.

#### `test_pointgrey.py`
Displays raw imagery from a pointgrey camera publishing through ROS. To run, first setup a pointgrey camera such that it is
publishing over ROS by looking at this wiki page: ([link](https://github.com/olinrobotics/gravl/wiki/Kubo:-Cameras))  Then,
run the pointgrey program by entering `python pointgrey.py` in the Terminal.

#### `pointgrey_bag.py`
Displays bag file of pointgrey camera imagery. To run, first setup a pointgrey camera such that it is
publishing over ROS by looking at this wiki page: ([link](https://github.com/olinrobotics/gravl/wiki/Kubo:-Cameras))  Then,
run the program by entering `python pointgrey_bag.py "filename"`, where `"filename"` is the name of a rosbag file of the
`/camera/image_raw` topic.

#### `LidarCodeBasic.py`
Takes a scan topic from LIDAR to track obstacles. To run, first either run a node that produces a scan topic (e.g. rosrun urg_node urg_node) or the bags labeled tractorLidar*. Open rviz with rosrun rviz rviz. Press the + button and add by topic /scan. Change the box that has 'map' to '/laser'. You should be able to visualize the data. Now run the program (rosrun gravl LidarCodeBasic.py) and data will be published. /estop is whether there is an obstacle in the way, /scan_verticals is the distance from the tractor of the obstacle, /scan_horzontals is the distance horizontally from the center of the tractor.

#### `LidarFollower.py`
Takes a scan topic from LIDAR to track obstacles. To run, first either run a node that produces a scan topic (e.g. rosrun urg_node urg_node) or the bags labeled tractorLidar*. For debugging, open rviz with rosrun rviz rviz. Press the + button and add by topic /scan. Change the box that has 'map' to '/laser'. You should be able to visualize the data. Now run the program (rosrun gravl LidarFollower.py) and data will be published. /autodrive is the steering angle and speed to go towards the 'obstacle', /scan_verticals is the distance from the tractor of the obstacle, /scan_horzontals is the distance horizontally from the center of the tractor.

#### `BackOnTrack.py`
Takes a scan topic from LIDAR to track obstacles. To run, first either run a node that produces a scan topic (e.g. rosrun urg_node urg_node) or the bags labeled tractorLidar*. For debugging, open rviz with rosrun rviz rviz. Press the + button and add by topic /scan. Change the box that has 'map' to '/laser'. You should be able to visualize the data. Now run the program (rosrun gravl LidarFollower.py) and data will be published. /autodrive is just a place holder for now because i don't know where to publish to, but it publishes the steering angle and speed to go towards the road i think, very experimental, /scan_verticals is the distance from the tractor of the obstacle, /scan_horzontals is the distance horizontally from the center of the tractor.

#### `gps_navigation_node.py`
Takes latitude, longitude, and heading data from a topic and latitude and longitude data from waypoints and uses this data to produce ackermann drive velocity and steering angle messages  . To run, first setup the Hemisphere Vector GPS to publish heading data and the Swift Navigation RTK GPS to publish current latitude and longitude data. Then, run the program by entering `gps_navigation_node.py` in the Terminal.

#### `recognize_road.py`
Tries to recognize the road.
At the moment, all it does is display an annotated video stream of a road, but will be modified to publish probably ackermann messages.
To run, start `roscore`, play the rosbags which can be found on a usb in the lab, and `rosrun tractor road_recognition`.
The script will subscribe to the topic `/camera/image_raw`.
The important bit of code is the callback which establishes the publisher, and the recognize_road function which at the moment returns an annotated picture, but should soon give the tractor directions.

#### `RecordPoints.py`
Subscribes to `/tractor_odom` and starts a tkinter GUI with Record Point and Save Data buttons. When Record Point is clicked, it saves the last 2D x-y odometry point recieved. When Save Data is clicked, it writes the data to a file that can be used in the cut_planner package.

#### `TractorOdom.py`
Uses orientation data from `/imu/data` and position data from `/piksi/navsatfix_best_fix` and creates an odometry message that is published to `/tractor_odom`. Also broadcasts this odometry message as the transform between `/base_link` and `/odom`
