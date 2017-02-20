# Ubuntu交叉编译环境

**实现方法**：

思路上实现交叉编译的搭建可以有：

1. 使用交叉编译工具CROOSS_TOOLCHAIN直接在开发机器（host）上编译出目标机（target）可执行的二进制文件
2. 使用虚拟机（qemu，kvm等）在host上虚拟target机器，在虚拟的target中搭建编译环境。
3. 使用debootstrap（针对debian系统）在host上利用chroot虚拟系统，使用chroot/schroot在虚拟环境中进行编译

## 利用debootstrap|chroot交叉编译

**chroot的优点在于：**

1. 建立独立的环境，可用于开发测试
2. 使用qemu的功能，可建立target机器用户空间（user mode）的编译、运行环境，实现用户空间程序的交叉编译

**步骤：**

Step 1： 安装所需的工具， host：

`sudo apt-get install binfmt-support qemu qemu-user-static debootstrap`
  
Step 2: 建立用于安装挂在系统的根目录

`mkdir <root directory>`   例如mkdir ~/xenial_armhf

Step 3: 安装系统

`sudo debootstrap --arch=armhf --foreign --variant=buildd --verbose xenial xenial_armhf`
  
--arch选择target机器的架构，如果与host为不同架构，需要搭配--foreign参数，这样会下载解压需要的package，但并不会安装

其它参数参见debootstrap mannual

`sudo cp /usr/bin/qemu-arm-static xenial_armhf/usr/bin`

此处将qemu命令拷贝至chroot环境用以运行target架构下的binary文件，如不拷贝，会出现类似**chroot: failed to run command ‘/bin/bash’: No such file or directory**的错误提示，原因是在执行chroot命令后运行的/bin/bash为target上架构的binary

`sudo chroot xenial_armhf/`    切换至target环境

target: `/debootstrap/debootstrap --second-stage`  完成安装

Step 4：添加schroot配置文件

host环境中，添加编辑/etc/schroot/chroot.d/xenial_armhf

    [xenial-armhf]
    description=Ubuntu xenial armhf
    type=directory
    directory=<host环境安装目录， 例如~/xenial_armhf的绝对路径>
    groups=sbuild,root
    root-groups=sbuild,root
    users=root,<当前host环境的用户名>

至此，可通过`schroot -c chroot:xenial-armhf`用当前用户登入target环境，`schroot -c chroot:xenial-armhf -u root`用root登入target环境。
