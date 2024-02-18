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

