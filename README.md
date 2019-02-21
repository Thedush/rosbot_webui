# ROSbot_webui

This repository contains interface, which enable you to use web browser to control ROSbot or other ROS comaptible device.

Interface is based on [RobotWebTools project](https://github.com/RobotWebTools).

## How does it work

A `rosbridge_server` package is a tool which exposes ROS interface through websocket. 

JavaScript application based on `roslibjs` in web browser uses websocket to subscribe and publish ROS topics.

## Installation

To install required dependencies:

`sudo apt update`

`sudo apt install python-tornado python-pip ros-kinetic-rosbridge-suite ros-kinetic-web-video-server`

Clone this repository:

`cd /home/husarion/ros_workspace/src`

`git clone https://github.com/husarion/rosbot_webui.git`

Clone `husarion_ros` repository:

`git clone https://github.com/lukaszmitka/husarion_ros.git`


Build workspace:

`cd /home/husarion/ros_workspace`

`catkin_make`

`source devel/setup.sh`


JavaScript application needs to be hosted on http server, we will use Nginx for this purpose, but you can use server of our choice.

Install Nginx:

`sudo apt install nginx`

Then edit configuration file:

`sudo nano /etc/nginx/sites-enabled/default`

and change line:

`       root /var/www/html;`

to:

`        root /home/husarion/ros_workspace/src/rosbot_webui/edit;`

Restart Nginx:

`sudo systemctl restart nginx`

Nginx will start automatically at boot. You do not need to launch it again.

By now you should be able to see draft of web panel in your browser. Just type IP addrees of your ROSbot in adress bar and it shoule be visible. Although you will not be able to control rosbot. It is still required to launch websocket and apropriate nodes.

# Usage

Application subscribes some topics to obtain robot pose, battery state, image from camera and generated map, it also publishes topics with velocity commands and path planning or explortion tasks. Corresponding nodes must be launched at ROSbot to enable full functionality of UI. Example launch file with supporting scripts and configuration files is included in this repository.

You can use it:

`roslaunch rosbot_webui demo.launch`

Go to the web browser and type `http://device_name.local/` in adress bar, substituting phrase `device_name` with name of your ROSbot defined in Husarnet panel. Now you will be able to control ROSbot.

If you do not have Husarnet configured, you can use local IP of your device.


# Launch webui at startup

If you want web UI to launch at robot startup edit file '/etc/rc.local`:

```
sudo nano /etc/rc.local
```

Find line:
```
exit 0
```
at the end of the file, and add above it:

```
/home/husarion/ros_workspace/src/scripts/autostart.sh
```
