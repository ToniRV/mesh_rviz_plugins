# mesh_rviz_plugins
RVIZ plugins to display a textured 3D mesh, a refactoring of plugins in: https://github.com/robustrobotics/flame_ros

<div align="center">
    <img src="docs/media/SparkVIO_ROS_mesh.gif">
</div>

## Installation

- Install ROS + the following dependencies: (change `melodic` for your ROS distribution):
```bash
sudo apt-get install ros-melodic-image-geometry ros-melodic-pcl-ros ros-melodic-cv-bridge
```

- Then, just clone in your workspace, catkin build it, source the workspace and use rviz as you would normally do.

```bash
# Setup catkin workspace
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws/
catkin init

# Add workspace to bashrc.
echo 'source ~/catkin_ws/devel/setup.bash' >> ~/.bashrc

# Clone repo
cd ~/catkin_ws/src
git clone https://github.com/ToniRV/mesh_rviz_plugins.git

# Install dependencies from rosinstall file using wstool
wstool init
wstool merge mesh_rviz_plugins/install/mesh_rviz_plugins.rosinstall
wstool update

# Compile code
catkin build

# Refresh workspace
source ~/.bashrc
```

## Usage & Example output

After loading rviz whit this plugin, add a topic of type `TexturedMesh`, for which you can use a dropdown menu in rviz to subscribe to a point cloud `pcl_msgs` and an image for texture.
Check usage and example output in [spark_vio_ros](https://github.com/MIT-SPARK/Kimera-VIO-ROS).
