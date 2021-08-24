# clover_task
This repository localise a car equipped with different odometry sources given to it.

### Instructions
- User must have ros installed on the station. For more information check the following link:
`http://wiki.ros.org/`

- User must also have the robot localization package installed. Use the following command to install it (for ubuntu package manager users):
`sudo apt-get install ros-melodic-robot-localization -y`

- Clone the package at `~/catkin_ws/src`. Use the command `catkin_make` at `~/catkin_ws`.

- After the package is built, run `roscore` in the terminal.

- Use the following command to play the rosbag:
```
rosbag play data.bag
```

- Open another terminal and use the comand `source ~/catkin_ws/devel/setup.bash` to source the package. Afterwards run the following command:
```
roslaunch clover_task robot_lacalisation.launch
```
- Open another terminal and launch rviz:
```
rosrun rviz rviz
```
Press Ctrl+O 
Navigate to `~/catkin_ws/src/clover_task/robot_pos_config.rviz` and choose this file.

The output will show ground truth odom and filtered odom.

### Description
The basic idea was to localize the robot given different odometry sources. First step I did was to check the rosbag by using command `rosbag info --freq data.bag` for see some information. This is what i found:

```javascript
path:        data.bag
version:     2.0
duration:    1:50s (110s)
start:       Jul 30 2021 20:52:26.18 (1627660346.18)
end:         Jul 30 2021 20:54:16.82 (1627660456.82)
size:        9.0 MB
messages:    12163
compression: none [12/12 chunks]
types:       nav_msgs/Odometry [cd5e73d190d741a2f92e81eda573aca7]
topics:      /sensors/gnss/odom           1106 msgs @ 10.0 Hz : nav_msgs/Odometry
             /sensors/odom                5528 msgs @ 76.5 Hz : nav_msgs/Odometry
             /sensors/odom/ground_truth   5529 msgs @ 76.8 Hz : nav_msgs/Odometry
```

Then I plotted the data for get some idea of what the robot is doing and how to proceed. The following is the data from gnss.

![gnss_odometry](https://user-images.githubusercontent.com/54701432/130675655-63ed5ed7-4257-43fc-9fc7-50005345c7c8.png)

This is the Ground truth:
![ground_truth](https://user-images.githubusercontent.com/54701432/130675820-050e1a03-3a40-4f9c-ad62-0d7ddc1f89af.png)

Afterwards, I added the two sources ( gnss and raw odom) to the ekf and compared the output with the ground truth. The resulting image in rviz is as follows:
![rviz_comparasion_gnss_ground_truth](https://user-images.githubusercontent.com/54701432/130676152-9ab3eb4c-1223-4f31-b81d-df668226a06b.png)
