# Memos

## Install Ubuntu 14.04

- [Ubuntu 14.04 release page](http://releases.ubuntu.com/14.04/)

## Install ROS for Ubuntu

- [Install ROS Jade](http://wiki.ros.org/jade/Installation/Ubuntu)

## Testing the install

- [ROS Tutorials](http://wiki.ros.org/ROS/Tutorials/)

-
  [Setup a catkin workspace](http://wiki.ros.org/ROS/Tutorials/InstallingandConfiguringROSEnvironment)

## Install ROS on EV3

-
  [Brickstrap](http://www.ev3dev.org/docs/tutorials/using-brickstrap-to-cross-compile/)

- Moriarty uses *ev3dev-jessie*, but the ev3dev tutorial for
  installing Brickstrap uses *ev3-ev3dev-jessie*.
- The location of the *yaml* file will dependo on the installation of
  brickstrap.  Do a `locate ev3dev.yaml` on the host machine to get
  the correct path.
- ROS distributions conflicted.  *Jade* was installed, but tutorial
  specified *Inidigo*.  Had to change everything to *Jade*.
- When running `catkin_make_isolated`, getting the error ` Assertion
  failed: check for file existence, but filename (RT_LIBRARY-NOTFOUND)
  unset.  Message: RT Library`.
  - Modified line 35 of
    `/opt/ros/jade/share/catkin/cmake/tools/rt.cmake` to include
    `UNIX` in the `IF` statement.  Suggested in this
    [post](http://answers.ros.org/question/66978/what-for-catkin-needs-to-link-to-librt-realtime-extension/).
- Use the ROS tutorials to setup the catkin workspace and
  beginner_tutorials package.
- To run the talker-listener scripts, need to set `ROS_MASTER_URI` and
  `ROS_IP` on the EV3 to properly connect to `roscore` on the desktop.

## Problems
- After loading the new image onto the SD card, testing the
  talker-listener program worked in only one direction.  The Ev3 could
  only be the talker, not the listener.  Using `roswtf` to try and
  diagnose.
- Running `roswtf` did not reveal any errors on the host that were
  interfering with the program.
- Running `roswtf` on the robot showed an error that said `host
  nick-office is unknown`.  The environment variables `ROS_HOSTNAME`,
  `ROS_IP`, and `ROS_MASTER_URI` needed to be set on the host
  machine. After exporting these in `~/.bashrc` and restarting the
  terminal sessions, everything worked.
