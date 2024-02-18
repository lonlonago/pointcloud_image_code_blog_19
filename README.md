 Disclaimer: My dataset is point cloud data acquired by the drone, similar to Semantic3D, so the following is modified based on Semantic3D's processing. 

 In fact, using RandLANet to train your own dataset mainly modifies the data preprocessing part and the dataset part. 

#  First, data preprocessing 

##  1. Data format 

 My own point cloud dataset is in TXT format, with each row representing a point and each row having 7 columns, each of which is x, y, z, r, g, b, lable. 

##  2. Data preprocessing 

 Our own datasets are somewhat different from public datasets in terms of data storage. For example, the labels of my dataset are stored in the last column of the point cloud file, while the labels of Semantic3D are stored separately in a .label file with the same name as the point cloud file. Below is the preprocessing script I modified according to my dataset format. 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573753036
  ```  
 You can see that data preprocessing is still relatively easy to modify. You only need to know your own data set storage format, load it correctly, and input the correct coordinates, colors, and labels when downsampling. For details, please refer to the tf1.x version of RandLA-Net source code interpretation (1): Data preprocessing. 

#  II. Modification of the dataset 

 What needs to be modified in the dataset part is the partitioning and loading of the dataset. Just modify the initialization part. 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573753036
  ```  
 For the detailed process of datase, please refer to the RandLA-Net source code interpretation of tf1.x version (2): Dataset 

 Training our own dataset also requires modifying the weights of the corresponding dataset help_tool.py: 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573753036
  ```  
 get_class_weights is based on the name of the dataset to obtain the weight, corresponding to my dataset power, need to calculate the sum of the number of points in my dataset, my dataset has a total of 5 categories, set the corresponding data num_per_class. If you don't want to use weighting, you can directly set the celabel_weight to an array of all 1. 

#  III. Training and testing 

 This part of the process is the same as for Semantic3D data. You can refer to the link given at the beginning of this article. 



--------------------------------------------------------------------------------

#  I. Environment 

 Project address: SensatUrban 

#  II. Preparations 

##  2.1. Compilation 

 Compiling Nearest Neighbor Search and Downsampling Modules 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573774496
  ```  
##  2.2. Data preprocessing 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573774496
  ```  
 data structure collation 



--------------------------------------------------------------------------------

#  Text (txt) 

##  1.1. Storage structure 

 The file structure of point cloud data stored in text format is relatively simple, each point is a row of records, and the information storage format of the points is x y z or x y z r g b. 

##  1.2. Reading 

 When reading point cloud data in text format, you can follow the general text reading method. Here is a record of how to use open3d to read point cloud data in txt format 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573741758
  ```  
#  Second, PCD format 

 A PCD file is composed of a file header and a data part 

##  1.1. Storage structure 

##  1.2. Reading and writing 

###  1.2.1, open3d read and write (python) 

 Read pcd point cloud file 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573741758
  ```  
 Save pcd point cloud file 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573741758
  ```  
 ![avatar]( 20210716093718382.png) 

 ![avatar]( 2021071609374197.png) 

 The PCD point dataset is partially saved in binary format, where RGB is written in bit storage. 

 ![avatar]( 20210716093751407.png) 

###  1.2.2, PCL read and write (C++) 

 We use the pcd file generated above for testing 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573741758
  ```  
 Print result 

 ![avatar]( 20210716141615420.png) 

 Save pcd file 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573741758
  ```  
 Open the output ascii file 

 ![avatar]( 20210716152854533.png) 

 It can be seen that the pcd point cloud file generated by pcl is inconsistent with the point cloud file generated by open3d in the color part, mainly due to the data storage type. After pcl 1.7, the color part will be saved as an integer. See here for details. 

#  III. LAS format 

##  3.1. Storage structure 

 LAS is a public data format for storing point cloud, which is mainly used to store radar point cloud data. LAZ is a lossless compression of LAS format. 

 The latest LAS specification version is LAS 1.4. The laspy library supports LAS files from 1.2 to 1.4. 

 A LAS file consists of three parts: the file header area (Header), the variable length recording area (VLRs), and the point set recording area (Point Records). 

 The file header contains information such as the data version, the format of the point (different dimensions stored by each point), etc.; the variable-length record includes some meta-information, such as coordinate system information, additional dimensions, etc.; the point set record area is the core of the las file, recording the x, y, z coordinate information of the point and other attribute information such as r, g, b, classification, intensity, etc. There are 11 versions of the point format, of which version 0 is the basis, and other subsequent versions add other fields on the basis of version 0. For specific fields, please refer to here. 

##  3.2. Reading and writing 

###  3.2.1, use laspy to read and write (Python) 

 In python, you can use the laspy package to read and write las point cloud files 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573741758
  ```  
 Write las files with laspy 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573741758
  ```  
###  3.2.2, use laslib to read and write (C++) 

 The laslib library can be used to manipulate las files with C++. 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573741758
  ```  
 Write to las file 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573741758
  ```  
#  IV. PLY format 

##  4.1. Storage structure 

 PLY (Polygon File Format) is a common point cloud storage format developed by Stanford University. It was originally used to store point cloud data from 3D scanners. 

 PLY file consists of two parts: file header and data area. 

###  File header 

 The file header records the comments, element categories, and attributes in the point cloud file, starting with ply and ending with end header. 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573741758
  ```  
 The second line of the file header is the storage method and version of the file, starting with format, followed by encoding method and version. There are three encoding methods: ascii, binary_little_endian, binary_big_endian. Currently PLY only has version 1.0. 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573741758
  ```  
 Format followed by comment information, starting with comment, the author can add some author information, point cloud basic information. 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573741758
  ```  
 The comment information is followed by element element information + property attribute information of the element. The element information includes the type and number of elements, and the property attribute information includes the storage type and attribute name of the attribute field. Elements in a PLY file generally include vertices, faces, edges, etc. Element information and attribute information should be combined in the following format 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573741758
  ```  
 We define a ply file with 6 vertices and 8 face elements. The file header is as follows. 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573741758
  ```  
###  data range 

 Start storing data directly after the file header, and the storage form is divided into ASCII and binary. Taking ASCII as an example, record each point by line first, and then record each face by line after all point records are completed. 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573741758
  ```  
 So the complete ply file is as follows 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573741758
  ```  
##  4.2. Reading and writing 

###  4.2.1, using plyfile to read and write (Python) 

 Use plyfile to read ply files 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573741758
  ```  
 Print result 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573741758
  ```  
 Write ply file using plyfile 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573741758
  ```  
 Open in meshlab 

 ![avatar]( 88893a4d08534f3f8df1334c3d6a120e.png) 

 ![avatar]( f84a4b402ebe4f39acc514cafbdd78bc.png) 

###  4.2.2, use PCL to read and write (C++) 

 Read ply point cloud file 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573741758
  ```  
 Save ply point cloud file 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573741758
  ```  


--------------------------------------------------------------------------------

#  Point cloud visualization - (1) Two methods of point cloud visualization 

###  article directory 

 Preface, one, two, three, follow-up 

###  foreword 

 PCL official document: pcl English official document 

 Recently started to learn point cloud, refer to the official documents and books "point cloud library PCL from entry to proficiency", which point cloud mainly includes the following modules: filter, feature, keypoints, registration, kdtree, octree, segmentation, sample_consensus, surface, recognition, io, visualization, etc. This article mainly talks about the two easiest operations of point cloud visualization, I hope to record my learning process here. 

###  One. 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573779942
  ```  
 result graph 

 ![avatar]( 20190508104812577.png) 

###  II. 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573779942
  ```  
 ![avatar]( 20190508104545793.png) 

 result graph  

###  III. Follow-up 

 Continuing to learn, I hope to make progress. 



--------------------------------------------------------------------------------

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



--------------------------------------------------------------------------------

#  一、placeholder_inputs 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573747395
  ```  
#  二、get_loss 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573747395
  ```  
#  三、get_model 

 The network structure of pointnet is very simple, which is a stack of MLPs. 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573747395
  ```  


--------------------------------------------------------------------------------

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


--------------------------------------------------------------------------------

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


--------------------------------------------------------------------------------

 Here, the S3DIS dataset is mainly used as an example to explain the source code of pointnet. Blog point cloud semantic segmentation: PointNet training S3DIS dataset records how to use pointnet to train S3DIS models. Just execute train.py directly. Here we record each step of train.py. It can be roughly divided into parameter setting, data loading, training parameter setting, single-round training, and single-round verification. 

#  First, parameter settings 

 The parameter settings mainly specify which region the test set is: - - test_area, and then some hyperparameters of model training, such as batch_size, learning rate, decay rate, optimizer, etc 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573773579
  ```  
#  Data loading training 

 All the processed training data in H5 format is loaded into memory, and then the training dataset and test set are divided according to the test_area. 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573773579
  ```  
#  Training parameter settings (sessions, models, operations, loss functions, optimizers, etc.) 

 This step is to create a session, acquire the model and the operation op of the loss, and set the training parameters such as the optimizer and learning rate according to the parameters. 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573773579
  ```  
#  IV. Single round of training 

 Scrambling the data, batch by batch of training data, the core code is sess.run () this sentence, 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573773579
  ```  
#  V. Single round verification 

 As with training types, both perform inference operations, but do not require layers. 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573773579
  ```  


--------------------------------------------------------------------------------

 PointXYZI - member variables: float x, y, z, intensity 

 PointXYZI is a simple X Y Z coordinate plus intensity point type, which is a separate structure and satisfies storage alignment, since most operations on point will set the data [4] element to 0 or 1 (for transformation), 

 You can't put intensity and XYZ in the same structure. If so, its content will be overwritten. For example, the dot product of two points will set the fourth element to 0. Otherwise, the dot product has no meaning. 

  PointXYZRGBA - member variables: float x, y, z; uint32_t rgba is similar to PointXYZI except that the RGBA information is contained in an integer variable 

 PointXYZRGB - float x, y, z, rgb Except that the RGB information is contained in a floating-point data variable, the other and PointXYZRGBA 

 PointXY - member variables: float x, y Simple 2D x-y structure code 

 struct{

float x;

float y;

}; 

 InterestPoint - member variables: float x, y, z, strength In addition to strength, which represents the strength measurement of the keypoint, the rest and PointXYZI 

 Normal - member variables: float normal [3], curvature; 

 Another commonly used data type, the Normal structure represents the normal direction on the sample surface where the given point is located, and the corresponding curvature measurement, for example, the first coordinate of the normal vector can be accessed by points [i]. data_n [0] or points [i]. normal [0] or points [i]. 

 PointNormal - member variables: float x, y, z; float normal [3], curvature; PointNormal is a point structure that stores XYZ data and includes the normal and curvature of the sampled points 

  To be continued ***************************************88888888888 

 Note: Regarding the learning of point cloud library PCL, you can scan the QR code to follow the official account. If you are interested, you can directly reply to the official account to communicate with me and learn from each other. 

 ![avatar]( 976394-20170228174800298-1557261569.jpg) 



--------------------------------------------------------------------------------

#  First, the principle of FPS 

 Farthest Point Sampling (FPS) is a common method in point cloud sampling, which is used to select M (M < N) points from N points. The specific steps are as follows: Assume that the original point set is, there are N points in A, the sampled point set is, and the initial state B is empty. You need to select M points from the point set to the point set. The steps are as follows: 

#  Second, pytorch implementation 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573730484
  ```  
#  reference 

 1. Analysis of the core ideas of Farthest Point Sampling (FPS) algorithm 2. pointnet2_utils.py  



--------------------------------------------------------------------------------

![avatar]( 20200508213416433.png) 

 This is a deep learning framework for 3D point clouds, providing a general deep learning model of common point cloud analysis methods. It relies primarily on Pytorch Geometric and Facebook Hydra. The framework is capable of building lean and complex models with minimal cost and great reproducibility. The goal is to build a tool for benchmarking SOTA models while allowing researchers to study point cloud analysis efficiently, with the ultimate goal of building models that can be applied to practical applications. The code has been open-sourced https://github.com/nicolas-chaulet/torch-points3d (and seems to have been updated recently) engineering structure  

 As a function library, it necessarily provides some common deep learning algorithms and interfaces, and divides models and datasets by task. Supports segmentation, classification, and registration. Supported datasets 

 ![avatar]( 20200508213433841.gif) 

  Segmented dataset: 

 Classified datasets: 

 Registered datasets: 

 dependency 

 Articles related to deep learning that have been implemented 

 If you are interested in this article, please click "Original Reading" to get the QR code of Knowledge Planet. Be sure to join the free Knowledge Planet according to the remarks of "Name + School/Company + Research Direction", download the pdf document for free, and communicate with more friends who love to share! 

 ![avatar]( 20200508213509312.png) 

 Scan QR code  

 Let's share and learn together! Looking forward to friends who have ideas and are willing to share joining the free planet to inject fresh vitality of love and sharing. The topics shared include but are not limited to 3D vision, point clouds, high-precision maps, autonomous driving, and robotics and other related fields. 

 Sharing and cooperation methods: You can contact the group owner WeChat "920177957" (note required) Contact email: dianyunpcl@163.com, enterprises are welcome to contact the official account for cooperation. 



--------------------------------------------------------------------------------

#  foreword 

  1. Question: In the "QT PCL" column, many people have asked to add tree controls to the display interface, and the effect is as follows: 

  2. Solution: Take the above source code as an example and add tree controls. 

 3. Main functions: 

  4. Result: Final source code: 



--------------------------------------------------------------------------------

#  I. Introduction 

  1. The problem 

 ![avatar]( f19fc9bd652c4c749a03acfbc2b97728.png) 

  2. Solution 

 Take the addition of the tree control source code as an example, and add simple filtering operations and registration operations. The final effect is such as the effect display part. 3. Main functions 

  4. Final result 

 Final source code download 

#  Effect display 

 ![avatar]( 459c038c0e25420aad040845b92d3d04.gif) 

#  III. Interface layout 

 Reference: QT PCL Voxel Filtering 

 QT PCL point cloud registration 

#  IV. Code 

 The code part only has two points: 1. Select point cloud operation 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573736207
  ```  
 2. Registration - how to select multiple items 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 2024020309573736207
  ```  
 ![avatar]( 2b8199fe510448a0b6b03972aa182aaf.png) 

 over!!! 



--------------------------------------------------------------------------------

#  foreword 

 Function: Receive point cloud data transmitted by UDP and display it in real time 

#  First, the effect 

 ![avatar]( 3ed97a85c3cc47539c8c5c7a182352b0.gif) 

#  Code 

 Client side: send data 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 202402030957375073
  ```  
 Server level: receiving data 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 202402030957375073
  ```  
 Where: readData () is the code displayed in real time 

#  III. Issues 

 Since the udp connection of qt causes packet loss all the time, add this on the sending side, Udp_Client - > waitForReadyRead (1); 

 ![avatar]( a8ef9bb57ffc4d3f8cd32511ceb0d71c.jpeg) 

 But the follow-up transmission has become so slow, a headache!!!! Let's think of a way later!  

 over！！！！！！ 



--------------------------------------------------------------------------------

 Pointnet is the first network to do point cloud deep learning directly based on points. The network as a whole is relatively simple, and we mainly use Pointnet's semantic segmentation network to explain it here. 

#  First, point cloud characteristics 

#  Network structure 

 ![avatar]( 70a87b490ff94b7289bbb045de7f9367.png) 

 According to the characteristics of the point cloud introduced above, the author designs a pointnet network. The network structure diagram is as follows: The overall structure of the pointnet network is relatively simple, mainly including three operations: T-Net, MLP, and MaxPool. The functions of each operation are as follows: 

 T-Net: is a transformation matrix, including the transformation of point coordinates (input transform) and the transformation of features (feature transform). Its function is to align the coordinates and features of points and solve the rotation invariance of point clouds. We will explain it in the third section. 

 MLP: MLP is used for feature learning to continuously add features to the point cloud through shared MLP. 

 MaxPool: used to aggregate point cloud features, where maximum pooling is the function of symmetric functions, which can solve the disorder of input points. 

 In the semantic segmentation network, pointnet also adds a local and global feature aggregation operation, which replicates the maximum pooling feature N (number of points) times, and then stacks the features of the transformed points. 

#  III. Understanding of T-Net 

 After some geometric transformations (such as rigid transformations) are performed on the point cloud, the semantic labels of the point cloud are invariant. The author hopes that the feature representations learned by the point set are invariant to these transformations. 

 A straightforward approach is to align the input points into a canonical space before feature extraction. In this paper, Spatial Transformer Networks proposes methods for aligning 2D images through sampling and interpolation. 

 The section on the Spatial Transformer is from the translation in Reference 1 

>  Shown in the figure below are the various inputs and corresponding outputs of a spatial converter (ST). It can be seen that ST provides pose normalization for the inputs of other rotations. Using this type of pose normalization in a digital classifier will relax the constraints of downstream algorithms and reduce the degree of data enhancement required. Pose normalization is also beneficial in the case of point clouds, as objects can similarly assume an infinite number of poses. 

 It is mentioned here that ST provides pose normalization for the input. What does it mean? Let's look at the first number 7 on the left. No matter how the input 7 is rotated, the output after ST is a relatively stable shape of the number 7. This is the alignment operation mentioned above. Through the ST operation, no matter how the input number is transformed, it can be well recognized. In fact, it is to improve the network's ability to generalize the input. 

 Still from the translation in reference 1 

>  The following diagram shows the composition of the space converter (ST). Based on the input U, through a small localisation net, the transformation parameter theta is output. To construct the output V for a given U and theta, a grid generator and a sampler are used. The grid generator is equivalent to rotating the input U by theta, such as rotating the written "7" by an angle theta, and then using the sampler to sample the generated rotated image. 

 Let's explain the operation of ST again. It learns an angle through the positioning network, and then uses the mesh generator to rotate the input to obtain a rotated image, because the size of the rotated image will be transformed. Finally, it is resampled to obtain a normalized image. 

 Translate again 

>  Returning to PointNet, a similar approach can be taken: For a given input point cloud, apply an appropriate rigid or affine transformation to achieve pose normalization. Because each of the n input points is represented as a vector and independently mapped to the embedding space, applying a geometric transformation is simply equivalent to multiplying each point with a transformation matrix. Unlike image-based spatial converter applications, no sampling is required. The following figure shows the structure of the input transformation. Similar to the positioning network in ST, T-Net is a regression network whose task is to predict a 3 × 3 transformation matrix dependent on the input, and then multiply the matrix with the n × 3 input. 

 ![avatar]( e659f86af25146cca2e961147a9cc506.png) 

 The network structure of T-Net is shown in the following figure (the picture is from Reference 1): The network structure adopted by T-Net is itself similar to a large network, composed of basic modules of point-independent feature extraction, maximum pooling, and a fully connected layer.  

 Finally, the author shows that the same idea can be extended to the alignment of the feature space by inserting another alignment network on the point features and predicting a feature transformation matrix to align features from different input point clouds. However, the transformation matrix in the feature space has a much higher dimension than the spatial transformation matrix, which greatly increases the difficulty of optimization. Therefore, the author adds a regularization term for the training loss, constraining the feature transformation matrix to be close to the orthogonal matrix.  

#  reference 

 1、An In-Depth Look at PointNet 2、Spatial Transformer Networks 



--------------------------------------------------------------------------------

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



--------------------------------------------------------------------------------

#  effect realization 

 ![avatar]( 5b73606bd1594efba630871ab4798178.png) 

 Raw grid data, segmented grid data  

#  Project address 

 GitHub reference link 

 Paper address: seg-mat 

#  Reproduction process 

 ![avatar]( 9c671643eea54c05bfebf6feadc4b31e.png) 

  1. Download the source code, unzip it, and create a new build folder. 2. Cmake-gui, set the cgal path. 3. VS can be run. Note: Command parameters:  

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 202402030957379390
  ```  
 Compile and run. 



--------------------------------------------------------------------------------

#  foreword 

 I took a look at the hole filling algorithm for point clouds (meshes) these days, and implemented it here. 

 The following code is based on C++ implementation, with the presentation section replaced by MeshLab. 

#  data effect comparison 

 The first set of data 

##  Raw grid data, there are holes 

 ![avatar]( 9c4ad65e6ea148d19ec3f44f6b33eefb.gif) 

 ![avatar]( 1b9da0e600c045a88891cb83003164ff.png) 

##  Method 1. Repair effect of hole filling based on RBF 

 ![avatar]( ef7735f3badb4c018ab509b917a813bf.gif) 

 ![avatar]( e0eea2de4ec8433cb4b62841d2f5d051.png) 

##  Method 2. Reference papers: 

>  Smoothly Filling Holes in 3D meshes using Variational Calculus and Surface Fairing 

 Reference link: Hole filling reference paper 

 ![avatar]( 81178d36925444fe93fff983ae820859.gif) 

 ![avatar]( f0bb977efa5749a9bc76a3f7519e6c68.png) 

##  Method 3 

 ![avatar]( fc7c246eda7a4036a5dc8fadcb3883a3.gif) 

 ![avatar]( b4670647f9ad4df0a562b75a0c6db043.png) 

##  Method 4 

 ![avatar]( 6bb9ba76923249d5b326fff289c875fb.gif) 

 ![avatar]( 10697b27fb3d44a996d44bc8b2070024.png) 

 The second set of data 

##  Raw grid data, there are holes 

 ![avatar]( 283454e29fa84f9ca79708836bc7cadf.png) 

##  Method 1. Repair effect of hole filling based on RBF 

 ![avatar]( ab46fac742144dd7b579aff81ba5ec38.gif) 

##  Method 2 

 ![avatar]( 025b13a1d56347c7af9d667a8e8457c1.gif) 

##  Method 3 

 ![avatar]( d56049a2d8d447b8839fe379cbc91219.gif) 

##  Method 4 

 ![avatar]( e47a9301a4994b88a34c020815f3c998.gif) 

##  compare 

 It can be observed that method 1 (based on RBF) and method 4 are effective. 

##  follow-up 

 The c ++ code of the above 4 methods will be uploaded to the resource in the future, over!!! 



--------------------------------------------------------------------------------

1. Use geodesy library 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 202402030957374413
  ```  
 2. Using GeographicLib 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 202402030957374413
  ```  
 3. Use the GeographicLib header file: 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 202402030957374413
  ```  
 Source file: 

  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 202402030957374413
  ```  
  ```python  
After clicking on the GitHub Sponsor button above, you will obtain access permissions to my private code repository ( https://github.com/slowlon/my_code_bar ) to view this blog code. By searching the code number of this blog, you can find the code you need, code number is: 202402030957374413
  ```  
 Test results: enu2: -21.55637298376162, -1.775981625076383 enu3: -21.58742491883459, -1.351323097478598 enu1: -21.61596268927678, -1.354094658978283 

 enu2:-34.56130186195978,-2.79318039258942 enu3:-34.61002051952528,-2.112335940822959 enu1:-34.64409087330569,-2.11610805336386 

 enu2:-86.73813263529708,-7.122480107471347 enu3:-86.86262539529707,-5.41375290369615 enu1:-86.89391857566079,-5.416469991672784 

 It can be found that the processing results of mode 1 and mode 3 are basically the same, but compared with mode 2, the y value will have a large deviation, about 2 to 3 meters. 



--------------------------------------------------------------------------------

