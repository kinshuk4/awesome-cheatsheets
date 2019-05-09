---
title: ROS
category: ros
keywords:
  - carnd
  - ros
---

- [1. Installing and Configuring](#1-installing-and-configuring)
  - [1.1 Install ROS](#11-install-ros)
  - [1.2 Managing Environment](#12-managing-environment)
  - [1.3 Create a ROS Workspace](#13-create-a-ros-workspace)
- [2. Navigating the ROS Filesystem](#2-navigating-the-ros-filesystem)
- [3. Creating a ROS Package](#3-creating-a-ros-package)
  - [3.1 Create](#31-create)
  - [3.2 Build](#32-build)
  - [3.3 Package Dependencies](#33-package-dependencies)
- [4. Building a ROS Package](#4-building-a-ros-package)
- [5. ROS Nodes](#5-ros-nodes)
- [6. ROS Topics](#6-ros-topics)
- [7. ROS Services and Parameters](#7-ros-services-and-parameters)
- [8. Recording and playing back data](#8-recording-and-playing-back-data)
  - [8.1 Creating a bag file](#81-creating-a-bag-file)
  - [8.2 Recording all published topics](#82-recording-all-published-topics)
  - [8.3 Examining and playing the bag file](#83-examining-and-playing-the-bag-file)
- [Start-up and Process Launch Tools](#start-up-and-process-launch-tools)


## 1. Installing and Configuring

### 1.1 Install ROS

[ROS installation instructions](http://wiki.ros.org/kinetic/Installation/Ubuntu)

It's about the Ubuntu install of ROS Kinetic

### 1.2 Managing Environment

```bash
$ printenv | grep ROS
```

### 1.3 Create a ROS Workspace

```bash
$ mkdir -p ~/catkin_ws/src
$ cd ~/catkin_ws/
$ catkin_make
```

Then, source the shell file. (devel/setup.bash or devel/setup.zsh).

Or, you can add it to your ~/.zshrc (~/.bashrc) with source.

## 2. Navigating the ROS Filesystem

|Command|Description|
|:-----:|:---------:|
|rospack find [package_name]|find ros packet|
|roscd [locationname[/subdir]]|change dir|
|roscd log|change dir to log dir(~/.ros/log)|
|rosls [locationname[/subdir]]|list the file within dir|

## 3. Creating a ROS Package

### 3.1 Create

Using `catkin_create_pkg` command.

Use the `catkin_create_pkg` script to create a new package called 'beginner_tutorials' which depends on **std_msgs**, **roscpp**, and **rospy**.

Usage: `catkin_create_pkg <package_name> [depend1] [depend2] [depend3]`

```bash
# You should have created this in the Creating a Workspace Tutorial
$ cd /to/your/catkin_w/src/dir
$ catkin_create_pkg beginner_tutorial std_msgs rospy roscpp
```

### 3.2 Build

```bash
$ cd /to/your/catkin_w/src/dir
$ catkin_make
```

Add workspace to ROS environment we can source the generated setup file in devel dir.

### 3.3 Package Dependencies

First-order dependencies: `rospack depends1 beginner_tutorials`

Indirect dependencies: `rospack depends beginner_tutorials`

## 4. Building a ROS Package

Using `catkin_make`.

You can imagine that catkin_make combines the calls to cmake and make in the standard CMake workflow.

```bash
# In a catkin workspace
$ catkin_make [make_targets] [-DCMAKE_VARIABLES=...]
```

## 5. ROS Nodes

`roscore` is the first thing you shour run when using ROS.

```bash
$ roscore
```

Command CheatSheet:

```bash
# Lists active nodes
$ rosnode list

# Returns Information about a specific node
$ rosnode info /rosout

# Directly run a node within a packet
$ rosrun [package_name] [node_name]
```

## 6. ROS Topics

`rqt_graph` creates a dynamic graph of what's going on in the system.

`rqt_plot` displays a scrolling time plot of the data published on topics.

```bash
$ rosrun rqt_graph rqt_graph
$ rqt_graph

$ rosrun rqt_plot rqt_plot
$ rqt_plot
```

Command CheatSheet:

```bash
# Shows the data published on a topic.
$ rostopic echo [topic]

# Returns a list of all current topics.
$ rostopic list

# Returns the message type of any topic.
$ rostopic type [topic]

# Publishes data.
$ rostopic pub [topic] [msg_type] [args]

# Reports the rate at which data is published.
$ rostopic hz [topic]
```

## 7. ROS Services and Parameters

```bash
$ rosservice list
$ rosservice type [service]
$ rosservice call [service] [args]
```

```bash
$ rosparam list
$ rosparam set [param_name]
$ rosparam get [param_name]
$ rosparam dump [file_name] [namespace]
$ rosparam load [file_name] [namespace]
```

## 8. Recording and playing back data

### 8.1 Creating a bag file

Run the following nodes.

`roscore`

`rosrun turtlesim turtlesim_node`

`rosrun turtlesim turtle_teleop_key`

### 8.2 Recording all published topics

```bash
$ mkdir ~/bagfiles
$ cd ~/bagfiles
$ rosbag recoard -a
```

### 8.3 Examining and playing the bag file

```bash
$ rosbag info <bagfile>
$ rosbag play <bagfile>
```



Moving to workspace.

## Workspace

### Sourcing ROS

```
# Make sure you've added the system-wide ROS setup script to .bashrc file
# If you haven't done that, do it now, or source the file by hand

souce /opt/ros/kinetic/setup.bash
```

### Creating a new workspace

#### STEP 1: Create a new workspace folder

```
# Create a new workspace folder, e.g. telebot_ws
mkdir -p /telebot_ws/src
```

#### STEP 2: Initialize the workspace

```
# Enter the src directory
cd telebot_ws/src

# Use catkin to initialize workspace
catkin_init_workspace
```

#### STEP 3: Generate the catkin workspace files

```
# Generate the catkin workspace files
cd ../telebot_ws
catkin_make
```

## Packages

```
# Go into the workspace src directory
cd telebot_ws/src
# Create package e.g. my_awesome_code
catkin_create_pkg my_awesome_code rospy
```

## Topics

### Help on rostopic parameters

```
rostopic -h
```

### List all topics

```
rostopic list
```

### Show messages published to a topic

```
rostopic echo [topic-name] [param] [param-value]

# For example, for a node called 'counter'. Show only 5 messages
rostopic echo counter -n 5
```

### Check publish rate

```
rostopic hz [topic-name]

# For example, for a node named 'counter'
rostopic hz counter
```

### Display information about an advertised topic

```
rostopic info [topic-name]

# For example, for a node called 'counter'
rostopic info counter
```

### Find topics of a given message type

```
rostopic find [message-type]

# For example, for a topic with 'std_msgs/Int32' type
rostopic find std_msgs/Int32
```

### Publis a message to a topic

```
rostopic pub [topic_name] [message_type] [message_data]

# For example, for a topic named 'counter', publish a message with a type of 'std_msg/Int32' and a value 1000000
rostopic pub counter std_msgs/Int32 1000000
```

<details class="details-reset details-overlay details-overlay-dark" style="box-sizing: border-box; display: block;"><summary data-hotkey="l" aria-label="Jump to line" aria-haspopup="dialog" style="box-sizing: border-box; display: list-item; cursor: pointer; list-style: none;"></summary></details>

 




UsageFilesystem Management Tools

---------------------- -------------------------------------------------------------------------------------------------------------------------------------------
  `rospack`              A tool for inspecting [packages](http://wiki.ros.org/Packages).
  `rospack profile`      Fixes path and pluginlib problems.
  `roscd`                Change directory to a package.
  `rospack`/`rosstack`   A tool inspecting [packages](http://wiki.ros.org/Packages).
  `rospack profile`      Fixes path and pluginlib problems.
  `roscd`                Change directory to a package or stack.
  `rospd`/`rosd`         [Pushd](http://ftp.gnu.org/old-gnu/Manuals/bash-2.05a/html_node/bashref_73.html) equivalent for [ROS](http://wiki.ros.org/rosbash#roscd).
  `rosls`                Lists package or stack information.
  `rosed`                Open requested ROS file in a text editor.
  `roscp`                Copy a file from one place to another.
  `rosdep`               Installs package system dependencies.
  `roswtf`               Displays a errors and warnings about a running ROS system or launch file.
  `catkin_create_pkg`    Creates a new ROS stack.
  `wstool`               Manage many repos in workspace.
  `catkin_make`          Builds a ROS catkin workspace.
  `roscreate-pkg`        Creates a new ROS package.
  `roscreate-stack`      Creates a new ROS stack.
  `rosmake`              Builds a ROS package.
  `rqt_dep`              Displays package structure and dependencies.
---------------------- -------------------------------------------------------------------------------------------------------------------------------------------

Usage:
`$ rospack find [package]`
`$ roscd [package[/subdir]]`
`$ rospd [package[/subdir] | +N | -N]`
`$ rosd`
`$ rosls [package[/subdir]]`
`$ rosed [package] [file]`
`$ roscp [package] [file] [destination]`
`$ rosdep install [package]`
`$ roswtf or roswtf [file]`
`$ catkin_create_pkg [package_name] [depend1]..[dependN]`
`$ wstool [init | set | update]`
`$ catkin_make`
`$ roscreate-pkg [package_name]`
`$ rosmake [package]`
`$ rqt_dep [options]`

## Start-up and Process Launch Tools
[roscore](http://wiki.ros.org/roscore) The basis
[nodes](http://wiki.ros.org/Nodes) and programs for ROS-based systems. A
roscore must be running for ROS nodes to communicate.
Usage:
`$ roscore`

[rosrun](http://wiki.ros.org/rosrun) Runs a ROS
package's executable with minimal typing.
Usage:
`$ rosrun package_name executable_name` Exāmple (runs
[turtlesim](http://wiki.ros.org/turtlesim)):
`$ rosrun turtlesim turtlesim_node`

[roslaunch](http://wiki.ros.org/roslaunch) Starts a
roscore (if needed), [local
nodes](http://wiki.ros.org/roslaunch/XML#Minimal_Example), [remote
nodes](http://wiki.ros.org/roslaunch/XML/machine) via SSH, and sets
parameter server
[parameters](http://wiki.ros.org/roslaunch/XML#Setting_parameters).
Example:
Launch a file in a package:
`$ roslaunch package_name file_name.launch`
Launch on a different port:
`$ roslaunch -p 1234 package_name file_name.launch`
Launch on the local nodes:
`$ roslaunch –local package_name file_name.launch`

UsageIntrospection and Command Tools

[rosnode](http://wiki.ros.org/rosnode) Displays
debugging information about ROS nodes, including publications,
subscriptions and connections.
Commands:

------------------- ----------------------------------
  `rosnode ping`      Test connectivity to node.
  `rosnode list`      List active nodes.
  `rosnode info`      Print information about a node.
  `rosnode machine`   List nodes running on a machine.
  `rosnode kill`      Kill a running node.
------------------- ----------------------------------

Example:
Kill all nodes:
`$ rosnode kill -a`
List nodes on a machine:
`$ rosnode machine aqy.local`
Ping all nodes:
`$ rosnode ping –all`

[rostopic](http://wiki.ros.org/rostopic) A tool for
displaying information about ROS [topics](http://wiki.ros.org/Topics),
including publishers, subscribers, publishing rate, and messages.
Commands:

----------------- ------------------------------------------
  `rostopic bw`     Display bandwidth used by topic.
  `rostopic echo`   Print messages to screen.
  `rostopic find`   Find topics by type.
  `rostopic hz`     Display publishing rate of topic.
  `rostopic info`   Print information about an active topic.
  `rostopic list`   List all published topics.
  `rostopic pub`    Publish data to topic.
  `rostopic type`   Print topic type.
----------------- ------------------------------------------

Example:
Publish hello at 10 Hz:
`$ rostopic pub -r 10 /topic_name std_msgs/String hello`
Clear the screen after each message is published:
`$ rostopic echo -c /topic_name`
Display messages that match a given Python expression:
`$ rostopic echo –filter "m.data==’foo’" /topic_name`
Pipe the output of rostopic to rosmsg to view the msg type:
`$ rostopic type /topic_name | rosmsg show`

[rosservice](http://wiki.ros.org/rosservice) A tool
for listing and querying ROS services.
Commands:

------------------- ------------------------------------------
  `rosservice list`   Print information about active services.
  `rosservice node`   Print name of node providing a service.
  `rosservice call`   Call the service with the given args.
  `rosservice args`   List the arguments of a service.
  `rosservice type`   Print the service type.
  `rosservice uri`    Print the service ROSRPC uri.
  `rosservice find`   Find services by service type.
------------------- ------------------------------------------

Example:
Call a service from the command-line:
`$ rosservice call /add_two_ints 1 2`
Pipe the output of rosservice to rossrv to view the srv type:
`$ rosservice type add_two_ints | rossrv show`
Display all services of a particular type:
`$ rosservice find rospy_tutorials/AddTwoInts`

[rosparam](http://wiki.ros.org/rosparam) A tool for
getting and setting ROS
[parameters](http://wiki.ros.org/Parameter Server) on the parameter
server using YAML-encoded files.
Commands:

------------------- ------------------------------
  `rosparam set`      Set a parameter.
  `rosparam get`      Get a parameter.
  `rosparam load`     Load parameters from a file.
  `rosparam dump`     Dump parameters to a file.
  `rosparam delete`   Delete a parameter.
  `rosparam list`     List parameter names.
------------------- ------------------------------

Example:
List all the parameters in a namespace:
`$ rosparam list /namespace`
Setting a list with one as a string, integer, and float:
`$ rosparam set /foo "[’1’, 1, 1.0]"`
Dump only the parameters in a specific namespace to file:
`$ rosparam dump dump.yaml /namespace`

[rosmsg/rossrv](http://wiki.ros.org/rosmsg)
Displays Message/Service (msg/srv) data structure definitions.
Commands:

------------------- -------------------------------------------
  `rosmsg show`       Display the fields in the msg/srv.
  `rosmsg list`       Display names of all msg/srv.
  `rosmsg md5`        Display the msg/srv md5 sum.
  `rosmsg package`    List all the msg/srv in a package.
  `rosmsg packages`   List all packages containing the msg/srv.
------------------- -------------------------------------------

Example:
Display the Pose msg:
`$ rosmsg show Pose`
List the messages in the nav\_msgs package:
`$ rosmsg package nav_msgs`
List the packages using sensor\_msgs/CameraInfo:
`$ rosmsg packages sensor_msgs/CameraInfo`

UsageLogging Tools
[rosbag](http://wiki.ros.org/rosbag) A set of tools for recording and
playing back of ROS topics.
Commands:

--------------------- ------------------------------------------
  `rosbag record`       Record a bag file with specified topics.
  `rosbag play`         Play content of one or more bag files.
  `rosbag compress`     Compress one or more bag files.
  `rosbag decompress`   Decompress one or more bag files.
  `rosbag filter`       Filter the contents of the bag.
--------------------- ------------------------------------------

Example:
Record select topics:
`$ rosbag record topic1 topic2`
Replay all messages without waiting:
`$ rosbag play -a demo_log.bag`
Replay several bag files at once:
`$ rosbag play demo1.bag demo2.bag`


[tf\_echo](http://wiki.ros.org/tf2/Tutorials/Introduction to tf2#Using_tf_echo)
A tool that prints the information about a particular transformation
between a source\_frame and a target\_frame.
Usage:
`$ rosrun tf tf_echo <source_frame> <target_frame>`
Example:
To echo the transform between /map and /odom:
`$ rosrun tf tf_echo /map /odom`

UsageLogging Tools

[rqt\_console](http://wiki.ros.org/rqt_console) A
tool to display and filtering messages published on rosout.
![image](rqt_console.png){width="0.6\\columnwidth"} Usage:
`$ rqt_console`

[rqt\_bag](http://wiki.ros.org/rqt_bag) A tool for
visualizing, inspecting, and replaying bag files.
![image](rqt_bag.png){width="0.6\\columnwidth"} Usage, viewing:
`$ rqt_bag bag_file.bag`
Usage, bagging:
`$ rqt_bag *press the big red record button.*`


[rqt\_logger\_level](http://wiki.ros.org/rqt_logger_level) Change the
logger level of ROS nodes. This will increase or decrease the
information they log to the screen and rqt\_console.
Usage:
`viewing $ rqt_logger_level`

UsageIntrospection & Command Tools

[rqt\_topic](http://wiki.ros.org/rqt_topic) A tool
for viewing published topics in real time.
Usage:
`$ rqt`
`Plugin Menu->Topic->Topic Monitor`

[rqt\_msg](http://wiki.ros.org/rqt_msg),
[rqt\_srv](http://wiki.ros.org/rqt_srv), and
[rqt\_action](http://wiki.ros.org/rqt_action) A tool for viewing
available msgs, srvs, and actions.
Usage:
`$ rqt`
`Plugin Menu->Topic->Message Type Browser`
`Plugin Menu->Service->Service Type Browser`
`Plugin Menu->Action->Action Type Browser`

[rqt\_top](http://wiki.ros.org/rqt_top) A tool for
ROS specific process monitoring.
Usage:
`$ rqt`
`Plugin Menu->Introspection->Process Monitor`


[rqt\_publisher](http://wiki.ros.org/rqt_publisher), and
[rqt\_service\_caller](http://wiki.ros.org/rqt_service_caller) Tools for
publishing messages and calling services.
Usage:
`$ rqt`
`Plugin Menu->Topic->Message Publisher`
`Plugin Menu->Service->Service Caller`


[rqt\_reconfigure](http://wiki.ros.org/rqt_reconfigure) A tool for
dynamically reconfiguring ROS parameters.
Usage:
`$ rqt`
`Plugin Menu->Configuration->Dynamic Reconfigure`

[rqt\_graph](http://wiki.ros.org/rqt_graph), and
[rqt\_dep](http://wiki.ros.org/rqt_dep) Tools for displaying graphs of
running ROS nodes with connecting topics and package dependancies
respectively.
![image](rqt_graph.png){width="0.45\\columnwidth"}
![image](rqt_dep.png){width="0.53\\columnwidth"} Usage:
`$ rqt_graph`
`$ rqt_dep`

UsageDevelopment Environments

[rqt\_shell](http://wiki.ros.org/rqt_shell), and
[rqt\_py\_console](http://wiki.ros.org/rqt_py_console) Two tools for
accessing an xterm shell and python console respectively.
Usage:
`$ rqt`
`Plugin Menu->Miscellaneous Tools->Shell`
`Plugin Menu->Miscellaneous Tools->Python Console`

UsageData Visualization Tools


[view\_frames](http://wiki.ros.org/tf2/Tutorials/Introduction to tf2#Using_view_frames)
A tool for visualizing the full tree of coordinate transforms.
Usage:
`$ rosrun tf2_tools view_frames.py`
`$ evince frames.pdf`

[rqt\_plot](http://wiki.ros.org/rqt_plot) A tool
for plotting data from ROS topic fields.
![image](rqt_plot.png){width="0.6\\columnwidth"} Example:
To graph the data in different plots:
`$ rqt_plot /topic1/field1 /topic2/field2`
To graph the data all on the same plot:
`$ rqt_plot /topic1/field1,/topic2/field2`
To graph multiple fields of a message:
`$ rqt_plot /topic1/field1:field2:field3`


[rqt\_image\_view](http://wiki.ros.org/rqt_image_view) A tool to display
image topics.
![image](rqt_image_view.png){width="0.5\\columnwidth"} Usage:
`$ rqt_image_view`

UsageROS Indigo Catkin Workspaces

[Create a catkin
workspace](http://wiki.ros.org/catkin/Tutorials/create_a_workspace)
Setup and use a new catkin workspace from scratch. Ex̄āmple:
`$ source /opt/ros/indigo/setup.bash`
`$ mkdir -p ~/catkin_ws/src`
`$ cd ~/catkin_ws/src`
`$ catkin_init_workspace`

[Checkout an existing ROS
package](http://wiki.ros.org/catkin/Tutorials/CreatingPackage) Get a
local copy of the code for an existing package and keep it up to date
using
[wstool](http://wiki.ros.org/catkin/Tutorials/workspace_overlaying).
Example:
`$ cd ~/catkin_ws/src`
`$ wstool init`
`$ wstool set tut –git git://github.com/ros/ros_tutorials.git`
`$ wstool update`

[Create a new catkin ROS
package](http://wiki.ros.org/catkin/Tutorials/CreatingPackage)

Create a new ROS catkin package in an existing workspace with [catkin
create package](http://wiki.ros.org/catkin/Tutorials/CreatingPackage).
Us̄age:
`$ catkin_create_pkg <package_name> [depend1] [depend2]`
Ex̄āmple:
`$ cd ~/catkin_ws/src`
`$ catkin_create_pkg tutorials std_msgs rospy roscpp`

[Build all packages in a
workspace](http://wiki.ros.org/catkin/Tutorials/using_a_workspace) Use
[catkin\_make](http://wiki.ros.org/catkin/Tutorials/using_a_workspace)
to build all the packages in the workspace and then source the
setup.bash to add the workspace to the
[ROS\_PACKAGE\_PATH](http://wiki.ros.org/ROS/EnvironmentVariables#ROS_PACKAGE_PATH).
Example:
`$ cd ~/catkin_ws`
`$ ~/catkin_make`
`$ source devel/setup.bash`


[CMakeLists.txt](http://wiki.ros.org/catkin/CMakeLists.txt)

Yōur CMakeLists.txt̄ file MUST follow this format otherwise\
your packages will not build correctly.
`cmake_minimum_required()` Specify the name of the package\
`project()` Project name which can refer as \${PROJECT\_NAME}\
`find_package()` Find other packages needed for build\
`catkin_package()` Specify package build info export\

**Build Executables and Libraries:**\
Use CMake function to build executable and library targets. These macro
should call after `catkin_package()` to use `catkin_` variables.

  `include_directories(include ${catkin_INCLUDE_DIRS})`
  `add_executable(hoge src/hoge.cpp)`
  `add_library(fuga src/fuga.cpp)`
  `target_link_libraries(hoge fuga ${catkin_LIBRARIES})`

**Message generation:**\
There are `add_{message,service,action}_files()` macros to handle
messages,services and actions respectively. They must call before
`catkin_package()`.

  `find_package(catkin COMPONENTS message_generation std_msgs)`
  `add_message_files(FILES Message1.msg)`
  `generate_messages(DEPENDENCIES std_msgs)`
  `catkin_package(CATKIN_DEPENDS message_runtime)`

If your package builds messages as well as executables that use them,
you need to create an explicit dependency.

  `add_dependencies(hoge ${PROJECT_NAME}_generate_messages_cpp)`



## Available Resources

- [notes-programming-with-ros](https://github.com/ATR-Lab/all-things-ros/blob/master/notes-programming-with-ros.md)
- [cheatsheet-ros-basic](https://github.com/ATR-Lab/all-things-ros/blob/master/cheatsheet-ros-basic.md)

## Guides

- [Catkin Tutorials](http://wiki.ros.org/catkin/Tutorials)
- [ROS Start Guide](http://wiki.ros.org/ROS/StartGuide)
- [ROS Beginner Tutorials](http://wiki.ros.org/ROS/Tutorials)
- [RViz User's Guide](http://docs.ros.org/kinetic/api/rviz/html/user_guide/)
- [RViz Troubleshooting Guide](http://ros.org/wiki/rviz/Troubleshooting)
- [tf Overiew](http://wiki.ros.org/tf)
- [tf Tutorials](http://wiki.ros.org/tf/Tutorials)
- [actions Overview](http://wiki.ros.org/actionlib)
- [actionlib Tutorials](http://wiki.ros.org/actionlib/Tutorials)
- [Services and Parameters Tutorials](http://wiki.ros.org/ROS/Tutorials/UnderstandingServicesParams#Using_rosparam)

https://github.com/ATR-Lab/all-things-ros/blob/master/cheatsheet-ros-basic.md

https://github.com/cyanial/ros-cheatsheet
https://github.com/kuroemon2509/ROS-cheatsheet
------------------------------------------------------------------------

Copyright © 2015 Open Source Robotics Foundation

Copyright © 2010 Willow Garage
