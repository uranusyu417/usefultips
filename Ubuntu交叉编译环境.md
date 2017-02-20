# Ubuntu交叉编译环境

**实现方法**：

思路上实现交叉编译的搭建可以有：

1. 使用交叉编译工具CROOSS_TOOLCHAIN直接在开发机器（host）上编译出目标机（target）可执行的二进制文件
2. 使用虚拟机（qemu，kvm等）在host上虚拟target机器，在虚拟的target中搭建编译环境。
3. 使用debootstrap（针对debian系统）在host上利用chroot虚拟系统，使用chroot/schroot在虚拟环境中进行编译

## 利用debootstrap|chroot交叉编译
