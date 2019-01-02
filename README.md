
# ROS and the Fraunhofer Institut Volksbot

## 1. ROS Kinetic Installation: 


### I. Why ROS Kinetic
In order to guarantee full compatibility between ROS and Volksbot, a couple necessary requirements have to be examined and compared beforehand. 
They matter on making the right decision on what ROS distribution to pick. The following ***4 criteria*** will help how to choose the right distribution:

#### A. volksbot_driver
A group of students from the Osnabrueck University has already build a complete open-source ROS module for the Volksbot and published it on Github.
Their latest tested version of the volksbot_driver module can be applied up to release ROS Lunar.

* [volksbot_driver Repository](https://github.com/uos/volksbot_driver)
* [volksbot_driver Release Compatibility](http://wiki.ros.org/uos-ros-pkg#ROS_Release_Compatibility)

#### B. uos_tools
The uos_tools module serves to control the the volksbot_driver module and is only supported up to release ROS Kinetic.
* [uos_tools Repository](https://github.com/uos/uos_tools)

#### C. The Sick Laser
The Sick Laser TiM551 module can also be used up to release ROS Lunar and beyond.
* [sick_scan](http://wiki.ros.org/sick_scan)
* [sick_tim](http://wiki.ros.org/sick_tim)

#### D. Microsoft Kinect
The ROS driver for the Microsoft Kinect is only supported up to ROS Kinetic.
* [freenect_stack](http://wiki.ros.org/freenect_stack)

#### Result
By looking at the upper most supported compatible releases of points A. - D. and also considering the [End Of Life span of each distribution](http://wiki.ros.org/Distributions), one can identify ROS Kinetic as the lowest common denominator and most suitable ROS option for our cause.

### II. How to install ROS Kinetic

As of December 2018, ROS Kinetic is the recommended distribution for Volksbot, so now we will have a look on how to install it.

#### A. The right Unix distribution

ROS Kinetic is only fully supported on:

* [Ubuntu Wily (15.10)](http://old-releases.ubuntu.com/releases/15.10/)
* [Ubuntu Xenial (16.04)](http://releases.ubuntu.com/16.04/)

It probably does not matter which one you pick but the latter is recommended. Look [here](http://www.ros.org/reps/rep-0003.html#kinetic-kame-may-2016-may-2021) for reference.
Install one of the two OS on your computer before you proceed.

#### B. Install ROS Kinetic

Now that the operating system is running you pretty much just have to follow the [ROS installation guide](http://wiki.ros.org/kinetic/Installation/Ubuntu).
It is highly recommended to at least go through the [beginner level tutorial series](http://wiki.ros.org/ROS/Tutorials#Beginner_Level) after installation to understand the core principles of ROS. 

#### C. Installing libepos2:

While we are installing all kinds of stuff, let us install die ***epos2 drivers*** for on our Linux machine.

* [libepos2 for volksbot_driver](http://wiki.ros.org/volksbot_driver#Installation)

#### D. Creating your aliases

Since ROS requires a lot of constantly opening new terminals and initializing the ROS environments with ***long commands over and over again***, it would more convenient if there was some sort of ***list of abbreviations*** that can be used to execute longer command prompts. *This is only recommended and not mandatory*.

If not already existing, open a terminal and create a *.bash-profile*. This file will be executed every time you open a new terminal window:
```bash
[vim or nano] ~/.bash-profile
```
In there you write the following, which we you will get to know and need later on in this Tutorial. After that save and close the file and reopen a terminal:
```bash
alias cdcat='cd ~/catkin_ws'
alias gokinetic='. /opt/ros/kinetic/setup.bash'
alias gocat='. ~/catkin_ws/devel/setup.bash'

alias goros='cdcat && gokinetic && gocat'

alias ccp='catkin_create_pkg'
alias cm='catkin_make'

#optionally for editing and activating .bash-profile
alias ealias='vim ~/.bash-profile'
alias aalias='. ~/.bash-profile'
```

***Congratulations! Now you are all set up and ready to work with ROS.***

## 2. Workflow with ROS

A new project in ROS is build in a ***catkin package***. A catkin package is part of a ***catkin workspace***. [Catkin](http://wiki.ros.org/catkin/workspaces#Catkin_Workspaces) provides the concept of workspaces, where you can build multiple *independent packages* together all at once.

### I. Initializing ROS
ROS is primarily used via the terminal. Therefore it is recommended to ***initialize/activate ROS every you open a new terminal window***.
This is done by the following command, which also can be written into the *~/.bash-profile* for automated activation if desired:

```bash
source /opt/ros/kinetic/setup.bash
```

*Now this terminal window knows ROS!*


### II. The Catkin Workspace

In a catkin workspace you can independently build all your ROS catkin packages in one place.

#### A. Create a catkin workspace
In a desired directory create a Folder named `catkin_ws` for instance.
```bash
mkdir -p ~/catkin_ws/src && cd ~/catkin_ws/
# now you build it
catkin_make
```

This is basically done every time before you want to use a package and respectively done after you modified a package. [See here for reference.](http://wiki.ros.org/ROS/Tutorials/InstallingandConfiguringROSEnvironment#Create_a_ROS_Workspace)


#### B. Initialize workspace
After a successful build you can initialize/activate this workspace by sourcing the setup.bash:
```bash
source ~/catkin_ws/devel/setup.bash
```
This should also be done every time you open a new terminal window!

#### C. Create your first package

The following is the standard procedure to [create a new package from scratch:](http://wiki.ros.org/ROS/Tutorials/CreatingPackage)
```bash
# catkin_create_pkg [package name] [dependency1] [dependency n]
# for instance
catkin_create_pkg beginner_tutorials std_msgs rospy roscpp
# after that
cd ~/catkin_ws && catkin_make
```

## 3. ROS and the Volksbot

The last section [2.II.C.](#c.-create-your-first-package) was for learning purposes only and is not relevant for our Volksbot project, since there are already the volksbot_driver package for the bot itself and the uos_tool package to control the bot. Now we will learn how to use the two modules together.

### I. Cloning the projects

#### A. volksbot_driver
In your catkin workspace you want to go to the `src` directory and clone the package in it. Since were cloning yet another project we postpone the `catkin_make` after doing so:
```bash
cd ~/catkin_ws/src
git clone https://github.com/uos/volksbot_driver.git
```
#### B. uos_tools
Similarly to the [volksbot_driver](#a.-volksbot_driver-1) package, we are cloning the uos_tools into our `src` folder and build our catkin workspace afterwards:
```bash
cd ~/catkin_ws/src
git clone https://github.com/uos/uos_tools.git
# build the workspace
cd ~/catkin_ws && catkin_make
```
After the successful build and of course installing the ***libepos2*** from section [1.II.C.](#c.-installing-libepos2) you can now plug in your robot cables and start using ROS with your robot as described in the next section.

### II. Lets move

Provided that you followed the complete guide up to this point and also created your `~/.bash-profile` with the mentioned aliases from section [1.II.D.](#d.-creating-your-aliases) where are now starting to move the bot.

#### A. Launch volksbot_driver

Open a new terminal and initialize ROS and the catkin workspace with:
```bash
goros
```
Then launch the volksbot_driver by doing one of the following methods:
```bash
roslaunch volksbot_driver volksbot.launch
# or
rosrun volksbot_driver volksbot
```
Now this terminal is occupied with the node that runs the robot.

#### B. Use uos_tools
Next we open a new terminal and activate ROS again:
```bash
goros
```
This time we launch the `uos_diffdrive_teleop` to control the bot:
```bash
roslaunch uos_diffdrive_teleop key.launch
# or
rosrun uos_diffdrive_teleop uos_diffdrive_teleop_key 
```
Surely the robot will not perfectly drive as expected from the get-go, because every robot is of different size and components. Therefor a tweaking in either `volksbot.launch` or `key.launch` and their respective sub-files has to be made.
#### C. Understanding the connection

In order to help you [understand the connection](http://wiki.ros.org/ROS/Tutorials/UnderstandingNodes) and differences between those two nodes and methods, you can visualize the connection by typing in a new terminal:
```bash
goros
rosrun rqt_graph rqt_graph
```

Also you can output everything in a [ROS console](http://wiki.ros.org/ROS/Tutorials/UsingRqtconsoleRoslaunch#Using_rqt_console_and_rqt_logger_level) by opening two new terminals:
```bash
# terminal 1
goros && rosrun rqt_console rqt_console

# terminal 2 
goros && rosrun rqt_logger_level rqt_logger_level 
```



