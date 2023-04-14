# CS/ME/ECE/AE/BME 7785 Gazebo Environments
Gazebo simulation can be utilized for code development and initial testing. This repository holds simulation environments for CS 7785: Intro to Robotics Research class. Please see instructions below to setup the environment.

## (Temporary) Setup Instructions
These instructions assume you have already completed the necessary steps to setup ROS, Gazebo, and Turtlebot3. Once this has been done, the setup is straight forward. 

### Lab 1/2/3 Instructions
#### Setup
For the first three labs, the Gazebo simulation uses the same world. In order to setup this environment download the Lab2 folder. Once downloaded, copy the launch file to your `/path/to/ros2_ws/src/install/turtlebot3_gazebo/share/turtlebot3_gazebo/launch` directory. Then move to the `lab2_worlds` folder to the `/path/to/ros2_ws/src/install/turtlebot3_gazebo/share/turtlebot3_gazebo/worlds` directory. Once this is done, you will need to run `. install/local_setup.bash` in your ROS directory. 

#### Running Environment
```
ros2 launch turtlebot3_gazebo lab2_world.launch.py
```

#### Code Development
In order to use this environment, you simply need to:
1. Run your lab code (i.e. `chase_object.py`)
2. Select *translation mode* in Gazebo (intersecting double arrows in top left corner)
3. Click on the sphere
4. Drag the sphere where you wish the turtlebot to follow/rotate/chase


### Lab 4 Instructions [WIP]

### Lab 5 Instructions [WIP]

### Lab 6 Instructions 

1. Copy the `turtlebot3_maze.launch.py` file to `/path/to/turtlebot3_simulations/turtlebot3_gazebo/launch` folder
2. Copy the `maze_world_7785` folder to `/path/to/turtlebot3_simulations/turtlebot3_gazebo/worlds`
3. Copy the folders in the `maze_models` directory (i.e. 3ft_wall, sign_right2, etc.)  into the `/.gazebo/models/` folder
4. Go to ros2_ws and rebuild `colcon build --symlink-install`
4. The `sim_images` folder contains images of the signs in simulation these can be used to train a classifer. 

Finally to run the environment, ensure that your workspace is properly sourced and run the following command:
```
ros2 launch turtlebot3_gazebo turtlebot3_maze.launch.py
```

Note: the `/.gazebo` folder is a *hidden* folder. It is most likely in your `/home` folder. Using Nautilus, you can use `cntrl-h` to view hidden files or in the terminal you can use ls -a. 


## Notes on Gazebo Simulation:
Unlike the real Turtlebot3 robot, **Gazebo does NOT use the `CompressedImage` topic**. Instead, it uses the `Image` topic. Therefore, when subscribing to the camera node, be sure to use the `/camera/image_raw` topic and NOT the `/camera/image/compressed`. When you deploy your code on the real robot, you will need to change this back. This also affects to the `CvBridge`. You will need to use the `imgmsg_to_cv2()` method instead of `compressed_imgmsg_to_cv2`.

**Sample Video Subscriber:**
```
# Gazebo Video Subscriber
self._video_subscriber = self.create_subscription(
        Image,
        '/camera/image_raw',
        self._image_callback,
        1)
self._video_subscriber # Prevents unused variable warning.
```

**Sample image_callback:**
```
 def _image_callback(self, CompressedImage):	
        self._imgBGR = CvBridge().imgmsg_to_cv2(Image, "bgr8")
```

