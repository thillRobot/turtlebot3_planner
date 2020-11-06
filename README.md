# turtlebot3_planner

This is a ROS package for planning and executing simple plans and missions for Turtlebot3

### try a simple test


`roslaunch turtlebot3_gazebo turtlebot3_world.launch`

`roslaunch turtlebot3_navigation turtlebot3_navigation.launch map_file:=/home/thill/me4140_ros/src/turtlebot3_planner/maps/map0.yaml`

Use `find` so that you do not have to include the entire path

`roslaunch turtlebot3_navigation turtlebot3_navigation.launch map_file:='$(find turtlebot3_planner)/maps/map0.yaml'`

