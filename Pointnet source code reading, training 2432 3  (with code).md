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
