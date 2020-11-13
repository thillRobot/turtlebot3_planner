# turtlebot3_planner

This is a ROS package for planning and executing simple plans and missions for Turtlebot3

### try a simple test


`roslaunch turtlebot3_gazebo turtlebot3_world.launch`

`roslaunch turtlebot3_navigation turtlebot3_navigation.launch map_file:=/home/thill/me4140_ros/src/turtlebot3_planner/maps/map0.yaml`

Use `find` so that you do not have to include the entire path

`roslaunch turtlebot3_navigation turtlebot3_navigation.launch map_file:='$(find turtlebot3_planner)/maps/map0.yaml'`


### launch the turtle in Brown Hall - 3rd Floor
This is going to use a 'gazebo plugin' to build a different world -> Brown Hall 

you have to set these paths before it will work or put the exports in `~/.bashrc`

```
export GAZEBO_PLUGIN_PATH=~/<path>/my_package_example/lib:${GAZEBO_PLUGIN_PATH}
export GAZEBO_MODEL_PATH=~/<path>/my_package_example/models:${GAZEBO_MODEL_PATH}
export GAZEBO_RESOURCE_PATH=~/<path>/my_package_example/models:${GAZEBO_RESOURCE_PATH}
```

I learned this by reading this post (https://answers.gazebosim.org//question/1940/problem-with-including-a-model/)

now try to launch the robot in brown hall

`roslaunch turtlebot3_planner turtlebot3_planner.launch`

and turn on a keyboard node and you can drive the robot around in gazebo brown hall


### creating and editing a world in Gazebo


