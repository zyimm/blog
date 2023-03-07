---
title: Qemu模拟win10环境
---

# QEMU

QEMU是一个通用的开源机器仿真器和虚拟机。

当用作机器仿真器时，QEMU可以在不同的机器（例如您自己的PC）上运行针对一台机器（例如ARM板）的操作系统和程序。通过使用动态翻译，它获得了非常好的性能。比如模拟嵌入式开发环境！

ubuntu 22.04 桌面版本已经默认安装了，所以无需额外安装。

# win10 镜像

win10 镜像可以在[https://next.itellyou.cn/Original/#cbp=Product?ID=f905b2d9-11e7-4ee3-8b52-407a8befe8d1](https://next.itellyou.cn/Original/#cbp=Product?ID=f905b2d9-11e7-4ee3-8b52-407a8befe8d1) 下载原版win10镜像

建立一个虚拟机文件夹，如win10，接着如下操作步骤：

1. 将上文镜像iso文件移动进来重命名为`win10.iso`
2. 在当前目录下，使用`qemu-img` 创建运行虚拟机镜像,大小40G&格式为qcow2。命令如下：`qemu-img create -f qcow2 win10.img 40G`
3. 如下是启动win10虚拟机相关命令：

```sh
qemu-system-x86_64 -hda ./win10.img -cdrom win10.iso  -boot d -enable-kvm -machine q35 -device intel-iommu -smp 2,sockets=1,cores=2 -m 2G -vga std -net nic,model=e1000 -net user -usbdevice tablet

```

提示：
下次启动，移除`-cdrom win10.iso` 参数，第一次需要通过镜像安装！

# qemu-system-x86_64 命令相关说明

## smp

配置客户机的smp系统

```sh
-smp [cpus=]n[,maxcpus=cpus][,cores=cores][,threads=threads][,sockets=sockets]
```

**子参数说明**

1. [cpus=]n                    设置客户机中使用逻辑的CPU数量（默认值是1）
2. [,maxcpus=cpus]       设置客户机最大可能被使用的CPU数量（可以用热插拔hot-plug添加CPU，不能超过maxcpus上限）
3. [,cores=cores]           设置每个CPU socket上的core数量（默认值是1）
4. [,threads=threads]    设置每个CPU core上的线程数（默认值是1）
5. [,sockets=sockets]    设置客户机中总的CPU socket数量

**示例**

qemu-system-x86_64  -smp 1,sockets=1,cores=2,threads=2 -boot cd -hda /server/kvm-1.qcow2  -enable-kvm​

## cpu

设置CPU模型。默认情况会给客户机提供qemu64或qemu32的基本CPU模型。这样做可以对CPU特性提供一些高级的过滤功能，让客户机在同一组硬件平台上的动态迁移会更加平滑和安全。在客户机中查看CPU信息(cat /proc/cpuinfo)，model name就是当前CPU模型的名称。

## m

设置内存大小,支持M、G为单位，默认为128MB。

**示例**

```sh
qemu-system-x86_64 -m 1GB -boot cd -hda /server/kvm-1.qcow2 -mem-path /dev/hugepages 

```

## mem-path

对于内存访问密集型的应用，使用huge page是可以比较明显地提高客户机性能。使用huge page的内存不能被换出（swap out），也不能使用ballooning方式自动增长。

**x86支持2MB大小的大页（huge page**

**示例**

```sh
qemu-system-x86_64 -m 1GB -boot cd -hda /server/kvm-1.qcow2 -mem-path /dev/hugepages 

```

## mem-prealloc

使宿主机在客户机启动时就全部分配好客户机的内存。

## hda|hdb|hdc|hdd

1. 若客户机使用PIIX_IDE驱动，显示为/dev/hda设备
2. 若客户机使用ata_piix驱动，显示为/dev/sda设备
3. 若没有使用-hdx的参数，则默认使用-hda参数
4. 可以将宿主机的一块硬盘作为-hda的参数使用,文件名包含逗号，应使用两个连续的逗号进行转义。

## fda|fdb

为客户机指定软盘设备，指定客户机的第一个软盘设备（序号0）

1. fda指定的设备对应在客户机中显示为/dev/fd0
2. fdb指定的设备对应在客户机中显示/dev/fd1

## cdrom

为客户机指定光盘CD-ROM。可以将宿主机的光驱（/dev/cdrom）设备作为-cdrom参数使用。**因为-cdrom就是客户机里的第三个IDE设备，所以-cdrom参数不能与-hdc参数同时使用。**

## vga

配置图形显卡

**子参数说明**

1. std 使用 -vga std 你可以得到最高 2560 x 1600 像素的分辨率。从 QEMU 2.2 开始是默认选项。
2. qxl QXL是一个支持2D的并行虚拟化图形驱动。需要在客户机中安装驱动并在启动QEMU时设置-vga qxl选项。你可能也会想使用#SPICE优化QXL的图形表现。在Linux客户机中，需要加载qxl和bochs_drm这两个内核模块，以获得一个比较好的效果。QXL设备的默认VGA内存大小为16M，这样的内存大小最高支持QHD (2560x1440)的分辨率，如果想要一个更高的分辨率，请增加vga_memmb。
3. vmware 尽管Bug有点多，但相比于std和cirrus它的表现会更好。对于Arch Linux客户机来说可以安装xf86-video-vmware包和xf86-input-vmmouse包获取VMware驱动。

## mtdblock

为客户机指定一个Flash存储器（闪存）

`-mtdblock file use 'file' as on-board Flash memory image`

## -sd 参数

为客户机指定一个SD卡。

`-sd file use 'file' as SecureDigital card image`

## pflash

为客户机指定一个并行Flash存储器。

`-pflash file use 'file' as a parallel flash image`

## drive

详细定义一个存储驱动器

```sh
-drive [file=file][,if=type][,bus=n][,unit=m][,media=d][,index=i] [,snapshot=on|off][,cache=writethrough|writeback|none|directsync|unsafe][,aio=threads|native][,format=f][,addr=A][,id=name][,readonly=on|off]
```

**子参数说明**

1. [file=file]：加载file镜像文件到客户机的驱动器中。
2. [,if=type]：指定驱动器使用的接口类型：可用的类型有：ide、scsi、virtio、sd、floopy、pflash等。
3. [,bus=n]：设置驱动器在客户机中的总线编号。
4. [,unit=m]：设置驱动器在客户机中的单元编号。
5. [,media=d]：设置驱动器中媒介的类型，值为disk或cdrom。
6. [,index=i]：设置在通一种接口的驱动器中的索引编号。
7. [,snapshot=on|off]：当值为on时，qemu不会将磁盘数据的更改写回到镜像文件中，而是写到临时文件中，可以在qemu moinitor中使用commit命令强制将磁盘数据保存回镜像文件中。
8. [,cache=writethrough|writeback|none|directsync|unsafe]：设置宿主机对块设备数据访问的cache模式。，writethrough（直写模式）：调用write写入数据的同时将数据写入磁盘缓存和后端块设备中。writeback（回写模式）：调用write写入数据时只将数据写入到磁盘缓存中，当数据被换出缓存时才写入到后端存储中。优点写入数据块，缺点系统掉电数据无法恢复。
9. [,aio=threads|native]：选择异步IO的方式：threads：为aio参数的默认值，让一个线程池去处理异步IO；native：只适用于cache=none的情况，使用的是linux原生的AIO。
10. [,format=f]：指定使用的磁盘格式，默认是QEMU自动检测磁盘格式的。
11. [,serial=s]：指定分配给设备的序列号。
12. [,addr=A]：分配给驱动器控制器的PCI地址，只在使用virtio接口时适用。
13. [,id=name]：设置驱动器的ID，可以在QEMU monitor中用info block看到。
14. [,readonly=on|off]：设置驱动器是否只读。

## boot

设置客户机启动顺序,在qemu模拟的x86平台中，用"a"、"b"分别表示第一和第二软驱，用"c"表示第一个硬盘，用"d"表示CD-ROM光驱，用"n"表示从网络启动。默认从硬盘启动。

**子参数说明**

1. [order=drives]：设置启动顺序。
2. [,once=drives]：只设置下一次启动的顺序，再重启后无效。
3. [,menu=on|off]：设置交互式的启动菜单（需要BIOS支持）。

## net nic

创建客户机的网卡

`-net nic[,vlan=n][,netdev=nd][,macaddr=mac][,model=type][,name=str][,addr=str][,vectors=v]`

**子参数说明**

1. [,vlan=n]：网卡所属VLAN。
2. [,netdev=nd]：
3. [,macaddr=mac]：设置网卡的MAC地址。
4. [,model=type]：设置模拟的网卡类型
5. [,name=str]：设置网卡的名称。
6. [,addr=str]：设置网卡的PCI设备地址。
7. [,vectors=v]：设置王阿卡设备的MSI-X向量的数量，仅对使用virtio驱动有效。

**示例**

```sh
qemu-system-x86_64 -m 1024 -smp 2 -boot cd -hda /server/kvm-1.qcow2  -enable-kvm -vnc :1 -net nic,macaddr=52:54:00:12:34:22,model=e1000,addr=08 -net nic
```

## netdev tap

创建一个tap设备作为后端，这个tap设备可以连接到bridge、fd、vhost等设备或者模块来为虚拟机网络接口创造数据包通路

`-netdev tap,id=str[,fd=h][,fds=x:y:...:z][,ifname=name][,script=file][,downscript=dfile]`

## netdev bridge

配置私有网桥。

`-netdev bridge,id=str[,br=bridge][,helper=helper]`

## netdev user

配置内部用户网络，与其它任何vm和外部网络都不通，属于宿主host和qemu内部的网络通道。

从vm上访问外部网络相当于在宿主host上访问。但是User Networking不支持某些网络特性，例如ICMP报文，因此在vm中不能使用ping命令。

```sh
-netdev user,id=str[,ipv4[=on|off]][,net=addr[/mask]][,host=addr][,ipv6[=on|off]][,ipv6-net=addr[/int]][,ipv6-host=addr][,restrict=on|off][,hostname=host][,dhcpstart=addr][,dns=addr][,ipv6-dns=addr][,dnssearch=domain][,tftp=dir][,bootfile=f][,hostfwd=rule][,guestfwd=rule][,smb=dir[,smbserver=addr]]
```
