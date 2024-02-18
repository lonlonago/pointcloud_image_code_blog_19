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

