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
we are going to use `ROS2 Humble` with `Ubuntu 22.04`

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
