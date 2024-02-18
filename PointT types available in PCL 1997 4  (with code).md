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

