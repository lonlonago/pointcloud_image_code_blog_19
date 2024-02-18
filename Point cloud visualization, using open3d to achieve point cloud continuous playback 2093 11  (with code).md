![avatar]( 067c5be8deee4149a4815dbbb3fe1b47.gif) 

  After the model training is completed, in addition to seeing whether the quantitative indicators such as AP are better, we also need to visualize the results and directly observe the output results of the model. Often we have more data. If we look at a single frame, it will be more troublesome. We need to close the window frequently. It is best to play the data and the model's reasoning results directly and continuously. There are three methods: 

 I give the implementation process of open3d continuous playback visualization in a scene in the waymo dataset. The sample data has been uploaded to the network disk. (Only the first and third are released here. The second is too complicated and requires a lot of control variables to be designed) 

##  一、clear_geomotry()和update_render() 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573777958
  ```  
##  register_animation_callback callback function 

 Open3D provides a dynamic visualization function, register_animation_callback (), the function can be dynamically displayed by calling the callback function data, we only need to implement the internal functions of the callback function, there is a keyboard event register_key_action_callback () function used in conjunction with it to respond to keyboard operations, first look at a small example to familiarize yourself with the usage of this function 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573777958
  ```  
 After execution, it is found that every time you press the space bar, the point cloud will rotate by an angle. We can also modify the callback function of the keyboard as follows, thus realizing the functions of pause and playback. 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573777958
  ```  
 Ok, through the above small example we use register_animation_callback and register_key_action_callback to achieve our point cloud continuous playback function, while you can pause and play data, you can play the previous frame and the next frame. I only release the key code here, all the code and data uploaded to the network disk together. The first is the callback function for continuous playback animation_callback, as long as the play_status variable is True and not played to the last frame, continue to execute, this execution operation is called register_animation_callback. 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573777958
  ```  
 The callback function that controls pause and playback is mainly to modify the play_status variable in the callback function of continuous playback by receiving the operation of the space bar. Each time the space bar is pressed, it is changed to not play_status, that is, true is changed to false, and false is changed to true. 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573777958
  ```  
 The forward callback function is mainly to increase the data read id by 1, and the id cannot exceed the maximum value. 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573777958
  ```  
 Back callback function, the data read id is reduced by 1, and the id cannot be less than 0. 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573777958
  ```  
 At the same time, we have realized the visualization of box here. Since a box is a geometry object in open3d, the number of boxes in the point cloud of each frame is not fixed, so the method of update_geomotry () cannot be used to accurately update the box. You need to apply for the object of box first, and then constantly overwrite the box object. Apply for 100 lineset objects and add them to vis. 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573777958
  ```  
 When it comes to real visualization, first determine whether the number of boxes is greater than the number of existing lineset_list. If it is greater, you need to expand line_list. 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573777958
  ```  
##  Third, reference and network disk address 

 1, http://www.open3d.org/docs/latest/tutorial/Advanced/customized_visualization.html 2, https://github.com/isl-org/Open3D/blob/master/examples/python/visualization/customized_visualization_key_action.py 3, link: https://pan.baidu.com/s/1TziBSyyo5BtWbVTphERyug extraction code: asvi 

