<!-- title : RUID/EUID/SUID -->

# RUID EUID SUID介绍 #

参考: [UID/EUID/SUID](http://blog.cykerway.com/post/396)

* `Real user ID(RUID)` - Login时所用的ID，表示用户的真实身份
* `Effective user ID(EUID)` - 有效ID，执行各种操作，进行权限检查，创建文件、目录时所用的ID
* `Saved set-user ID(SUID)` - 执行具有setuid位的程序时用于保存该程序文件的属主ID

先介绍SUID，修改密码时，会用到`passwd`命令，修改的文件是`/etc/shadown`

	root@gentoo ~ # ll /etc/shadow
	-rw-r----- 1 root root 663 Feb 16 10:02 /etc/shadow

可以看到owner/group都是root

但是为什么任何用户都可以修改自己密码，这就是SUID的作用

修改密码的命令`passwd`，路径`/bin/passwd`

	root@gentoo-gs ~ # ll /bin/passwd
	-rws--x--x 1 root root 42592 Sep 17 13:20 /bin/passwd

可以看到，owner得x权限位置不是x，而是s，这就是suid权限

至于SUID的详细作用，Google之


接下来谈RUID和EUID

RUID就是实际登陆时的UID，可以通过`id`命令来获取

	$ man id
	id - print real and effective user and group IDs

	$ id -run
	root


而EUID是实际起作用的UID

	$ id -un
	root

	或者

	$ whoami


NOTE：这里在看`man id`时，

	-n, --name
		  print a name instead of a number, for -ugG

	-r, --real
		  print the real ID instead of the effective ID, with -ugG

	-u, --user
		  print only the effective user ID


先开始我使用


	$ id -r
	id: cannot print only names or real IDs in default format


后来发现上面已经写了，`with -ugG`，意思就是要配合`ugG`这三个使用

`-ru`就是使用read uid代替effective uid，先开始没注意，以为用`-r`就可以了

不过这里感觉确实怪怪的，不如直接用`-r`就可以获取read uid方便


一般情况下，RUID == EUID  
但是遇到比如上面距离的passwd，则就不同了，具体过程[TODO]


# Read More #

* [What is GID? What is EUID? What is SUID? What is RUID?](http://centoscert.com/content/what-gid-what-euid-what-suid-what-ruid)
* [实际用户ID，有效用户ID及设置用户ID](http://blog.csdn.net/guosha/article/details/2679334)
* [real and effective user id](http://en.allexperts.com/q/Unix-Linux-OS-1064/real-effective-user-id.htm)
* [Real and Effective IDs](http://www.makelinux.net/alp/083)
* [Linux > More on USER ID, Password, and Group management](http://www.cyberciti.biz/tips/linux-more-on-user-id-password-and-group-management.html)(*)


# History #

Create 2013/02/16

Modify 2013/02/16
