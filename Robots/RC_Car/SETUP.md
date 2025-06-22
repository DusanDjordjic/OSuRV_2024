# Kako instalirati i podesiti ROS1

Potreban operativni sistem je Ubutnu 20.04 jer ROS1 ne radi na novijim.

1. Instalacija
   1. Instalirati **Noetic ROS** verziju, i sve ostalo sto je spomenuto na [link](https://wiki.ros.org/noetic/Installation/Ubuntu). Intsalirati **Desktop-Full Install** verziju.
   2. Obavezno odraditi `source ...` komande
   3. Instalirati **Dependencies for building packages**
   4. Restartovati terminal
   5. Pokrenuti neku ros komandu i budite sigurni da dobijate neki ros error, a ne command not found. Na primer, pokretanjem `rosversion` treba da vidimo error
   ```
   usage: rosversion [-h] [-s] [-d] [-a] [package]
   rosversion: error: one of the arguments package -d/--distro -a/--all is require
   ```
2. Prateci ovaj [tutorial](http://wiki.ros.org/ROS/Tutorials/CreatingPackage) smo napravili catkin workspace i jedan package u tom workspace-u
3. Sledeci korak je da build-ujemo package [tutorial](http://wiki.ros.org/ROS/Tutorials/BuildingPackages)
