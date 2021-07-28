# How to do Site redirection using AWS S3?

 

 

**What is site redirection?** 

When we want to redirect a site “www.myolddomain.com” to “www.mynewdomain.com” then it is called site redirection and the URL in the user’s browser will also change with HTTP 301 status code, indicating that “www.myolddomain.com” is permanently moved to “www.mynewdomain.com”. This is usually done incase we want to rebrand our product with a new domain.

**Site redirection can’t be done just using CNAME record at DNS server level, if we just use CNAME record to point “ [www.myolddomain](http://www.myolddomain/).com” to “www.mynewdomain.com” then URL won’t be redirected in the user’s browser and it will still show as “www.myolddomain.com” with the content from “www.mynewdomain.com”.**

***AWS S3 is famous for [static web hosting ](https://medium.com/tensult/host-a-static-website-2129275f145f)and it also supports Site redirection so we don’t have to run any Nginx or Apache servers to handle site redirections.***

> Let us see how to do this using S3 and Route 53

1. We need to create bucket with name “www.myolddomain.com” (replace myolddomain to your domain and select region appropriately)

 

![https://miro.medium.com/max/700/1*DA1mSovXbYU0T1bCWh8_9Q.png](C:\Users\Aditya Mishra\Downloads\clip_image002.jpg)

​                                                       Create S3 bucket using AWS S3 console

2. **We need to step site redirection using “Static web hosting” feature of S3**

 

![https://miro.medium.com/max/700/1*2Z_MEmiFx47LhUZ8-Tg3Jg.png](C:\Users\Aditya Mishra\Downloads\clip_image004.jpg)

​                                                         Select bucket and Click on Properties

 

![https://miro.medium.com/max/700/1*0TfbzOyf_KPYxHWiNGcdkw.png](C:\Users\Aditya Mishra\Downloads\clip_image006.jpg)

​                                                            Click on Static website hosting

 

![https://miro.medium.com/max/550/1*Y1e8WXvWw8SNJCr8sVpJ-w.png](C:\Users\Aditya Mishra\Downloads\clip_image008.jpg)

​                                    Set Redirection to new domain and select protocol appropriately

 

 

 

 

3. **We can now test the redirection by clicking the link shown.**

 

![https://miro.medium.com/max/555/1*RI8T6jjlAIP1yLLYz1EHbw.png](C:\Users\Aditya Mishra\Downloads\clip_image010.jpg)

​                                                 Click on the link marked to test redirection



4. **[Route 53](https://aws.amazon.com/route53/) is an AWS DNS service which is guaranteed to be available 100% of the time and if you are using Route 53 for your domain then do the following, else you can set the above URL as CNAME in your domain using your domain registerer.**

 

![https://miro.medium.com/max/700/1*IF04tMFAMCt7OoMZt8RCQQ.png](C:\Users\Aditya Mishra\Downloads\clip_image012.jpg)

​                                                        From Route 53 Console click **Create Hosted Zone**

 

 

![https://miro.medium.com/max/700/1*S6926c8EqoiY62OqdX2yFA.png](C:\Users\Aditya Mishra\Downloads\clip_image014.jpg)

​                                                        Enter your old domain name and click **Create**

5. **We need to create a DNS record for hosted zone on Route 53 for setting up redirection.**

 

![https://miro.medium.com/max/700/1*CLu-uKXriV-nUO2zARHwYA.png](C:\Users\Aditya Mishra\Downloads\clip_image016.jpg)

​                                Select the appropriate Hosted Zone and click create **Create Record Set**



6. **We need to create an A record for “www.myolddomain.com” for using Alias to point to S3 bucket which we created earlier.**

 

![https://miro.medium.com/max/410/1*Po7DdylB1c5sLY9ij4URHQ.png](C:\Users\Aditya Mishra\Downloads\clip_image018.gif)

​                                             Select S3 Website Endpoint and click **Create**



7. **We also need to create an A record for myolddomain.com, as some users might be typing http://myolddomain.com. So redirection should apply for this address too.**

 

![https://miro.medium.com/max/427/1*cmPNPIYW-byNE6E7FnuYhw.png](C:\Users\Aditya Mishra\Downloads\clip_image020.jpg)

​                                      Select www.myolddomain.com record set and click **Create**

 

![https://miro.medium.com/max/700/1*1dhO2Dfup1auEDwjewXSBw.png](C:\Users\Aditya Mishra\Downloads\clip_image022.jpg)

​                                                      All myolddomain.com Route53 DNS records



8. ### Now you can test by going to [http://myolddomain.com](http://myolddomain.com/) and [http://www.myolddomain.com](http://myolddomain.com/), it should redirect to [http://www.mynewdomain.com](http://www.mynewdomain.com/)

***What about HTTPS site URL redirection?***

If you have noticed [https://www.myolddomain.com](http://www.mynewdomain.com/) won’t be redirecting to the new domain. Above process is sufficient if the site: myolddomain.com only supports HTTP but if we have to support HTTPS site URL redirection then we need to add two more component: [CloudFront](https://medium.com/tensult/introduction-to-aws-cloudfront-eaf5d2d780a6) and [ACM](https://aws.amazon.com/certificate-manager/) (AWS Certificate Manager).

## S3 static web hosting doesn’t support HTTPS so in order to support HTTPS, we need to use CloudFront.

Create CloudFront distribution with S3 origin: [www.myolddomain.com.s3-website.ap-south-1.amazonaws.com](http://www.myolddomain.com.s3-website.ap-south-1.amazonaws.com/) (S3 static website URL which we have configured earlier using S3 console).

 

![https://miro.medium.com/max/555/1*RI8T6jjlAIP1yLLYz1EHbw.png](C:\Users\Aditya Mishra\Downloads\clip_image023.jpg)

​                                                               Use this URL as origin in CloudFront



Please don’t follow the suggestion by CloudFront to choose S3 bucket URL while creating the origin. If you follow the suggestion, URL of the origin becomes: [www.myolddomain.com.s3.amazonaws.com](http://www.myolddomain.s3.amazonaws.com/) and site redirection won’t work properly with this origin URL. Also set Alternative domain names (CNAMEs) to www.myolddomain.com otherwise Route53 DNS forwarding doesn’t work.

CloudFront will ask for a SSL certificate as it is need for supporting HTTPS traffic so first we need to create a new or import an existing certificate to ACM then use it for CloudFront distribution. Please note that while selecting the SSL certificate, make sure it is associated with myolddomain.com otherwise you will get certificate errors; also make sure that ACM certificate is created in North Virginia (us-east-1) region as CloudFront allows certificates from that region only.

Once CloudFront distribution is created and then wait for it be deployed then update an **A record** in Route 53 for “www.myolddomain.com” with distribution domain name.

Now test HTTPS redirection by going to [https://www.myolddomain.com](https://www.myolddomain.com/).

***This procedure only works for a single site redirection***

 