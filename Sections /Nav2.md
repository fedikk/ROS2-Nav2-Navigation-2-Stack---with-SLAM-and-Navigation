# ROS2 Nav2 [Navigation 2 Stack] - with SLAM and Navigation

## What is Navigation2 Stack, Why do we need it ?    

<blockquote> Navigation = Navigation2 = Nav2 </blockquote>
      
Benefits of ROS2: 
  * Create the base layer super fast
  * Provide a standard for robotics application
  * use on any robot
  * open source community
Whats navigation 2 Stack :
  - a collection of ROS packages created to acheive a specific goal :
  - the Robot must move safely from point A to Point B
  - 2 Step-Process
    1. create a map with SLAM
    2. MAke the robot navigate from point A to point B
# Section 2 : Setup and Installation for ROS2 and Nav2
## Setup and installation 
we are going to use __`ROS2 Humble`__ with __`Ubuntu 22.04`__

## PLEASE READ - DO NOT USE ROS2 FOXY
I'm just adding a quick note here to urge you not to use ROS2 Foxy, and instead go with a recent and supported version, such as ROS2 Humble.

The reason for this note is that many are still trying to work with Nav2 and Foxy, and unfortunately there are a lot of bugs and hard-to-solve issues. Also, the Simple Commander API won't work in Foxy.

And there is simply no solution to that, as Foxy is not supported anymore.

So, I strongly recommend that you install Ubuntu 22.04 and ROS2 Humble.

If you are still on Ubuntu 20.04, it might seem like a lot of setup and wasted time.

I can guarantee you that you will waste way more time trying to fix issues with Nav2 on ROS2 Foxy.

So, take the time to install ROS2 Humble, and this small time investment will actually save you a lot of time in the mid/long term :)

## [Recap] Install and Setup ROS2 on Ubuntu 22.04

Seen in ROS2_For_beginners

## Install the Navigation 2 Stack

```bash
sudo apt update
```
```bash
sudo apt upgrade
```
```bash
sudo apt autoremove
```
```bash
sudo apt install ros-humble-navigation2 ros-humble-nav2-bringup ros-humble-turtlebot3*
```

## Install the Tools We’ll Need For the Course

```bash
sudo apt install python3-colcon-common-extensions
```
```bash
sudo apt install git
```
```bash
sudo apt install terminator
```
```bash
sudo apt install code --classic
```
___

# Section 3 : Generate a Map with SLAM (Simultaneous Localization and Mapping)

[SLAM+Steps+(ROS2+Nav2+Course+-+Section+3).pdf](https://github.com/fedikk/ROS2-Nav2-Navigation-2-Stack---with-SLAM-and-Navigation/files/15121921/SLAM%2BSteps%2B.ROS2%2BNav2%2BCourse%2B-%2BSection%2B3.pdf)

```bash
gedit ~/.bashrc 
``` 

add this line : 

```
export TURTLEBOT3_MODEL=waffle
```

to Run the __`TurtleBOT3 world`__ run this command : 
```bash
ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py
```
 you must see something like this : 

![image](https://github.com/fedikk/ROS2-Nav2-Navigation-2-Stack---with-SLAM-and-Navigation/assets/98516504/31f8c435-65d5-460f-9bce-17ae3907df8c)

to controll the robot mouvement from the keyboard : 

```bash
ros2 run turtlebot3_teleop teleop_keyboard
```
## Generate and Save a Map with SLAM in ROS2
Run the commands simultanly : 
### 1 
```bash
ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py
```
### 2 
```bash
ros2 launch turtlebot3_cartographer cartographer.launch.py use_sim_time:=True
```
### 3 
```bash
ros2 run turtlebot3_teleop teleop_keyboard
```
After moving the robot in the world you must got somthing like this : 

![map](https://github.com/fedikk/ROS2-Nav2-Navigation-2-Stack---with-SLAM-and-Navigation/assets/98516504/7c250671-9463-449d-881c-f8aa9acd791d)

### 4
```bash
cd && mkdir maps
```
### 5
```bash
ros2 run nav2_map_server map_saver_cli -f ~/maps/my_map
```
```plaintext
[INFO] [1714081189.367483320] [map_saver]: 
	map_saver lifecycle node launched. 
	Waiting on external lifecycle transitions to activate
	See https://design.ros2.org/articles/node_lifecycle.html for more information.
[INFO] [1714081189.367973914] [map_saver]: Creating
[INFO] [1714081189.373619976] [map_saver]: Configuring
[INFO] [1714081189.379934597] [map_saver]: Saving map from 'map' topic to '/home/ros/maps/my_first_map' file
[WARN] [1714081189.380015574] [map_saver]: Free threshold unspecified. Setting it to default value: 0.250000
[WARN] [1714081189.380046128] [map_saver]: Occupied threshold unspecified. Setting it to default value: 0.650000
[WARN] [map_io]: Image format unspecified. Setting it to: pgm
[INFO] [map_io]: Received a 124 X 116 map @ 0.05 m/pix
[INFO] [map_io]: Writing map occupancy data to /home/ros/maps/my_first_map.pgm
[INFO] [map_io]: Writing map metadata to /home/ros/maps/my_first_map.yaml
[INFO] [map_io]: Map saved
[INFO] [1714081191.092074723] [map_saver]: Map saved successfully
[INFO] [1714081191.099774816] [map_saver]: Destroying
```

<blockquote> The Process of saving map may not run in the first times repeat it until it works </blockquote>

## Saved Map 

![maps_folder](https://github.com/fedikk/ROS2-Nav2-Navigation-2-Stack---with-SLAM-and-Navigation/assets/98516504/c07699e8-dc95-4c84-b0e7-c514bdec876c)
![my_first_map](https://github.com/fedikk/ROS2-Nav2-Navigation-2-Stack---with-SLAM-and-Navigation/assets/98516504/1dd483f5-8cd5-45a8-9bd7-6f74df297feb)

## What’s Inside the Generated Map? 

### my_first_map.yaml
```yaml
image: my_first_map.pgm
mode: trinary
resolution: 0.05
origin: [-1.25, -2.41, 0]
negate: 0
occupied_thresh: 0.65
free_thresh: 0.25
```
when you open __`my_first_map.pgm`__ :
```
P5
124 116
255
```
then : 124 * 0.05 = 6.2 m on X axis 
       116 * 0.05 = 2.8 m on Y axis

##  SLAM Activity - Create a New Map in Turtlebot3 House

Generate the map of TurtleBOT3 House !!

Hint :  to open the house world 
```bash
ros2 launch turtlebot3_gazebo turtlebot3_house.launch.py
```
### challenge_TurtleBOT3_house_map.yaml
```yaml
image: my_house.pgm
mode: trinary
resolution: 0.05
origin: [-5.76, -5.14, 0]
negate: 0
occupied_thresh: 0.65
free_thresh: 0.25
```
___ 

# Section 4 : Make a Robot Navigate with Nav2

## A Quick Fix We Need To Do Before Starting

we are going to fix the lag on loading the map 

follow this commands in the same order 

```bash 
sudo apt update && sudo apt upgrade 
```
```bash 
sudo apt install ros-humble-rmw-cyclonedds-cpp
```
```bash
gedit ~/.bashrc 
```
### .bashrc ending  
```bashrc
export TURTLEBOT3_MODEL=waffle
export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp
source /opt/ros/humble/setup.bash
source /usr/share/colcon_argcomplete/hook/colcon-argcomplete.bash
source ~/fedi_ws/install/setup.bash
source /usr/share/gazebo/setup.bash
source ~/ros2_ws/install/setup.bash
```

```bash
cd  ~/opt/ros/humble/share/turtlebot3_navigation2/param && sudo gedit waffle.yaml
```
### waffle.yaml 
```yaml
#robot_model_type: "differential"
robot_model_type: "nav2_amcl::DifferentialMotionModel"
```

## Make the Robot Navigate Using the Generated Map

first of all launch the robot : 
```bash
ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py
```
then launch launch the __turtleBOT3_navigation2__ 
```bash
ros2 launch turtlebot3_navigation2 navigation2.launch.py use_sim_time:=True map:=/maps/my_first_map.yaml 
```
### RViz

![map_loaded on rviz](https://github.com/fedikk/ROS2-Nav2-Navigation-2-Stack---with-SLAM-and-Navigation/assets/98516504/85f7a4a4-06d9-4143-acc1-2488516dd593)

we need first to give a **2D Pose** for the robot after Select **Navigation Goal** : 

![2D pose](https://github.com/fedikk/ROS2-Nav2-Navigation-2-Stack---with-SLAM-and-Navigation/assets/98516504/af89391a-9d16-45f5-8203-8488ab78843b)


https://github.com/fedikk/ROS2-Nav2-Navigation-2-Stack---with-SLAM-and-Navigation/assets/98516504/ae99e386-6337-477f-9a69-3d0e52e1253b

## Waypoint Follower - Go Through Multiple Nav2 Goals

we can go a multiple goal to go **A** then **B** then **C**

1. Set the __`Pose`__ of the robot
2. Click on Waypoint :
 
   ![waypoint](https://github.com/fedikk/ROS2-Nav2-Navigation-2-Stack---with-SLAM-and-Navigation/assets/98516504/07ed4764-67bf-4e0a-a66f-5ef4c3752b3e)
3. Click on **`Nav2 Goal and set the point you want`**
   ![waypoints_goals](https://github.com/fedikk/ROS2-Nav2-Navigation-2-Stack---with-SLAM-and-Navigation/assets/98516504/39323ee9-b2fa-45c5-834f-798dacdc52ec)
4. finally, Press on **`Start Nav Through Poses`**

# Dynamic Obstacle Avoidance

if you choose a goal for the robot and the path was planned its update every time a new obstacle is detected.

# Section 5 : Understand the nav2 stack 
## Global Planner, Local Planner and Costmaps
we have to main parts : 
1. the global planner
2. the locale planner 

### STEP1 : Create the path 
 Let's focus on the __`global planner`__ : 
 - will compute the path between the **robot pose** and the **goal pose**
 - How does it compute the path  ?
 - it uses the entire map to compute the best path based on **`cost map ( where each pixel has a cost ) `** the colorfull one
 - the white pixel has **`0 cost`** and the darker pixels has **`maximum cost`**
 - we have some kind of  **`safety margin`** : the one on **`blue`**
 - the global plan will be updated every **` 1 2 or 3 Hz`**
 ![cost_map](https://github.com/fedikk/ROS2-Nav2-Navigation-2-Stack---with-SLAM-and-Navigation/assets/98516504/3b170176-6e62-43fe-94a4-6605a2341288)
### STEP2 : Sending the Path to the local planner 
- the controller will control the robot to follow the path : **`see the blue line`**
- the __update_rate__ for this controller will be higher : **`20 50 100 Hz`**

![local_costmap](https://github.com/fedikk/ROS2-Nav2-Navigation-2-Stack---with-SLAM-and-Navigation/assets/98516504/cfb6ee32-a148-4185-a201-274619749cf7)

  
Real Life analogie :
- imagine that we are drinving a car with a GPS system : you tell the GPS you want to go to a specific GOAL
- the GPS will tell you the path and where to go and you controll the car to follow the path 

## Parameters

launch : 
   - rqt > Plugins > Configuration > Dynamic Reconfiguration > search for Glaobal costmap

![Global_cost_map_rqt](https://github.com/fedikk/ROS2-Nav2-Navigation-2-Stack---with-SLAM-and-Navigation/assets/98516504/5d67ced6-fcb3-4a54-8c41-eeda1a9c8a16)

- we are going to change the **`inflation_layer.inflation_radius`** from **`0.55`** to **`0.25`** 

- ![Map_with_new_inflation_value](https://github.com/fedikk/ROS2-Nav2-Navigation-2-Stack---with-SLAM-and-Navigation/assets/98516504/10c6a043-f4e0-4e4e-89bc-2e1bab2179b9)

- the map is much more clearer now because the __inflation radius__ is  the radius from the obstacle that where we still have a cost for the pixels so 0.25 means that we still have cost __20 cm__ from the obstacle 

launch : 
   - rqt > Plugins > Configuration > Dynamic Reconfiguration > search for Local costmap

![local_cost_map_rqt](https://github.com/fedikk/ROS2-Nav2-Navigation-2-Stack---with-SLAM-and-Navigation/assets/98516504/b31c454e-1688-4a2c-be5e-95a18605efc4)


- we are going to change the **`inflation_layer.inflation_radius`** from **`1.0`** to **`0.5`** 

![the_new_local_map](https://github.com/fedikk/ROS2-Nav2-Navigation-2-Stack---with-SLAM-and-Navigation/assets/98516504/a8a5583a-726c-4ff5-b169-9a722a462ae2)

__on the controller server you can change the velocity max and min and many other parameters__ .

# TFs and important Frames
launch the gazeo and navigation2 then 

```bash
ros2 run tf2_tools view_frames -o turtlebot
``` 

[turtlebot.pdf](https://github.com/fedikk/ROS2-Nav2-Navigation-2-Stack---with-SLAM-and-Navigation/files/15130999/turtlebot.pdf)


## Nav2 Architecture 

## Detailed : 
![image](https://github.com/fedikk/ROS2-Nav2-Navigation-2-Stack---with-SLAM-and-Navigation/assets/98516504/02d645de-29f3-4f1d-b307-1b6f4efdfc14)

## Simplified
![nav2_architecture ](https://github.com/fedikk/ROS2-Nav2-Navigation-2-Stack---with-SLAM-and-Navigation/assets/98516504/7f389869-027e-4b49-ba19-d0305e874247)

- in order to fonction Nav2 requires some inputs : __`TFs + Map + Sensor Data`__
- **navigation Goal** : Position + orientation will be the input for the __planner server__(global planner) who will give the **path** as an input to
-  **local_planner**(controller) that will make sure that the robot follows the path and reaches the goal .
- then we will pass the  __`Robot HW controller `__
- __recovery Beheviour__ : when an unusall thing happen 

# Build Your Own World for Navigation in Gazebo 

you can see [ROS2_For_Beginners_Level2](https://github.com/fedikk/ROS2-for-Beginners-Level-2-) 

## import a Floor Plan

1. open gazebo the open the __`building editor(Ctrl+b)`__  : 

![world_plan](https://github.com/fedikk/ROS2-Nav2-Navigation-2-Stack---with-SLAM-and-Navigation/assets/98516504/fb87d6e6-a3cb-4da4-9fc2-ef37d5066684)

2. then select the door and give the reel dimension

   ![plan_gazebo](https://github.com/fedikk/ROS2-Nav2-Navigation-2-Stack---with-SLAM-and-Navigation/assets/98516504/0b56552e-6ca4-4966-9fe0-052b45477767)

4. then make your walls

![designed_world_gazebo](https://github.com/fedikk/ROS2-Nav2-Navigation-2-Stack---with-SLAM-and-Navigation/assets/98516504/81646ff9-163c-4f09-ac9b-7da5faf7689d)

## Add objects to the world 

go to insert in gazebo and just drag and drop any item .

After saving your world for example : __`test_world.world`__ in my __`Desktop`__ : 

```bash
cd ~/Desktop && gazebo test_world.world
```

![my_world_gazebo](https://github.com/fedikk/ROS2-Nav2-Navigation-2-Stack---with-SLAM-and-Navigation/assets/98516504/26e470d1-5d65-4e60-a77b-96486fd2e767)


## Make Turtlebot3 Navigate In That World

first of all we are going to work on a copy of turtleBot3 repo that we are going to clone from github .

```bash
mkdir turtlebot3_ws && cd turtlebot3_ws && mkdir src && cd src
```
```bash
git clone https://github.com/ROBOTIS-GIT/turtlebot3_simulations.git .
```
```bash
cd turtlebot3_simulations/
```
```bash
git checkout humble-devel
```
```bash
$ git branch 
* humble-devel
  master
```
```bash
$ colcon build 
```

dont forget to edit ~/.bashrc and to add 
```bash
source ~/turtlebot3_ws/install/setup.bash
```

open a new terminal and everything should work properly for example launch turtlebot3 world : 
```bash
ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py
```

Let's now include our world : 
```bash
cd turtlebot3_ws/src/turtlebot3_simulations/turtlebot3_gazebo/launch
```
copy your `test_world.world` inside the **`turtlebot3_ws/src/turtlebot3_simulations/turtlebot3_gazebo/maps`** folder

then edit your file by adding : 

```xml
<?xml version="1.0"?>
```
![adding xml](https://github.com/fedikk/ROS2-Nav2-Navigation-2-Stack---with-SLAM-and-Navigation/assets/98516504/a3f0cbcd-a928-45e9-91df-3e2d372152b8)

copy what's inside **`turtlebot_house.launch.py`** inside an new file :

### turtlebot3_test_world.launch.py

```python
#!/usr/bin/env python3


import os

from ament_index_python.packages import get_package_share_directory
from launch import LaunchDescription
from launch.actions import IncludeLaunchDescription
from launch.launch_description_sources import PythonLaunchDescriptionSource
from launch.substitutions import LaunchConfiguration


def generate_launch_description():
    launch_file_dir = os.path.join(get_package_share_directory('turtlebot3_gazebo'), 'launch')
    pkg_gazebo_ros = get_package_share_directory('gazebo_ros')

    use_sim_time = LaunchConfiguration('use_sim_time', default='true')
    x_pose = LaunchConfiguration('x_pose', default='0.0')
    y_pose = LaunchConfiguration('y_pose', default='0.0')

    world = os.path.join(
        get_package_share_directory('turtlebot3_gazebo'),
        'worlds',
        'turtlebot3_test_world.world'
    )

    gzserver_cmd = IncludeLaunchDescription(
        PythonLaunchDescriptionSource(
            os.path.join(pkg_gazebo_ros, 'launch', 'gzserver.launch.py')
        ),
        launch_arguments={'world': world}.items()
    )

    gzclient_cmd = IncludeLaunchDescription(
        PythonLaunchDescriptionSource(
            os.path.join(pkg_gazebo_ros, 'launch', 'gzclient.launch.py')
        )
    )

    robot_state_publisher_cmd = IncludeLaunchDescription(
        PythonLaunchDescriptionSource(
            os.path.join(launch_file_dir, 'robot_state_publisher.launch.py')
        ),
        launch_arguments={'use_sim_time': use_sim_time}.items()
    )

    spawn_turtlebot_cmd = IncludeLaunchDescription(
        PythonLaunchDescriptionSource(
            os.path.join(launch_file_dir, 'spawn_turtlebot3.launch.py')
        ),
        launch_arguments={
            'x_pose': x_pose,
            'y_pose': y_pose
        }.items()
    )

    ld = LaunchDescription()

    # Add the commands to the launch description
    ld.add_action(gzserver_cmd)
    ld.add_action(gzclient_cmd)
    ld.add_action(robot_state_publisher_cmd)
    ld.add_action(spawn_turtlebot_cmd)

    return ld
```
**`build your package`**  and then launch your file : 
```bash
ros2 launch turtlebot3_gazebo turtlebot3_test_world.launch.py
```
![turtlebot3_in_test_world](https://github.com/fedikk/ROS2-Nav2-Navigation-2-Stack---with-SLAM-and-Navigation/assets/98516504/b12bd042-5920-4c29-b2be-51e60de14581)

let's make the map of our world : 

```bash
$ ros2 launch turtlebot3_cartographer cartographer.launch.py use_sim_time:=True
```
![map_of_test_world](https://github.com/fedikk/ROS2-Nav2-Navigation-2-Stack---with-SLAM-and-Navigation/assets/98516504/6b29cfde-fdb9-4599-8660-55e9cc7eb44f)

### let's save our map : 
```bash
$ ros2 run nav2_map_server map_saver_cli -f maps/test_world_map

```

### test_world_map.yaml
```yaml
image: test_world_map.yaml.pgm
mode: trinary
resolution: 0.05
origin: [-7.82, -6.71, 0]
negate: 0
occupied_thresh: 0.65
free_thresh: 0.25
```

![test_world_map_pgm](https://github.com/fedikk/ROS2-Nav2-Navigation-2-Stack---with-SLAM-and-Navigation/assets/98516504/ede96ae9-bec0-410e-84e8-9ec32a1dd5f6)

### let's run the nav2 in our world 

```bash
ros2 launch turtlebot3_gazebo turtlebot3_test_world.launch.py
```

```bash
ros2 launch turtlebot3_navigation2 navigation2.launch.py use_sim_time:=True map:=maps/test_world_map.yaml 
```

![map_nav2](https://github.com/fedikk/ROS2-Nav2-Navigation-2-Stack---with-SLAM-and-Navigation/assets/98516504/16d62a5a-c1cc-49ea-a103-d38a6272e1c3)

## Tips: How To Fix and Improve Maps with Gimp

like you can see our map misses some pixels the black areas 

![test_world_map_pgm](https://github.com/fedikk/ROS2-Nav2-Navigation-2-Stack---with-SLAM-and-Navigation/assets/98516504/ede96ae9-bec0-410e-84e8-9ec32a1dd5f6)

we are going to fix this we need to **`Gimp`**

```bash
sudo snap install gimp
```
edit your map then save it in **`maps folder`** the name you  want . 

![edited_map](https://github.com/fedikk/ROS2-Nav2-Navigation-2-Stack---with-SLAM-and-Navigation/assets/98516504/f2b64ff1-85bb-40bb-9755-ec7fbce39000)

then change the name of the new map the same as the old one  and everything should work properly.


# Activity : Custom World Activity - Make a Robot Navigate into a Generated Maze

1. go to [**Maze Generator**](https://www.mazegenerator.net/)
2. **`width = 5`** and **`height = 5`**
3. Generate the map
4. Donwload the map **`PNG`**
   
![challenge_map](https://github.com/fedikk/ROS2-Nav2-Navigation-2-Stack---with-SLAM-and-Navigation/assets/98516504/d4db0839-bafe-4d4b-a60d-40a5b618c00f)

5. **scale:** 1.2m for the door
6. adapt turtlebot3 to this world
7. make the robot appear to one of the doors

___

# Section 7 : Intro to Adapting a Custom Robot for Nav2 (Steps Overview)

## my_robot_base.xacro

```xacro
<?xml version="1.0"?>
<robot xmlns:xacro="http://www.ros.org/wiki/xacro" >
    
    <!--*********************************************************************************************-->
    <!--******************************************Variables******************************************--> 
    <!--*********************************************************************************************-->
    <xacro:property name="base_length" value="0.55"/>
    <xacro:property name="base_width" value="0.34"/>
    <xacro:property name="base_height" value="0.21"/>
    <xacro:property name="wheel_radius" value="0.06"/>
    <xacro:property name="wheel_length" value="0.06"/>
    <xacro:property name="caster_wheel_radius" value="${wheel_radius / 2.0}"/>

    <!--*********************************************************************************************-->
    <!--******************************************MACROS******************************************--> 
    <!--*********************************************************************************************-->
    <xacro:macro name="wheel_link" params="prefix"> 
        <link name="${prefix}_wheel_link">
            
            <visual>
                <geometry>
                    <cylinder radius="${ wheel_radius}" length="${ wheel_length}" />   
                    <!--in the size we put length width hight all in meter-->
                </geometry>
                <origin xyz="0 0 0" rpy="${ pi / 2} 0 0"/> <!--rpy = roll pitch yaw-->
                <material name="red"/>
            </visual>

            <collision>
                <geometry>
                    <cylinder radius="${ wheel_radius}" length="${ wheel_length}" />   
                </geometry>
                <origin xyz="0 0 0" rpy="${ pi / 2} 0 0"/> 
            </collision>

            <xacro:cylinder_inertia m="2.0" r="${2*wheel_radius}" h="${2*wheel_length}" xyz="0 0 0" rpy="${ pi / 2} 0 0"/>

        </link>
    </xacro:macro>


    
    <!--*********************************************************************************************-->
    <!--*********************************************Links*******************************************--> 
    <!--*********************************************************************************************-->

    <link name="base_footprint"/>
    
    <link name="base_link">
        
        <visual>
            <geometry>
                <box size="${base_length} ${ base_width} ${ base_height}" />   
            </geometry>
            <origin xyz="0 0 ${ base_height / 2.0 }" rpy="0 0 0"/> 
            <material name="black"/>
        </visual>

        <collision>
            <geometry>
                <box size="${base_length} ${ base_width} ${ base_height}" />   
            </geometry>
            <origin xyz="0 0 ${ base_height / 2.0 }" rpy="0 0 0"/> 
        </collision>
        
        <xacro:box_inertia m="5.0" l="${2*base_length}" w="${ 2*base_width}" h="${2* base_height}" 
                           xyz="0 0 ${ base_height / 2.0 }" rpy="0 0 0"/>

    </link>


    <xacro:wheel_link prefix="right"/>
    <xacro:wheel_link prefix="left"/>


    <link name="caster_wheel_link">
        
        <visual>           
            <geometry>
                <sphere radius="${ caster_wheel_radius}"  />   
            </geometry>
            <origin xyz="0 0 0" rpy="0 0 0"/> 
            <material name="white"/> 
        </visual>

        <collision>
            <geometry>
                <sphere radius="${ caster_wheel_radius}"  />   
            </geometry>
            <origin xyz="0 0 0" rpy="0 0 0"/> 
        </collision>

        <xacro:sphere_inertia m="0.3" r="${ 2*caster_wheel_radius}" xyz="0 0 0" rpy="0 0 0"/>
    
    </link>

    <link name="base_scan">
        
        <visual>
            <geometry>
                <cylinder radius="0.1" length="0.06"/>
            </geometry>
            <origin xyz="0 0 0" rpy="0 0 0" />
            <material name="blue" />
        </visual>

        <collision>
            <geometry>
                <cylinder radius="0.1" length="0.06"/>
            </geometry>
            <origin xyz="0 0 0" rpy="0 0 0" />
        </collision>

        <xacro:sphere_inertia m="0.3" r="${ 2*0.1}" xyz="0 0 0" rpy="0 0 0"/>
    
    </link>

    <!--**********************************************************************************************-->
    <!--*********************************************Joints*******************************************--> 
    <!--**********************************************************************************************-->
    
    <joint name="base_joint" type="fixed" > 
        <parent link="base_footprint"/>
        <child link="base_link"/>
        <origin xyz="0 0 ${ wheel_radius}" rpy="0 0 0"/>
    </joint>
    
    
    <joint name="base_right_wheel_joint" type="continuous" > 
        <parent link="base_link" />
        <child link="right_wheel_link"/>
        <origin xyz="${- base_length /  4.0} ${-(base_width / 2.0) - (wheel_length / 2.0)} 0" rpy="0 0 0"/>g
        <axis xyz="0 1 0"></axis>
    </joint>
    
    <joint name="base_left_wheel_joint" type="continuous" > 
        <parent link="base_link" />
        <child link="left_wheel_link"/>
        <origin xyz="${- base_length /  4.0} ${+(base_width / 2.0) + (wheel_length / 2.0)} 0" rpy="0 0 0"/>
        <axis xyz="0 1 0"></axis>
    </joint>

    <joint name="base_caster_wheel_joint" type="fixed" > 
        <parent link="base_link" />
        <child link="caster_wheel_link"/>
        <origin xyz="${ base_length /  3.0} 0 ${-caster_wheel_radius}" rpy="0 0 0"/>
    </joint>

    <joint name="base_scan_joint" type="fixed">
        <parent link="base_link"/>
        <child link="base_scan"/>
        <origin xyz="0 0 ${base_height+0.03}" rpy="0 0 0"/>
    </joint>

</robot>
```

![Add_lidar](https://github.com/fedikk/ROS2-Nav2-Navigation-2-Stack---with-SLAM-and-Navigation/assets/98516504/0c570c42-3f42-4af1-842d-2458d5f0565b)


# Run Navigation With Your Custom Robot (using slam_toolbox)

## let's save the map 
first of all we need to install SlamToolbox
```bash
sudo apt install ros-humble-slam-toolbox
```
let's work with turtlebot3 then generate : 
1.launch gazebo 
```bash
ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py
```
2. launch the nav2 bringup 
```bash
ros2 launch nav2_bringup navigation_launch.py use_sim_time:=True
```
3. launch slam toolbox asynchrone  
```bash
ros2 launch slam_toolbox online_async_launch.py
```
4. ros2 run rviz2 rviz2 
```bash
ros2 run rviz2 rviz2 
```
dont forget to add **`robot_model`** & **`TF`** & **`Laser_scan`** & **`camera`**
5. run  turtlebot3 teleop 
```bash
ros2 run turtlebot3_teleop teleop_keyboard 
```
6. Move the robot and create the map
7. Save the map  
```bash
ros2 run nav2_map_server map_saver_cli -f maps/my_custom_map
```

## Slam tool box vs cartographer map 

![slam_vs_carto](https://github.com/fedikk/ROS2-Nav2-Navigation-2-Stack---with-SLAM-and-Navigation/assets/98516504/a51e2a0f-62df-4c84-bbeb-57cb87b029e1)

## let's make the robot navigate in this map 
### 1 
```bash
ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py
```
### 2
```bash
ros2 launch nav2_bringup bringup_launch.py use_sim_time:=True map:=maps/slamtoolbox_map.yaml
```
### 3
```bash
ros2 run rviz2 rviz2 
```
dont forget to add **`map`** and inside it  go to : 
**`topic`** >> **`Durabilitie`** >> **`Transiant local`** 

also give a **`2D Pose Estimate`** and u can navigate as u like  : 

![nav2_slam_toolbox](https://github.com/fedikk/ROS2-Nav2-Navigation-2-Stack---with-SLAM-and-Navigation/assets/98516504/dc27fc81-eb01-4d09-aefc-0cbe38265298)
