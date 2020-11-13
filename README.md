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

Goal: Run turtlebot3 with navigation in a simulated brown hall.

So far I have the robot driving in a simulated brown hall with wall collisions working. Pretty Cool!

1) Start with blueprint of the building from Dr. Canfield.

2) Make a .stl from the blueprint of the 3rd floor using a CAD program of your choice. Joseph Owens did this part.

3) Make a 'collada mesh' as a .dae file, this can be done with 'blender' software app or 'meshlab'. I used blender in ubuntu 18.04. 

i) open blender
ii) import .stl
iii) export .dae

4) Create a world file from scratch in the turtlebot3 package directory. 

`gedit bh3.world`

copy in the following code (http://gazebosim.org/tutorials?tut=import_mesh&cat=build_robot)
notice it mentioned the .dae file to load

```
<?xml version="1.0"?>
<sdf version="1.4">
<world name="default">
  <include>
    <uri>model://ground_plane</uri>
  </include>
  <include>
    <uri>model://sun</uri>
  </include>
  <model name="my_mesh">
    <pose>0 0 0  0 0 0</pose>
    <static>true</static>
    <link name="body">
      <visual name="visual">
        <geometry>
          <mesh><uri>file://bh3.dae</uri></mesh>
        </geometry>
      </visual>
    </link>
  </model>
</world>
</sdf>
```


5) Test the new world file. You should be in brown hall when gazebo opens. 

`gazebo bh3.world`

That all worked just fine, but I do not think that is not exactly the way we are going to do it.



6) Instead creating a world from scratch, Bring turtlebot3_simulations into the workspace. We are going to create a new world inside of this package. We will use the already created worlds as a template.


`git clone https://github.com/ROBOTIS-GIT/turtlebot3_simulations.git`


Inside of this package I am going to create a new 'world' for turtlebot3 in live inside of Brown Hall


i) collect the meshes that are going to be in the model, they should be collada meshes (.dae)

ii) create a new .world file 

`gedit /home/thill/t540p_ros/src/turtlebot3_simulations/turtlebot3_gazebo/worlds/turtlebot3_bh3.world`

make the appropriate edit to the model name

```
<!-- Load world -->
<include>
<uri>model://turtlebot3_bh3</uri>
</include>
```

iii) create a new model, this requires a new folder for the model

you will have to edit the files: model.config and model.sdf  

copy the .dae files into the sub folder /meshes 

these files will be referenced in the model.sdf file


Here is the new model.config file.

____________________________________________________
```
<?xml version="1.0"?>

<model>
<name>TurtleBot3 bh3</name>
<version>1.0</version>
<!--<sdf version="1.4">model-1_4.sdf</sdf>-->
<sdf version="1.5">model.sdf</sdf>  

<author>
<name>Tristan Hill</name>
<email>thlim@robotis.com</email>
</author>

<description>
World of TurtleBot3 in Brown Hall
</description>
</model>
```
_____________________________________________________


Here is the new model.sdf file.

_____________________________________________________________________________________
```
<sdf version='1.4'>

<model name='bh3_walls'>
<static>1</static>
<link name='bh3_walls'>

<!-- Draw Wall -->
<collision name='body'>
  <pose>0 0 -0.3 0 0 -1.5708</pose>
  <geometry>
  <mesh>
      <uri>model://turtlebot3_bh3/meshes/bh3_walls.dae</uri>
      <scale>0.25 0.25 0.25</scale>
  </mesh>
  </geometry>
  <max_contacts>10</max_contacts>
  <surface>
      <bounce/>
      <friction>
          <ode/>
      </friction>
      <contact>
          <ode/>
      </contact>
  </surface>
</collision>

<visual name='body'>
<pose>0 0 -0.3 0 0 -1.5708</pose>
<geometry>
  <mesh>
      <uri>model://turtlebot3_bh3/meshes/bh3_walls.dae</uri>
      <scale>0.25 0.25 0.25</scale>
  </mesh>
</geometry>
<material>
  <script>
      <uri>file://media/materials/scripts/gazebo.material</uri>
      <name>Gazebo/FlatBlack</name>
  </script>
</material>
</visual>
</link>
</model>
</sdf>
```__________________________________________________________________________________



iv) create a new launch file to test this all 

```
gedit /home/thill/t540p_ros/src/turtlebot3_simulations/turtlebot3_gazebo/launch/turtlebot_bh3.launch  
```

you will have to edit the following line with the new world name
```
<arg name="world_name" value="$(find turtlebot3_gazebo)/worlds/turtlebot3_bh3.world"/>    
```

7)Now test! Launch this and the teleop drive and you should have a turtlebot3 in Brown hall that you can drive with the keyboard. The walls are there as objects and everything! The next thing is to try navigation but it should work no problem.

```
roslaunch turtlebot3_gazebo turtlebot3_bh3.launch
```
```
roslaunch turtlebot3_teleop turtlebot3_teleop_key.launch
```


