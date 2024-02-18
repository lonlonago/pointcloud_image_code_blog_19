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

