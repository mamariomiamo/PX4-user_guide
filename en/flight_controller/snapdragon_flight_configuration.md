# Configure Snapdragon
## Autostart PX4 and Snap VIO
In order to boot both the ROS node and PX4 automatically on bootup, edit the `/etc/rc.local` file on the snapdragon to look like this: (note that the first line changed too!)

```
#!/bin/bash -e
#
# rc.local
#
# This script is executed at the end of each multiuser runlevel.
# Make sure that the script will "exit 0" on success or any other
# value on error.
#
# In order to enable or disable this script just change the execution
# bits.
#
# By default this script does nothing.

# Generate the SSH keys if non-existent
test -f /etc/ssh/ssh_host_dsa_key || dpkg-reconfigure openssh-server
cd /home/linaro
source /opt/ros/indigo/setup.bash
source /home/linaro/ros_ws/devel/setup.bash
sleep 5
roslaunch snapdragon_mavros_vislam mavros_vislam.launch &
sleep 10
(cd /home/linaro && ./px4 ros_ws/src/snap-vislam-ros/px4_configs/ekf2/mainapp.co
nf > mainapp_vislam.log)
exit 0
```

## Put Snapdragon back into Access Point Mode
If you followed the installation process closely so far, your Snapdragon Flight will still be in station mode. This could be a problem if you want to fly it outdoors where your home Wi-Fi is no longer available, so we recommend putting it back into access point mode.
```
/usr/local/qr-linux/wificonfig.sh -s softap
sudo reboot
```

## Adjust Parameters

| Parameter Name    | Recommended Value           |
|-------------------|-----------------------------|
| EKF2_HEIGHT_MODE  | 3   VISION                  |
| EKF2_AID_MASK     | 24 (VISION POS, VISION YAW) |
| MPC_THR_HOWVER    | 25 %               	  |
| MC_PITCHRATE_P    | 0.03                 	  |
| MC_PITCHRATE_D    | 0.001                 	  |
| MC_ROLLRATE_P     | 0.03                 	  |
| MC_ROLLRATE_D     | 0.001              	  |
| MC_RAWRATE_P      | 0.08                        |
| EKF2_IMU_POS_Z    | 0.030m    		  |
| EKF2_EV_POS_Z     | 0.03m			  |
| PWM_MIN	    | 1150us			  |
| CAL_MAG0_ROT	    | No rotation		  |
| CAL_MAG1_ROT	    | Yaw 90°			  |