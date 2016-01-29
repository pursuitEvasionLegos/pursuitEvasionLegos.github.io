---
layout: post
title:  "Friday Update"
categories: Friday-Updates
---

This week we focused on tracking via webcams and updating the
brickstrap image.

# Tracking M
- Color matching
- SIFT algorithm


# Brickstrap updates
Want to setup the brickstrap image to compile on host machine.
and use NFS to run files on EV3.  First, we need to cleanup the
Brickstrap image.
- Setting up the brickstrap environment.
- After opening brickstrap shell, switch to `robot` user to run
  commands.
  - Needs a `sudo rosdep fix-permissions` and `rosdep-update`.
- Add `nick-office` to `/etc/hosts`
- Add the following to `.bashrc`
  - `source /opt/ros/jade/setup.bash`
  - `source ~/catking_ws/devel/setup.bash`
  - `export ROS_MASTER_URI=hostname-of-host-machine:11311`
  - `` export ROS_IP=`hostname -I` ``
  - `` export ROS_HOSTNAME=`hostname -I` ``


# Setup NFS for cross-compiling
- Install `nfs-common` on host and edit `/etc/exports`
- Follow
  [nfs setup instructions](https://github.com/ev3dev/ev3dev/wiki/Set-Up-An-nfs-FileShare)
- In `/etc/fstab` on the EV3, the open `vers=3` should be
  `nfsvers=3`.
- Make sure the directories are created on both the host and the EV3.
- To mount on EV3 start the services `nfs-common` and `rpcbind`
  using `sudo systemctl start`.
