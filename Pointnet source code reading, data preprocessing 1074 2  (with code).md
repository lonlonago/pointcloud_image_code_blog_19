 Here, the S3DIS dataset is taken as an example to explain the source code of pointnet. For the basic situation of the S3DIS dataset, please refer to the point cloud public dataset: S3DIS. 

#  Format conversion 

 Execute the following statement 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573785599
  ```  
  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573785599
  ```  
 The core of this script is to call indoor3d_util collect_point_label () function. The code is as follows 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573785599
  ```  
 This statement is to convert the original S3DIS txt data into npy binary format and save it, which can reduce the storage space and speed up the reading of the data later. Each room in each region is saved as a .npy file, and each point has 7 attributes of x, y, z, r, g, b, and label. 

 View an npy file 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573785599
  ```  
 You can see that Area_1 conferenceRoom1 room has a total of 1136617 points. 

 ![avatar]( 9674e3feca7245ecbe026561273a37b8.png) 

 Let's take a look at the original txt file corresponding to this room. Corresponding to Area_1\ conferenceRoom_1\ conferenceRoom_1 .txt, open the file in CloudCompare, you can see that the number of dots in the attribute is 1136617.  

#  Second, sample production 

 When pointnet network training, the data for each room is cut according to a block of 1mx1m, and then 4096 points are selected as 1 line of data, enough to save 1000 lines as a .h5 file, a total of 24 h5 files, the first 23 files are 1000 lines, the last h5 file has 585 lines. There are also two txt files, all_files and room_filelist, all_files .txt records the name of each h5 file, room_filelist Each line in txt is the name of a room in an area, indicating which room the line of data is taken from. If you want to generate samples yourself, you can execute the following script, or you can download the sample dataset made by the author: indoor3d_sem_hdf5_data.zip. 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573785599
  ```  
