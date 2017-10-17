
Hardware requirements:
-known to work with Arduino Mega. Microcontrollers with less memory may push not work given the overhead of the ROS Arduino library.

Install the following as you would install any ROS package from source, i.e.:
-make a catkin workspace
-clone each repository to <workspace>/src
-cd <workspace> && catkin_make
-source <workspace>/devel/setup.bash
	-you can put this line in your ~/.bashrc file, towards the bottom. if you do, make sure to put it AFTER the other line that sources some setup.bash file ROS uses.

Dependencies:
https://github.com/tom-f-oconnell/rosserial
https://github.com/tom-f-oconnell/multi_tracker
https://github.com/tom-f-oconnell/stimuli
https://github.com/tom-f-oconnell/metatools

To compile and install the Arduino code on the Arduino MEGA:
(on Ubuntu)
-run catkin_make from your catkin workspace and source the devel/setup.bash file.
-make sure your user is in the 'dialout' group, so it can use the USB ports to upload code (and other USB communication?)
-cd to the "libraries" directory under wherever you installed Arduino. If you installed it with the zip file on the Arduino website, this will be something like "./arduino-1.8.3/libraries". Run "rosrun rosserial_arduino make_libraries.py ." This will setup the ROS libraries for compilation in the Arduino IDE.
-open the arduino program (make sure it is the same for which you installed <arduino>/libraries, in case multiple are installed.). select appropriate port and board. press upload.


To run the experiment:
-cd to a directory with configuration files for the tracking and for the stimulus delivery.
-run "ROS_HOME=`pwd` roslaunch freewalk walk.launch"
TODO add examples
