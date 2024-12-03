# Pokretanje roscore listener and talker nodes

## laptop : MASTER node

export ROS_MASTER_URI=http://<ip>:11311
export ROS_HOSTNAME=<ip>


roscore - start the master node

rosrun rospy_tutorials talker - start talker node

## rpi : SLAVE node

export ROS_MASTER_URI=http://<master-ip>:11311
export ROS_HOSTNAME=<rpi-ip>

rosrun rospy_tutorials listener - start listener node 


# Pokretanje joy package

napavimo package sa catkin_package_create (kako vec)
stavimo launch fajl da pokrece joy_node koji smo kopirali is _common_teleop.launch fajla

pokrenemo roscore i roslauch <package_name> <launch_fajl> na master (PC)

odemo u rpi obrisemo taj joy_node is _common_teleop.launch i obrisemo argument za joy_remapper
odemo u joy_remapper.py i obrisemo kod koji cita prvi argument sto je /dev/input/js0 kad sve radi na rpi
