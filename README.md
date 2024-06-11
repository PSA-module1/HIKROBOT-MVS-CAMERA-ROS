# hikrobot-ros
## 1. Setting Static IP using MVS Software
MVS (Machine Vision Studio) software is used to configure cameras, including setting a static IP address which is crucial for reliable network connectivity in robotic applications. Follow these steps to set up your camera:
[Download Link](https://www.hikrobotics.com/en/machinevision/service/download): Download `Machine Vision Software MVS2.1.2（Linux_x86_64）`
### Steps:
1. **Connect the Camera**: Connect your camera to the same network as your computer running MVS.
2. **Launch MVS Software**: Start the MVS application on your computer.
3. **Detect the Camera**: Use the device discovery tool in MVS to find your camera on the network.
4. **Configure IP Settings**: Select your camera from the list, then navigate to the network settings. Set the IP configuration to static and enter the desired IP address, subnet mask, and default gateway.
5. **Save and Restart the Camera**: Save the settings and reboot the camera to apply the new network configuration.
## 2. Running Shell Scripts to Start ROS Nodes
Shell scripts are utilized to streamline the process of launching ROS nodes for each camera. The script sets environment variables corresponding to ROS parameters, ensuring each camera node operates with the correct configuration.
### Script Configuration
Edit the shell script to modify essential parameters for each camera instance before running it. These parameters include container names, serial numbers, and topic names.
#### Key Parameters:
- **Container Name**: Identifies the Docker container running the ROS node.
- **Serial Number**: Unique identifier for each camera.
- **Topic Name**: ROS topic under which camera images are published.
Example shell script snippet:
```bash
# Setting up environment variables for the ROS node
CONTAINER_NAME="hikvision_driver_2_0"
SERIAL_NUMBER="DA1785153"
TOPIC_NAME="/hik_camera_2_0/rgb"
```
### Modifying ROS Node Operation:
Additional parameters control the operation rate and image resolution of the ROS node.
- **ROS Loop Rate**: Determines the frequency at which the ROS node publishes images.
- **Image Dimensions**: Set the width and height for the captured images.
Adjust these in your script as follows:
```bash
ROS_LOOP_RATE="20" # Frequency of image publication
WIDTH="1080" # Width of the images
HEIGHT="720" # Height of the images
```
## Running the Script
After configuring the script with the correct parameters, execute it to start the ROS nodes. Ensure that Docker or your ROS environment is correctly set up to run the nodes.
```bash
./run_hikrobot_2-0
./run_hikrobot_2-1
./run_hikrobot_2-2
```
## Conclusion
By following these instructions, you can ensure that each camera is configured with a static IP for reliable connectivity, and that each ROS node is properly set up with unique settings for optimal operation. Adjust the parameters in the script as needed for different camera setups.
---
# HIKROBOT-MVS-CAMERA-ROS
The ros driver package of Hikvision Industrial Camera SDK. Support configuration parameters, the parameters have been optimized, and the photos have been transcoded to rgb format.
Please install mvs, https://blog.csdn.net/weixin_41965898/article/details/116801491

# Install
```
mkdir -p ~/ws_hikrobot_camera/src
git clone https://github.com/luckyluckydadada/HIKROBOT-MVS-CAMERA-ROS.git ~/ws_hikrobot_camera/src/hikrobot_camera
cd ~/ws_hikrobot_camera
catkin_make
```
# launch run
```
source ./devel/setup.bash 
roslaunch hikrobot_camera hikrobot_camera.launch
```
# launch run
use rviz subscribe topic： /hikrobot_camera/rgb
```
source ./devel/setup.bash 
roslaunch hikrobot_camera hikrobot_camera_rviz.launch
```
