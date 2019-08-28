# Linux使用介绍

## 使用VM创建linux虚拟机：

对在创建虚拟机时候，出现的**网络连接**的解释：

1.如果选择是**桥连接**的方式，则虚拟机创建的Linux系统可以和其他的系统通信，但是可能造成ip冲突

2.NAT模式：网络地址转换方式；优点是Linux可以访问外网，不会造成ip冲突；缺点是其他的主机不能访问它

3.主机模式：你的Linux是一个独立的主机，不能访问外网，外网也不能访问它

![1566309495905](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1566309495905.png)



vmtools的安装和使用

  1 开启CentOs虚拟机器

  2 点击

![安装VM_01](F:\桌面保存图片\安装VM_01.PNG)



![vm_02](F:\桌面保存图片\vm_02.jpg)

便可在CentOs系统中有一个供安装VMwareTools-10.2.5-8068393.tar.gz文件，复制这个文件到/opt文件夹中，然后打开终端，执行vmware-install.pl这个文件，便可安装VMware tools软件了。

同时可以使用如下方式设置Windows和CentOs两个操作系统的共享文件夹，在CentOs下这个文件夹的位置是:/mnt/hgfs

![share_document](F:\桌面保存图片\share_document.PNG)

总结：

   1.linux中有且只有一个根目录

2. linux的各个目录存放的内容是规划好，不用乱放文件

3. linux是以文件的形式管理我们的设备，因此linux一切皆为文件

4. linux下各个目录文件存放的是什么内容，需要有一个清晰的认识

5. 学习后，脑后中需要有一个linux目录

   (因为linux是一个使用终端操作的系统，需要对各个文件有详细的了解和认识)

5.3 安装XShell5并使用【XShell的作用：远程登陆linux操作系统，相当于在终端操作linux】

说明:如果希望在Windows上安装好XShell就可以远程访问linux的话，需要开启sshd.service这个服务，通过这个服务和Window端连接

![XShell](F:\桌面保存图片\XShell.PNG)

