Point cloud semantic segmentation: The PointNet training S3DIS dataset records the complete process of training and testing the S3DIS dataset using the pointnet. The testing process of the pointnet is recorded here. The testing process is divided into three parts: parameter setting, session and computational graph creation, and verification of a single room 

#  First, parameter settings 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573794935
  ```  
 The original S3DIS data needs to be saved in npy format in advance, and the npy file path of each room in the test area needs to be saved into a text file first. The corresponding parameter room_data_filelist. 

#  Creation of computational graphs and conversations 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573794935
  ```  
#  III. Verification 

 Pointnet performs individual inference for each room. First, each room is made into sample data and sent to the model. A 1m x 1m point cloud block is selected by sliding, and then 4096 points are selected. 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573794935
  ```  
