
### Hardware requirements

Known to work with Arduino Mega. Microcontrollers with less memory may not work, given the overhead of the ROS Arduino library.

### Dependencies

[tom-f-oconnell/rosserial](https://github.com/tom-f-oconnell/rosserial)

[tom-f-oconnell/multi_tracker](https://github.com/tom-f-oconnell/multi_tracker)

[tom-f-oconnell/stimuli](https://github.com/tom-f-oconnell/stimuli)

[tom-f-oconnell/metatools](https://github.com/tom-f-oconnell/metatools)


Install these dependencies as you would install any ROS package from source, i.e.

- Make a catkin workspace
- Clone each repository to `<workspace>/src`
- `cd <workspace> && catkin_make`
- `source <workspace>/devel/setup.bash`
	- You can put this line in your `~/.bashrc` file, towards the bottom. If you do, make sure to put it AFTER the other line that sources some `setup.bash` file ROS uses.

TODO make a file to install all of these with `wstool`.


### To compile and install the Arduino code on the Arduino MEGA (on Ubuntu)

- Run `catkin_make` from your catkin workspace and source the `<workspace>/devel/setup.bash` file.
	1. `cd <workspace>` (I generally put my workspace at `~/catkin`)
	2. `catkin_make`
	3. `source <workspace>/devel/setup.bash`

- Make sure your user is in the `dialout` group, so it can use the USB ports to upload code (and other USB communication?)
	- To check, run `groups` as the same user you plan to run the experiment from, and look for the `dialout` in the list that prints.
	- If `dialout` is not there, run `sudo usermod -aG dialout <username>`.
		- You will need to log out and back in for this change to take effect.

- `cd` to the `libraries` directory under wherever you installed Arduino. If you installed it with the zip file on the Arduino website, this will be something like `<place-you-unzipped>/arduino-1.8.3/libraries`. Run
```
rosrun rosserial_arduino make_libraries.py .
```
This will setup the ROS libraries for compilation in the Arduino IDE.

- Open the Arduino IDE (make sure it is the same for which you installed `<arduino>/libraries`, in case multiple are installed.). Select appropriate port and board. Press upload.

### To run the experiment

- `cd` to a directory with configuration files for the tracking and for the stimulus delivery. See `freewalk/example_config` for examples.
- Run ```ROS_HOME=`pwd` roslaunch freewalk walk.launch```

#### Parameters to the launch file

- `stimuli_only`: (default=`False`) If `True`, will run the same stimuli (modulo randomization) that would happen with the same configuration.  For validation / measuring the odor output.

- `port`: (default=`/dev/ttyACM0`) The port your Arduino is connected to. The port your MEGA is connected to can often be determind by inspecting the list of available ports in the Arduino IDE.

- `video_only`: (default=`True`) The tracking only saves the background subtracted video, plus background frames, and some metadata. Nodes to do extra image processing and data association necessary to generate trajectories are not started.

#### To test your valves

Run ```ROS_HOME=`pwd` roslaunch stimuli test_valves.launch```


This will use the same `stimulus_parameters.yaml` file `walk.launch` expects in your current directory (`ROS_HOME`), and test the union of the lists in the `olf/left_pins` and `olf/right_pins` parameters.

