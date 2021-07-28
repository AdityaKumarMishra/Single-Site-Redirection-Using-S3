### How to use AWS Application Load Balancer to setup Multi-Site redirections?

 

**What is an Application Load Balancer?**

Application load balancer is a Layer 7 load balancer which allows us to specify set of rules. Based on those rules it can either redirect or forward the incoming request to respective destination.

**Steps**

Create an AWS Application Load Balancer by going to EC2 management console.

![img](file:///C:\Users\ADITYA~2\AppData\Local\Temp\msohtmlclip1\01\clip_image002.jpg)

​                                                 Select Load Balancers from EC2 Console 



 

![img](file:///C:\Users\ADITYA~2\AppData\Local\Temp\msohtmlclip1\01\clip_image004.jpg)

​                                                                      Click on Create Load Balancer

 

![img](file:///C:\Users\ADITYA~2\AppData\Local\Temp\msohtmlclip1\01\clip_image006.jpg)

​                                                 Click on Create in Application Load Balancer panel



 

![img](file:///C:\Users\ADITYA~2\AppData\Local\Temp\msohtmlclip1\01\clip_image008.jpg)

​                                                            Choose internet-facing

  

 

You want HTTPS site redirection also then you can add HTTPS listener by clicking on Add listener. You also need to have an ACM certificate in the same region else you can’t create ALB with HTTPS listener.

 

![img](file:///C:\Users\ADITYA~2\AppData\Local\Temp\msohtmlclip1\01\clip_image010.jpg)

​                                       Only required incase you want to redirect a HTTPS website

 

![img](file:///C:\Users\ADITYA~2\AppData\Local\Temp\msohtmlclip1\01\clip_image012.jpg)

​                                                               Select at least 2 subnets





![img](file:///C:\Users\ADITYA~2\AppData\Local\Temp\msohtmlclip1\01\clip_image014.jpg)

​                                         Select a certificate matching to the site domain names

 



You can create single certificate with multiple site domains from the AWS console. But if you use AWS CLI or API you can create and attach multiple certificates to HTTPS listener of the ALB . Please note: there is no option to attach multiple certificates to an ALB listener in AWS console.

I have chosen default VPC here but you can use a custom VPC also but make sure that it has at lease 2 subnets.



![img](file:///C:\Users\ADITYA~2\AppData\Local\Temp\msohtmlclip1\01\clip_image016.jpg)

​                                                     Click on Create a new security group

 

If you want HTTPS site redirection also then you can add HTTPS port: 443 whitelisting by clicking on Add Rule.

![img](file:///C:\Users\ADITYA~2\AppData\Local\Temp\msohtmlclip1\01\clip_image018.jpg)

​                                      Only required incase you want to redirect a HTTPS website



 

![img](file:///C:\Users\ADITYA~2\AppData\Local\Temp\msohtmlclip1\01\clip_image020.jpg)

​                                                                   Create a Target and Click Next



 

​                            We won’t be using it but we need to give at least one target to create an ALB.

![img](file:///C:\Users\ADITYA~2\AppData\Local\Temp\msohtmlclip1\01\clip_image022.jpg)

​                                                                 Click on Next to Review

 

We don’t need to register any instances to the target for site redirection. So go to the Review page and then                            Click on Create.

![img](file:///C:\Users\ADITYA~2\AppData\Local\Temp\msohtmlclip1\01\clip_image024.jpg)

​                                      Navigate to Listeners Tab and Click on View/edit rules



 

![img](file:///C:\Users\ADITYA~2\AppData\Local\Temp\msohtmlclip1\01\clip_image026.jpg)

​                                                      Click on Insert Rule

 

​                                    Click on + icon next Rules (top-left) then click on Insert Rule.

![img](file:///C:\Users\ADITYA~2\AppData\Local\Temp\msohtmlclip1\01\clip_image028.jpg)

​                                       Click on Add condition and then on Host is…

 

![img](file:///C:\Users\ADITYA~2\AppData\Local\Temp\msohtmlclip1\01\clip_image030.jpg)

​                              Enter your old domain in IF section and select Redirect to… in THEN section

 

![img](file:///C:\Users\ADITYA~2\AppData\Local\Temp\msohtmlclip1\01\clip_image032.jpg)

​                                              Enter details of the new domain in THEN section

 



Here we are setting up redirection for [http://www.myolddomain.com](http://www.myolddomain.com/) to [https://www.mynewdomain.com](https://www.mynewdomain.com/), if you want setup direction to a HTTP site then change it appropriately.

We can also use the above procedure for following commonly used redirects:

[http://mynewdomain.com](http://mynewdomain.com/) to [http://www.mynewdomain.com](http://www.mynewdomain.com/) or [https://www.mynewdomain.com](https://www.mynewdomain.com/)

[http://www.mynewdomain.com](http://www.mynewdomain.com/) to [https://www.mynewdomain.com](https://www.mynewdomain.com/)

 

![img](file:///C:\Users\ADITYA~2\AppData\Local\Temp\msohtmlclip1\01\clip_image034.jpg)

​                                                                  Path based redirection

  

We can also redirect particular path of one domain to other using Path condition as show in the above picture.

You can follow above steps to add more site redirection rules to HTTP listener.

 



![img](file:///C:\Users\ADITYA~2\AppData\Local\Temp\msohtmlclip1\01\clip_image036.jpg)

​                            Click on HTTP : 443-View/edit rules to add HTTPS site redirections



 

To add HTTPS site redirection rules, click on View/edit rules for HTTP : 443 (HTTPS) listener in ALB Listeners tab and follow the same steps as HTTP listener.

 

![img](file:///C:\Users\ADITYA~2\AppData\Local\Temp\msohtmlclip1\01\clip_image037.jpg)

​                                     Use CNAME of the ALB to configure your site(s) DNS settings

 

Use the DNS (CNAME) of the ALB to configure the DNS settings of the sites for which you want the redirection to happen.

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 