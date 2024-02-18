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

