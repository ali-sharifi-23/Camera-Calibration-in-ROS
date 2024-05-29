# Camera-Calibration-in-ROS
# Camera-Calibration-in-ROS
- **ROS Setup**
	- Prerequisites
		- **Python:** Python version 3.8 should be installed in a new environment in conda
		  collapsed:: true
			- `conda create --name myenv python=3.8`
			- `conda activate myenv`
			- `python --version`
		- **OpenCV:** opencv version 4.2.0 should be installed on the created conda environment using conda command
			- `conda install -c conda-forge opencv=4.2.0`
		- **Camera Calibration ROS Package:**
	- Calibration Procedure
		- Monocular
			- **Step 1:** Open terminal and run ROS core
				- `roscore`
			- **Step 2:** Open another terminal and start camera node
				- `rosrun cv_camera cv_camera_node`
			- **Step 3:** Run the camera calibration tool
				- `rosrun camera_calibration cameracalibrator.py --size 8x6 --square 0.024 image:=/cv_camera/image_raw camera:=/cv_camera`
			- **Result:**
				-
				  ```javascript
				  **** Calibrating ****
				  mono pinhole calibration...
				  D = [-0.10953317803683454, 0.14884182449632868, -0.0007907678320178917, -0.00266869461877111, 0.0]
				  K = [542.7803046949343, 0.0, 320.43574145134596, 0.0, 543.1186635824461, 236.98327507005266, 0.0, 0.0, 1.0]
				  R = [1.0, 0.0, 0.0, 0.0, 1.0, 0.0, 0.0, 0.0, 1.0]
				  P = [534.1416015625, 0.0, 318.17463484144537, 0.0, 0.0, 535.6026611328125, 236.15932750747015, 0.0, 0.0, 0.0, 1.0, 0.0]
				  None
				  # oST version 5.0 parameters
				  
				  
				  [image]
				  
				  width
				  640
				  
				  height
				  480
				  
				  [narrow_stereo]
				  
				  camera matrix
				  542.780305 0.000000 320.435741
				  0.000000 543.118664 236.983275
				  0.000000 0.000000 1.000000
				  
				  distortion
				  -0.109533 0.148842 -0.000791 -0.002669 0.000000
				  
				  rectification
				  1.000000 0.000000 0.000000
				  0.000000 1.000000 0.000000
				  0.000000 0.000000 1.000000
				  
				  projection
				  534.141602 0.000000 318.174635 0.000000
				  0.000000 535.602661 236.159328 0.000000
				  0.000000 0.000000 1.000000 0.000000
				  
				  D = [-0.10953317803683454, 0.14884182449632868, -0.0007907678320178917, -0.00266869461877111, 0.0]
				  K = [542.7803046949343, 0.0, 320.43574145134596, 0.0, 543.1186635824461, 236.98327507005266, 0.0, 0.0, 1.0]
				  R = [1.0, 0.0, 0.0, 0.0, 1.0, 0.0, 0.0, 0.0, 1.0]
				  P = [534.1416015625, 0.0, 318.17463484144537, 0.0, 0.0, 535.6026611328125, 236.15932750747015, 0.0, 0.0, 0.0, 1.0, 0.0]
				  # oST version 5.0 parameters
				  ```
				- **D (Distortion Coefficients):**
					- This array `[-0.109533, 0.148842, -0.000791, -0.002669, 0.0]` represents the coefficients for lens distortion correction.
					- These coefficients correct the distortion caused by the camera lens.
					- The first two values usually represent the radial distortion (`k1`, `k2`).
					- The next two values are the tangential distortion (`p1`, `p2`).
					- The fifth coefficient (`k3`) is sometimes used for higher-order radial distortion.
				- **K (Camera Matrix):**
				  collapsed:: true
					- The matrix
					    
						$$K=\begin{bmatrix}542.780305 & 0 & 320.435741\\ 0 & 543.118664 & 236.983275\\ 0 & 0 & 1\end{bmatrix}$$
					  contains intrinsic parameters like focal length (first and second elements on the diagonal) and the optical center (third element of the first two rows) of the camera. These parameters are used to convert 3D camera coordinates to 2D image coordinates.
				- **R (Rectification Matrix):**
					- The matrix
					    
						$$R=\begin{bmatrix}1 & 0 & 0\\ 0 & 1 & 0\\ 0 & 0 & 1\end{bmatrix}$$
					  is an identity matrix here, indicating that no rectification transformation (rotation) is needed for the stereo setup.  
					- This 3x3 identity matrix is used for stereo camera systems to align the left and right camera images.
					- In this case, it indicates no rectification transformation (identity matrix).
				- **P (Projection Matrix):**
				  collapsed:: true
					- The matrix
					    
						$$P=\begin{bmatrix}534.141602 & 0 & 318.174635 & 0\\ 0 & 535.602661 & 236.159328 & 0\\ 0 & 0 & 1 & 0\end{bmatrix}$$ 					    
					  includes the camera matrix (K) and the translation vector (which is zero here). It maps 3D points in the camera coordinate system to 2D points in the image coordinate system.  
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
			- ![image.png](../assets/image_1716122106086_0.png)
	- Errors
		- **error 1:** camera calibration window does not open up (without any error)
			- **Troubleshooting:** opencv version 4.2.0 should be installed using conda
			  `conda install -c conda-forge opencv=4.2.0`

