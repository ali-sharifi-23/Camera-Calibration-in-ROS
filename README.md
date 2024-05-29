## Camera-Calibration-in-ROS
# Prerequisites
- ROS:
	- https://wiki.ros.org/ROS/Installation
- Camera Calibration ROS Package:
	- `sudo apt install ros-<ros1-distro>-camera-calibration`
- Python: Python version 3.8 should be installed in a new environment in conda
	- `conda create --name myenv python=3.8`
	- `conda activate myenv`
	- `python --version`
- OpenCV: opencv version 4.2.0 should be installed on the created conda environment using conda command
	- `conda install -c conda-forge opencv=4.2.0`
# Calibration Procedure
- Monocular
	- **Step 1:** Open terminal and run ROS core
		- `roscore`
	- **Step 2:** Open another terminal and start camera node
		- `rosrun cv_camera cv_camera_node`
	- **Step 3:** Run the camera calibration tool
		- `rosrun camera_calibration cameracalibrator.py --size 8x6 --square 0.024 image:=/cv_camera/image_raw camera:=/cv_camera`
- Stereo
	- **Step 1:** Open terminal and run ROS core
		- `roscore`
	- **Step 2:** Start camera nodes
		- Terminal 1: Start left camera node
			- `rosrun cv_camera cv_camera_node _device_id:=2 _frame_id:=left_camera __name:=left_camera`
		- Terminal 2: Start right camera node
			- `rosrun cv_camera cv_camera_node _device_id:=3 _frame_id:=right_camera __name:=right_camera`
	- **Step 3:** Run the camera calibration tool
		- `rosrun camera_calibration cameracalibrator.py --approximate 0.1 --size 8x6 --square 0.024 right:=/right_camera/image_raw left:=/left_camera/image_raw right_camera:=/right_camera left_camera:=/left_camera`
# Errors
- error 1: camera calibration window does not open up (without any error)
	- **Troubleshooting:** opencv version 4.2.0 should be installed using conda
	  `conda install -c conda-forge opencv=4.2.0`

