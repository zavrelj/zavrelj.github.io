---
layout: post
title:  "SharePoint on Windows Server 2012"
date:   2015-05-20 22:21:20
tags: sharepoint windows
---

Today we will leave Linux world and jump right into Windows. Let's install SharePoint 2013 (**SP**). It's quite easy and for free. Register to [Dreamspark](https://www.dreamspark.com) service where you can evaluate all sorts of Microsoft's products for 180 days. Download ISO of Windows Server 2012 Datacenter (Standard will work as well) and install it as a virtual machine (**VM**) using VirtualBox or Parallels Desktop or VMware Fusion (google it, if you hear it for the first time). Download [Sharepoint Foundation 2013](https://www.microsoft.com/en-us/download/details.aspx?id=35488).

Ready? Let's start!

First thing we want to do after Windows Server is installed is to change the name of our server.
This is easily done via Server Manager where you click on ComputerName in **Configure this local server**.

{% include image.html img="assets/sharepoint-on-windows-server-2012/sp_01.png" title="" caption="" %}

{% include image.html img="assets/sharepoint-on-windows-server-2012/sp_02.png" title="" caption="" %}

Doing it later may give you a pretty hefty headache, as in my case where I decided to change the name while installing Active Directory (**AD**) with **SP** with built-in database already setup.

So change the name to **WS2012** and leave default Workgroup for now. We will change it later while installing **AD**.

Restart **VM**.

To install **SP** double click installer and hit **Install software prerequisites** link.

{% include image.html img="assets/sharepoint-on-windows-server-2012/sp_03.png" title="" caption="" %}

Sit back, wait and restart. This will automatically configure Internet Information Services (**IIS**) and other stuff.

After second restart we are ready to install **SP**. So run SharePoint installer again and hit **Install SharePoint Foundation**.

Choose stand-alone version.

{% include image.html img="assets/sharepoint-on-windows-server-2012/sp_04.png" title="" caption="" %}

When the installation is successfully completed, run SP Configuration Wizard as suggested (just leave the option ticked and click Next). It will probably end with failure, but it will also populate folders we need to setup access rights, so we will run it anyway.

{% include image.html img="assets/sharepoint-on-windows-server-2012/sp_05.png" title="" caption="" %}


If sample data creation fail as expected, you need to do these steps:

1. Go to C:\Program Files\Windows SharePoint Services\15.0\Data

2. Right-click on the *Analytics_GUID* folder and choose **Properties**

3. On the *Sharing* tab click **Advanced Sharing**

4. Tick the box to share the folder (don’t change the share name) and click the Permissions button

5. Click **Add**, enter WSS\_ADMIN\_WPG and click **Check Names**, then **OK**.  Grant the user *Full Control* and click **Apply**, **OK** in both windows.

6. Close the window and run SharePoint Products Configuration Wizard again (you will find it in Start menu)

Everything should be ready now, except you can't access SP Central Administration, because you probably do not have password for your Windows Server account set.

Hit CTRL+ALT+DELETE and click Change password, leave the old one blank and set the new one.

Go to http://ws2012:10801 in your browser (or run SP Central Administration from Start menu) and you should be able to log in with username **WS2012\username** and your new password

{% include image.html img="assets/sharepoint-on-windows-server-2012/sp_06.png" title="" caption="" %}

{% include image.html img="assets/sharepoint-on-windows-server-2012/sp_07.png" title="" caption="" %}

To get to vanilla SP Homepage, just type http://ws2012

{% include image.html img="assets/sharepoint-on-windows-server-2012/sp_08.png" title="" caption="" %}
