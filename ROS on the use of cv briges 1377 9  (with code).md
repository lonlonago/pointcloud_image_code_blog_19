Recently, I want to use OpenCV and ROS to realize the splicing of point clouds and realize 3D reconstruction. After learning the basic use of kinect, we know that we can directly use the ROS package to get point clouds, depth maps, RGB diagrams and other information. 

 Roslaunch openni_launch openni.launch (depth map, color map, and some clouds have been obtained) 

 Rosrun openni_camera openni_node (depth and color) 

 Then to realize the splicing of point clouds, you need to use cv_bridge to convert the data format of ROS to the data format that Opencv can use. That is, a development kit that provides the interface between ROS and OpenCV library. 

 ![avatar]( aHR0cDovL2ltYWdlczIwMTUuY25ibG9ncy5jb20vYmxvZy85NzYzOTQvMjAxNzAzLzk3NjM5NC0yMDE3MDMyOTExMDMzOTcwMS0xNzg4Mzk2NDA3LnBuZw) 

 (1) Converting ROS image information to OpenCV image cvbridge defines the type of opencv image cvimage, including the encoding and ROS header. Cvimage contains accurate information sensor_msgs /image, so we can convert both data types. Cvimage class format: 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573719238
  ```  
 When converting a ROS's sensor_msgs /image information into a cvimage, cvbridge has two different uses: 

 (1) If we want to modify a bit of data, we must copy the ROS information data as a copy. 

 (2) Do not modify the data. ROS information that can be safely shared, rather than copied data. 

 The CvBridge conversion provides the following features for CvImage: 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573719238
  ```  
 The input is an image message pointer, with optional encoding parameters. Encoding refers to the type of cvimage. 

 Tocvcopy copies the image data from the ROS message and can freely modify the returned cvimage. Even when the source and destination encodings match. 

 Tocvshare will avoid copying, the message data in the ROS puts a pointer back to the cv :: mat,, if the source and destination encodings match. As long as you keep a copy of the returned cvimage, the ROS message data will not be freed. If the encodings do not match, it will allocate a new buffer and perform the conversion. The returned cvimage will not be modified, as it may share data with the ROS image information, which in turn may be shared with other callback functions. Note: It is more convenient to overload the tocvshare secondary, the conversion when a pointer points to other information types (such as stereo_msgs/disparityimage) contains a sensor_msgs /image. 

 If there is no encoding (or rather, empty string), the destination image encoding will be the same as the image message encoding. In this case tocvshare guarantees not to copy the image data. The image encoding can be any of the following opencv image encodings: 

 Introduces the common data encoding forms in the set cvbridge, cv_bridge can selectively convert color and depth information. In order to use the specified feature encoding, there are the following encoding forms in the set: mono8: CV_8UC1, grey release image mono16: CV_16UC1, 16-bit grey release image bgr8: CV_8UC3, with color information and the order of color is BGR order rgb8: CV_8UC3, with color information and the order of color is RGB order bgra8: CV_8UC4, BGR color image, and with alpha channel rgba8: CV_8UC4, CV, RGB color image, and with alpha channel, Note: The two image encoding formats mono8 and bgr8 are the encoding formats of most OpenCV. 

 Finally, CvBridge will recognize Bayer pattern encodings as having OpenCV type 8UC1 (8-bit unsigned, one channel). It will not perform conversions to or from Bayer pattern; in a typical ROS system, this is done instead by image_proc. CvBridge recognizes the following Bayer encodings: 

 1. Convert OpenCV images to ROS image format 

 To convert a CvImage into a ROS image message, use one the toImageMsg() member function: 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573719238
  ```  
 For example? 

 First, add the dependencies to the created package CMakeLists.txt. They are as follows: 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573719238
  ```  
  Create a cv_ros_beginner file under the src file. The file contents are as follows 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573719238
  ```  
 Then add it in CMakeLists.txt. 

   add_executable(cv_ros_beginner src/cv_ros_beginner.cpp)  target_link_libraries(cv_ros_beginner    ${catkin_LIBRARIES}  ) 

 Return to catin_ws catkin_make to generate the executable file 

 Then when we check the results, we need to 

 image_sub_ = it_.subscribe( 

 Just move the following official website procedures, of course you can achieve more handling 

 Then those who are interested in the use of ROS and PCL scan the QR code to follow and share with me. 

 ![avatar]( aHR0cDovL2ltYWdlczIwMTUuY25ibG9ncy5jb20vYmxvZy85NzYzOTQvMjAxNzA0Lzk3NjM5NC0yMDE3MDQwMzEyMzM1NDUwMy0xNTUwNDMyODcucG5n) 

