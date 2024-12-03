# Kako radi RC_Car

Remote kontrolu postizemo tako sto pokrecemo jedan **master** ros node na jendoj masini, a drugi ros node na raspberypi-u.
Master masina ce da da pokrece i joy node koji cita podatke koje joypad salje i njih proslednjuje dalje.
Pored joy node-a imamo i joy_remapper node koji podatke koje joy node salje remapira i salje ih dalje na TODO topic.

Na RaspberryPi-u smo pokrenuli ros node koji slusa na TODO topicu i dalje sa tim porukama nesto radi (nije nam bitan dalji deo za projekat).

## Kako smo napravili projekat

1. Setup

   1. Napravili smo novi folder pod nazivom "master" i u njemu "src" folder
   2. `cd ./master`
   3. Pokrenuli smo `catkin_make` da bi nam catkin napravio workspace
      1. Uradili smo `source devel/setup.xxxx` (xxxx zavisi od shell-a)
   4. Napravili smo catkin package `catkin_create_pkg joy_reader`
   5. Kopirali smo `_common.teleop.launch` file iz `vezba07_kernel_utils/ROS/arm_and_chassis_ws/src/common_teleop/launch/_commmon.teleop.launch` u `./master/src/joy_reader/launch/joy.launch`
   6. Kopirali smo `joy\_remapper.py` file iz `vezba07_kernel_utils/ROS/arm_and_chassis_ws/src/common_teleop/src/joy_remapper.py` u `./master/src/joy_reader/src/joy_remapper.py` i uradili `chmod +x ./master/src/joy_reader/src/joy_remapper.py`
   7. Instalirali smo joy package komandom `sudo apt-get install ros-noetic-joy`
   8. Promenili smo launch file i ostavili samo stvari vezane za joy node i joy_remapper node
   9. Pokrenuli smo `joy_reader` node komandom `roslaunch joy_reader joy.launch`
   10. Pokrenuli smo `rostopi echo /joy` da vidimo da li zapravo citamo, mapiramo i emitujemo podatke dalje na `/joy` topic

1. Napravili smo novi catkin workspace i u njemu joy_reader package koji pomocu launch fajla pokrece joy node.

https://wiki.ros.org/ROS/Tutorials/InstallingandConfiguringROSEnvironment

rosrun rospy_tutorials talker - start talker node

## rpi : SLAVE node

export ROS_MASTER_URI=http://<master-ip>:11311
export ROS_HOSTNAME=<rpi-ip>

rosrun rospy_tutorials listener - start listener node

# Pokretanje joy package

1. Napavimo package sa catkin_package_create
2. Stavimo launch fajl da pokrece joy_node koji smo kopirali iz \_common_teleop.launch fajla

pokrenemo roscore i roslauch <package_name> <launch_fajl> na master (PC)

odemo u rpi obrisemo taj joy_node is \_common_teleop.launch i obrisemo argument za joy_remapper
odemo u joy_remapper.py i obrisemo kod koji cita prvi argument sto je /dev/input/js0 kad sve radi na rpi

# Pokretanje

## laptop : MASTER node

stavi ovo u .bashrc i uradi `source .bashrc`

```
export ROS_MASTER_URI=http://<master-ip>:11311
export ROS_HOSTNAME=<master-ip>
```

`cd master`

`source devel/setup.zsh` da ucitamo catkin config

connect joypad

roscore - start the master node

`cd master/src`
roslaunch dusantest joy.launch

## RPI : Slave node

stavi ovo u .bashrc i uradi `source .bashrc`

```
export ROS_MASTER_URI=http://<master-ip>:11311
export ROS_HOSTNAME=<my-ip>
```

run ./tmuxer run from ROS/arm_and_chassis/
