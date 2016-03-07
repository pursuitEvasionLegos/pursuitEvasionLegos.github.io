# Tutorial for building ROS on the EV3

We thank Alex Moriarty for his work in getting ROS on the EV3.  A lot
of this tutorial is taken from his github repository.

## Setting up Brickstrap

- `mkdir ~/brickstrap && cd ~/brickstrap`
- `brickstrap -b ev3-ev3dev-jessie -d ev3dev-ros create-rootfs`
- `brickstrap -b ev3-ev3dev-jessie -d ev3dev-ros shell`
- `cd /host-rootfs/home/user`
- `apt-get update`
- `apt-get install unzip bzip2 build-essential`
- `wget
  https://raw.githubusercontent.com/moriarty/ros-ev3/master/ros-dependencies.debs`
- `bash ros-dependencies.debs`
- `pip install -U rosdep rosinstall_generator wstool rosinstall
  catkin_pkg rospkg`
- `wget
  http://netcologne.dl.sourceforge.net/project/sbcl/sbcl/1.2.7/sbcl-1.2.7-armel-linux-binary.tar.bz2`
- `tar -xjf sbcl-1.2.7-armel-linux-binary.tar.bz2`
- `cd sbcl-1.2.7-armel-linux`
- `INSTALL_ROOT=/usr/local sh install.sh`
- `cd ..`
- `rosdep init`
- Add `yaml
  https://raw.githubusercontent.com/moriarty/ros-ev3/master/ev3dev.yaml`
  as the first line under `os-specific listings first` in
  `/etc/ros/rosdep/sources.list.d/20-default.list`.  Edit the file
  using `nano /etc/ros/rosdep/sources.list.d/20-default.list`.
- `rosdep update`
- `mkdir ros_comm && cd ros_comm`
- `rosinstall_generator ros_comm --rosdistro indigo --deps --wet-only
  --tar > indigo-ros_comm-wet.rosinstall`
- `wstool init -j2 src indigo-ros_comm-wet.rosinstall`
- `rosdep check --from-paths src --ignore-src --rosdistro indigo -y
  --os=debian:jessie`
- `./src/catkin/bin/catkin_make_isolated --install --install-space
  /opt/ros/indigo -DCMAKE_BUILD_TYPE=Release`

## Initialize catkin workspace for drivers
- `mkdir ros_pkgs`
- In a new terminal, outside of brickstrap, `cd ~/ros_pkgs` some git
  repositories, `git clone https://github.com/ros/common_msgs.git` and
  `git clone https://github.com/pursuitEvasionLegos/robot_driver.git`.
- These are now available in brickstrap at
  `/host-rootfs/home/user/ros_pkgs`.  Now back to the brickstrap shell.
- `su robot`
- `source /opt/ros/indigo/setup.bash`
- `sudo rosdep fix-permissions`
- `rosdep update`
- `mkdir -p /home/robot/catkin_ws/src && cd /home/robot/catkin_ws/src`
- `catkin_init_workspace`
- `cd ..`
- `catkin_make`
- `sudo cp -r /host-rootfs/home/nick/ros_pkgs/* ./src/`
- `cd ./src`
- `sudo chown -R robot:robot *`
- `cd ..`
- `catkin_make`
- `cd ~/`
- Now edit `.bashrc` using `nano ~/.bashrc` and add the following
  lines to the end of the file

  ```
  if [ -f /opt/ros/indigo/setup.bash ]
  then
	  source /opt/ros/indigo/setup.bash
  fi

  if [ -f ~/catkin_ws/devel/setup.bash ]
  then
	  source ~/catkin_ws/devel/setup.bash
  fi

  export ROS_MASTER_URI=https://hostname-of-master:11311/

  export ROS_IP=`hostname -I`

  export ROS_HOSTNAME=`hostname -I`

  ```

- Now edit `/etc/hosts` using `sudo nano /etc/hosts` and add the
  following lines to the end of the file
  ```
  ip-of-master     hostname-of-master
  ```

- Exit the brickstrap shell.  Run `exit` twice.


## Create image
- `brickstrap -b ev3-ev3dev-jessie -d ev3dev-ros create-tar`
- `brickstrap -b ev3-ev3dev-jessie -d ev3dev-ros create-image`
- `sudo dd bs=4M if=/path/to/brickstrap/image of=/path/to/sd/card`
