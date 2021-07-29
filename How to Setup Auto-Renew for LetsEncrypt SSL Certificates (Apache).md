# How to Setup Auto-Renew for LetsEncrypt SSL Certificates (Apache)

Did you know that you can quickly configure your Let’s Encrypt certificates to automatically renew themselves by executing a simple letsencrypt auto-renew script?

> *Configuring auto-renew for you Let’s Encrypt SSL certificates means your website will always have a valid SSL certificate.*

In this beginner tutorial you will learn how to configure your [Let’s Encrypt SSL certificates](https://letsencrypt.org/) to automatically renew themselves prior to their expiration date.

> Before getting started with this tutorial, you should have already configured Let’s Encrypt SSL certificates for an Apache server on Google Cloud compute engine.

If you haven’t yet configured your SSL for your website, here is the tutorial for the [Click-to-deploy](https://onepagezen.com/free-ssl-certificate-wordpress-google-cloud-click-to-deploy/) (standard Apache) and [Bitnami](https://onepagezen.com/free-ssl-certificate-wordpress-google-cloud-bitnami/) (custom Apache) server configurations.

## There are 6 steps in this tutorial:

·     1. [Locate Certbot-Auto Package](https://onepagezen.com/letsencrypt-auto-renew-certbot-apache/#step1)

·     2. [Move Certbot-Auto Package](https://onepagezen.com/letsencrypt-auto-renew-certbot-apache/#step2)

·     3. [Edit Crontab File](https://onepagezen.com/letsencrypt-auto-renew-certbot-apache/#step3)

·     4. [Configure Auto-Renew Script](https://onepagezen.com/letsencrypt-auto-renew-certbot-apache/#step4)

·     5. [Basic Auto-Renew Testing](https://onepagezen.com/letsencrypt-auto-renew-certbot-apache/#step5)

·     6. [Advanced Auto-Renew (Optional)](https://onepagezen.com/letsencrypt-auto-renew-certbot-apache/#step6)

# 1. Locate Certbot-Auto Package

For those of you who configured SSL using the [Click-to-deploy](https://onepagezen.com/free-ssl-certificate-wordpress-google-cloud-click-to-deploy/) and [Bitnami](https://onepagezen.com/free-ssl-certificate-wordpress-google-cloud-bitnami/) SSL tutorials, your certbot-auto package was downloaded to your home directory. You can view the the package by simply executing the ls command.

![certbot auto ls command](E:\AA\clip_image002.jpg)For users who have followed the [Click-to-deploy](https://onepagezen.com/free-ssl-certificate-wordpress-google-cloud-click-to-deploy/) or [Bitnami](https://onepagezen.com/free-ssl-certificate-wordpress-google-cloud-bitnami/) SSL tutorials, you can view your certbot-auto package by executing the ls command. See it? Now proceed to [step 2](https://onepagezen.com/letsencrypt-auto-renew-certbot-apache/#step2).

For those of you who downloaded the certbot-auto package to a different directory, it is important to find it. If you cannot find the certbot-auto package, you can re-download the package by executing the following command:

```
wget https://dl.eff.org/certbot-auto && chmod a+x certbot-auto
```

![execute ls command to view certbot installation](E:\AA\clip_image004.jpg)If you have to re-download the certbot-auto package, you can verify that it has been installed properly by executing the ls command to view the package.

# 2. Move Certbot-Auto Package

After you’ve established the location of your Cerbot-Auto package, the next step is to move the certbot-auto package into the /etc/letsencrypt/ directory.

So, for users who followed either of the above mentioned [Click-to-deploy](https://onepagezen.com/free-ssl-certificate-wordpress-google-cloud-click-to-deploy/) or [Bitnami](https://onepagezen.com/free-ssl-certificate-wordpress-google-cloud-bitnami/) tutorials, your command would be:

```
sudo mv certbot-auto /etc/letsencrypt/
```

![move certbot to letsencrypt directory with mv command](E:\AA\clip_image006.jpg)Use the mv command to move your certbot package into the letsencrypt directory.![moved certbot package](E:\AA\clip_image008.jpg)You can verify that your certbot-auto package has been moved successfully by executing the command ls /etc/letsencrypt/ and seeing if the certbot-auto package appears in that directory.

# 3. Edit Crontab File

Now that you’ve moved your certbot-Auto package to the /etc/letsencrypt/ directory, the next step is to open your crontab file.

To open your crontab file, execute the following command:

```
sudo crontab -e
```

![opening crontab file for editing](E:\AA\clip_image010.jpg)You can open your crontab file by executing the command sudo crontab -e.

# 4. Configure Auto-Renew Script

Now that you’ve opened your crontab file, the next step is to add a script at the bottom of the crontab file which will execute once per week and will automatically renew the SSL certificates if they are about to expire.

![inside of crontab file with ssl auto renew script](E:\AA\clip_image012.jpg)At the bottom of your crontab file, you will enter a script which will tell your server to check for certificate renewals once per week, and to automatically renew the certificates if they are about to expire. To save changes, press CTRL + X, then CTRL + Y, then Enter.

For [Click-to-deploy](https://onepagezen.com/free-ssl-certificate-wordpress-google-cloud-click-to-deploy/) or standard Apache users, add the following script:

```
45 2 * * 6 cd /etc/letsencrypt/ && ./certbot-auto renew && /etc/init.d/apache2 restart
```

For [Bitnami](https://onepagezen.com/free-ssl-certificate-wordpress-google-cloud-bitnami/) users, add the following script:

```
45 2 * * 6 cd /etc/letsencrypt/ && ./certbot-auto renew && /opt/bitnami/ctlscript.sh restart
```

# 5. Basic Auto-Renew Testing

To test your auto-renew script for errors, you can quickly perform a 'dry run' - a process in which the auto-renew script will be executed without actually renewing the certificates. To perform a 'dry run', execute the following two commands:

For [Click-to-deploy](https://onepagezen.com/free-ssl-certificate-wordpress-google-cloud-click-to-deploy/) or standard Apache users:

```
sudo -i
cd /etc/letsencrypt/ && ./certbot-auto renew --dry-run && /etc/init.d/apache2 restart
```

For [Bitnami](https://onepagezen.com/free-ssl-certificate-wordpress-google-cloud-bitnami/) users:

```
sudo -i
cd /etc/letsencrypt/ && ./certbot-auto renew --dry-run && /opt/bitnami/ctlscript.sh restart
```

**Congratulations!** You've successfully configured your Let's Encrypt SSL certificates to automatically renew prior to expiration.

If you would like to test-run the renewal process, continue to the next step (optional). Because the script will renew the certificates one month prior to expiration, you can use a [SSL Checker](https://www.sslshopper.com/ssl-checker.html) to verify whether the certificates have renewed successfully.

# 6. Advanced Auto-Renew Testing

In this advanced testing section of the tutorial you will learn how to use the --force-renew command to simulate certificate renewal in a live environment.

To get started, check the current date and time stamp on your server. To do this, execute the date command.

![executing date command apache server](E:\AA\clip_image014.jpg)Check the current time on your server by executing the date command.

Take note of the date and time - either paste it into Notepad or write it down on a piece of paper. Based on the example above, I would write down 18:56:54

## 6.1  Check current expiry date

Now that you've logged your system's current date and time, the next step is to check when your certificate is currently set to expire. To do that, execute the following commmand:

```
openssl x509 -noout -dates -in /etc/letsencrypt/live/example.com/cert.pem
```

**Note:** Make sure to replace example.com with your own domain name.

![expiry date and time for letsencrypt script](E:\AA\clip_image016.jpg)Checking your SSL certificate expiry date beforehand will allow you to test if the auto-renew script is working properly.

Take note of the date and time when the certificate was issued - either paste it into notepad or write it down on a piece of paper.

Based on the example above, I would write down 13:34:41

## 6.2  Force Crontab script

Execute the command sudo crontab -e to re-open your crontab file.

In this example my virtual machine's date and time stamp showed 18:56:54. So, I would want the auto-renew script to execute a few minutes ahead of 18:56:54 at 18:59:00.

![change renew command cerbot apache crontab](E:\AA\clip_image018.jpg)In order to test the auto-renew script, you will have to temporarily change the script time and renew command. The numbers under the m and h represent the time (minute and hour) when you want the script to execute (18:59:00 in the image above).

For [Click-to-deploy](https://onepagezen.com/free-ssl-certificate-wordpress-google-cloud-click-to-deploy/) or standard Apache users:

```
59 18 * * * cd /etc/letsencrypt/ && ./certbot-auto renew --force-renew && /etc/init.d/apache2 restart
```

For [Bitnami](https://onepagezen.com/free-ssl-certificate-wordpress-google-cloud-bitnami/) users:

```
59 18 * * * cd /etc/letsencrypt/ && ./certbot-auto renew --force-renew && /opt/bitnami/ctlscript.sh restart
```

After the time at the front of the script has passed (18:59 in this example), check your system log to verify that the script has executed successfully.

To check your system log, navigate to your log directory by executing cd /var/log/.

![view system logs apache](E:\AA\clip_image020.jpg)By navigating to /var/log/, you will have access to your Apache system logs.

Next, print your system log to your screen by executing the command cat syslog.

![check system logs apache](E:\AA\clip_image022.jpg)If your test was successful, you will notice the crontab script appear in your Apache system logs, showing whether the script had executed successfully.

## 6.3  Check if renewal was successful

To check if renewal was successful, navigate back to your home directory by executing cd, then execute the following command, making sure to replace example.com with your own domain name.

```
openssl x509 -noout -dates -in /etc/letsencrypt/live/example.com/cert.pem
```

![ssl certificate expiration date after renew](E:\AA\clip_image024.jpg)After executing the certbot auto-renew script, you should notice that your SSL certificate expiration dates have changed, and have moved to 3-months in the future.

It is also a good idea to double-check with an online [SSL certificate checker](https://www.sslshopper.com/ssl-checker.html) to make sure your renewed certificates are being recognized.

## 6.4  Revert crontab script to default

Now that testing is complete, remember to change your crontab script back to the default from step 4 of this tutorial!

![inside of crontab file with ssl auto renew script](E:\AA\clip_image025.jpg)The original crontab script is set to execute every Saturday at 2:45am.

 