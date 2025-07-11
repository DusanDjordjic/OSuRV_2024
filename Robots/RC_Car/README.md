# Kako radi RC_Car

Remote kontrolu postizemo tako sto pokrecemo jedan **master** ros node na jendoj masini, a drugi ros node na raspberypi-u.
Master masina ce da da pokrece i joy node koji cita podatke koje joypad salje i njih proslednjuje dalje.
Pored joy node-a imamo i joy_remapper node koji podatke koje joy node salje remapira i salje ih dalje na /joy_dst topic.

Na RaspberryPi-u smo pokrenuli ros node koji slusa na /joy_dst topicu i dalje sa tim porukama nesto radi (nije nam bitan dalji deo za projekat).

## Kako smo napravili projekat

1.  Napravili smo novi folder pod nazivom "Remote_Joy" i u njemu "src" folder
2.  `cd ./Remote_Joy`
3.  Pokrenuli smo `catkin_make` da bi nam catkin napravio workspace
4.  Napravili smo catkin package `catkin_create_pkg joy_reader`
5.  Kopirali smo `_common.teleop.launch` file iz `vezba07_kernel_utils/ROS/arm_and_chassis_ws/src/common_teleop/launch/_commmon.teleop.launch` u `./vezba07_kernel_utils/Remote_Joy/src/joy_reader/launch/joy.launch`
6.  Kopirali smo `joy\_remapper.py` file iz `vezba07_kernel_utils/ROS/arm_and_chassis_ws/src/common_teleop/src/joy_remapper.py` u `.vezba07_kernel_utils/Remote_Joy/src/joy_reader/src/joy_remapper.py` i uradili `chmod +x .vezba07_kernel_utils/Remote_Joy/src/joy_reader/src/joy_remapper.py`
7.  Instalirali smo joy package komandom `sudo apt-get install ros-noetic-joy`
8.  Promenili smo launch file i ostavili samo stvari vezane za joy node i joy_remapper node
    1. Uradili smo `source devel/setup.xxxx` (xxxx zavisi od shell-a)
9.  Pokrenuli smo `joy_reader` node komandom `roslaunch joy_reader joy.launch`
10. Pokrenuli smo `rostopi echo /joy` da vidimo da li zapravo citamo, mapiramo i emitujemo podatke dalje na `/joy` topic
11. Pored master projekta imamo i vezba07_kernel_utils folder gde se nalazi postavka vezbe07. Ovaj projekat treba da radi na RaspberryPi-u.

## Kako pokrenuti projekat

### Master node (PC)

Mozemo uraditi `ls -la /dev/input/by-id` da bi videli povezan joysick kao u primeru:

```bash
# Jos neki output ce biti ovde verovatno nesto za mis
lrwxrwxrwx 1 root root  10 јун 22 21:34 usb-Sony_Interactive_Entertainment_Wireless_Controller-if03-event-mouse -> ../event31
lrwxrwxrwx 1 root root   6 јун 22 21:34 usb-Sony_Interactive_Entertainment_Wireless_Controller-if03-joystick -> ../js1
```

Na racunaru dolazimo do .../Robots/RC_Car/vezba07_kernel_utils/ROS/arm_and_chasis_ws/ i pokrecemo komandu ./tmuxer.py run_remote koja pokrece sve potrebne stvari za Master(PC) stranu: 
    roscore
    source devel/setup.sh
    mapira nas jsx i eventxy koji poziva u roslaunch joy_reader joy.launch joy_dev:=/dev/input/jsX joy_dev_ff=/dev/input/eventXY

Unutar routine polja tmuxer-a pokrenuta je komanda `rostopic echo /joy` i stiskamo dugmice na joypadu, trebalo bi da vidimo promene u konzoli. Sa ovim smo uspesno pokrenuli master node i joy_reader package.

### Slave node (RPI)

Slave node je sve ostalo sto treba da radi na raspberrypi-u. Potrebno je da ceo folder `vezba07_kernel_utils` prebacimo na raspberrypi. Pre nego sto bilo sta uradimo treba da postavimo ROS_MASTER_URI env na ROS_MASTER_URI koji smo dobili kao output `roscore` komande. Pazimo samo da ip bude dobar tj. ip treba da bude od racunara na kome smo pokrenuli `roscore`. Takodje setujemo ROS_HOSTNAME na nas ip (ip od raspberrypi-a). To sve radimo na sledeci nacin:

```bash
export ROS_MASTER_URI=http://<master-ip>:11311
export ROS_HOSTNAME=<rpi-ip>
```

Nakon svega toga pokrecemo program na raspberrypi-u sa `./tmuxer.py run_wc REMOTE=true` (`vezba07_kernel_utils/ROS/arm_and_chassis_ws/tmuxer.py` je lokacija) kao sto smo i do sada uvek radili.

Nakon svega ovoga imamo dve masine, na jednoj (desktop) je master node i joy_reader koji cita podatke od joypada, mapira ih i salje ih na /joy_dst topic, a na drugoj (rpi) je arm_and_chassis_ws koji prima podatke na /joy_dst topic-a i kontrolise auto.

## TODO

