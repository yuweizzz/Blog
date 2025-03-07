---
date: 2021-10-09 15:00:00
title: 硬盘知识笔记
tags:
  - "HDD"
draft: false
---

这篇笔记用来记录一些硬盘的相关知识。

<!--more-->

```bash

                                       (@@) (  ) (@)  ( )  @@    ()    @     O     @     O      @
                                  (   )
                              (@@@@)
                           (    )

                         (@@@)
                       ====        ________                ___________
                   _D _|  |_______/        \__I_I_____===__|_________|
                    |(_)---  |   H\________/ |   |        =|___ ___|      _________________
                    /     |  |   H  |  |     |   |         ||_| |_||     _|                \_____A
                   |      |  |   H  |__--------------------| [___] |   =|                        |
                   | ________|___H__/__|_____/[][]~\_______|       |   -|                        |
                   |/ |   |-----------I_____I [][] []  D   |=======|____|________________________|_
                 __/ =| o |=-~O=====O=====O=====O\ ____Y___________|__|__________________________|_
                  |/-=|___|=    ||    ||    ||    |_____/~\___/          |_D__D__D_|  |_D__D__D_|
                   \_/      \__/  \__/  \__/  \__/      \_/               \_/   \_/    \_/   \_/

```

## 常见的硬盘接口和传输速率

硬盘这一类外设存储处于计算机存储设备中最外围的一层，但它是数据持久化的核心部分，可靠性和传输速率是非常重要的指标。

外设和计算机通信需要建立传输链路，这个链路一般称为总线 bus ，使用通用的外设接口 interface 来增强总线的扩展性，使用传输协议 protocol 来协同传输信号。

### IDE

早期的硬盘接口为 IDE ，全称为电子集成驱动器 Integrated Drive Electronics ，它是直连主板的 40 针并口总线设计，拥有独立的电源接口。在一些老旧的 intel 奔腾 PC 机还可以见到这类硬盘，它使用的是宽大的并行排线，一般一个排线有两个接口，用于接入主从两块 IDE 硬盘。

IDE 接口使用的传输协议一般称为 ATA ，它已经演变了多个版本，每个版本的演进都增强了总线的带宽，在这个过程也衍生出了一些变种的接口，在最后的并行总线版本 ATA-7 中，使用 IDE 并行接口的硬盘最大的传输速率达到 133 MB/s ，之后并行总线的设计慢慢由串行总线取代，所以我们所说的 IDE/ATA/PATA 一般都是指这类 40 针并行接口的硬盘。

之后是流行的是 SATA 硬盘接口，至今还是主流的接口种类。

### SATA

SATA ，全称为串行的 ATA 接口 Serial ATA ，它的很多设计都是在 IDE 之上延续的，所以传输协议也不可避免地使用了一些原来的设计。相比于 IDE 接口，它的优势在于排线简单，抗干扰性强，更快的传输速率和更多的扩展功能。 SATA 接口是长条形状的物理接口，中间有断口将整个长条分为长短两个部分，并且断口处有防呆设计。其中长的部分为电源接口，短的部分为数据接口。除了通用的 SATA 接口类型，还有 mSATA ， eSATA 等物理接口变体。

SATA 接口的传输协议有 3 个版本：

- SATA 1.0 ： 1.5 Gbps ， 8b/10b 编码 ， 150 MB/s

- SATA 2.0 ： 3 Gbps ， 8b/10b 编码 ， 300 MB/s

- SATA 3.0 ： 6 Gbps ， 8b/10b 编码 ， 600 MB/s

我们可以通过换算公式 `总线传输速率 * 编码 / 8 = 总线带宽` 计算它的带宽。其中编码一般为校验或者标志位的码率损耗，由比特位换算到存储字节单位还需要除以 8 ，但是最终的带宽值为理论值，实际传输还会有折损，需要再去掉 10% 才能接近实际的带宽。

我们可以看出串行设计的优势所在，即使是最低版本的协议都要比并行版本来的快。但是 SATA 接口的硬盘本身速率都在 100 MB/s 到 200 MB/s 之间，即使是固态硬盘也大多在 300 MB/s 到 400 MB/s 之间，总线的理论上限带宽已足够现有的硬盘使用。

SATA 的传输协议继承了原有 ATA 的一些指令，为了支持原生的 SATA 指令，使用 SATA 设备的高级功能，英特尔制定了 AHCI 技术，来实现这一目的。它可以看做是 SATA 总线扩展的传输协议。现有的 PC 或者服务器的集成 SATA 控制器大都默认使用了这种协议，因为大部分现代设备本身就支持 AHCI 标准。它对一些场景下的设备性能有优化作用，但在带宽上没有特别明显的提升。

### SAS

另一种常见的硬盘物理接口形式为 SAS ，全称为 Serial Attached SCSI ，是 SCSI 技术的一种变体，它和 SATA 硬盘类似，都使用了串行总线技术以获得更高的传输速度。 SAS 接口是长条形状的物理接口，对比 SATA 接口来说，它的中间没有断口，而是使用突起取代，它外观上和 SATA 接口非常相似，所以大部分服务器的背板兼容这两种物理接口。

SCSI ，全称为 Small Computer System Interface ，它是一套庞大复杂的技术标准，比起 SATA 来说，它的应用面更广泛，接口类型也更多。 SAS 只是其中的一种，它的设计兼具了 SATA 的特色，并且在 SAS 的协议栈中，兼容了 SATA 的传输协议，这就是 SAS 控制器能够兼容使用 SATA 接口硬盘的原因。 SAS 比起 SATA 来说成本更高，在 PC 和服务器中，主板很少集成 SAS 控制器，虽然有少数的服务器主板会集成 SAS 控制器，但是更多地是通过外加 HBA 卡或者 RAID 卡来连接 SAS 硬盘。

SAS 接口的传输协议也有 3 个版本：

- SAS 1.1 ： 3 Gbps ， 8b/10b 编码 ， 300 MB/s

- SAS 2.1 ： 6 Gbps ， 8b/10b 编码 ， 600 MB/s

- SAS 3.0 ： 12 Gbps ， 8b/10b 编码 ， 1200 MB/s

SAS 的优势在于存储集群的场景，带宽比较大，并且可以兼容 SATA 这类主流的硬盘接口。在现有的机械硬盘接口中，使用最多的就是 SATA 和 SAS ，但整个存储构架使用了 SAS 的模式，就是因为它的兼容优势。

此外还有两种比较特殊的物理接口: M.2 和 U.2 ，它们多用于固态硬盘中。

### M.2 和 U.2

固态硬盘最明显的特点是速率远高于机械硬盘，为了适应这种速度，总线设计或者传输协议就要进行相应的改进。

在不改变原有总线设计的情况下，普通的固态硬盘还是使用了 SATA 接口，虽然它的速率提升了，但是以 SATA 总线的传输效率还是可以满足它的理论需求。

为了获得更高的总线传输效率，一些固态硬盘弃用了 SATA 总线和接口，使用了 PCIe 总线作为新的传输总线，这是直通 CPU 的捷径，速度提升非常明显。同时也就诞生了新的硬盘接口类型和传输协议。

原生的 PCIe 接口是给网卡或者显卡外设使用，是直接安装在主板的 PCIe 插槽中，一些固态硬盘也是使用了这样的接口设计，还有 M.2 和 U.2 两种接口设计。

M.2 接口是金手指直接安装在主板上的，接触面小，硬盘体积也比较小，外形类似于直尺。

U.2 接口则和 SAS 硬盘基本一致，长条形状的接口，中间无断口，硬盘体积也和普通硬盘类似，可以直接接在硬盘背板接口上。这两种物理接口都是直接连接到主板上的，不过 U.2 特殊一些，需要背板和主板的支持，多用于服务器场景，而 M.2 只需要主板带有对应插槽一般就可以了，服务器和 PC 场景都有使用到。

SATA 接口的固态硬盘的传输速率计算和普通硬盘相同，但实际速率会因为固态硬盘的特性而得到提升，在现有使用 PCIe 总线的固态硬盘中，大部分都使用了 NVMe 传输协议，所以传输速率的计算直接和使用的 PCIe 通道数相关，和 CPU 支持的 PCIe 总线版本相关。如果使用了 PCIe 总线，而采用 SATA 作为传输协议，则无法享受 PCIe 的高带宽了，一般的 PCIe NVMe 的硬盘都是使用 x4 的 PCIe 通道，以 PCIe 3.0 的速率计算，可以达到 4 GB/s 。

## 高级格式化磁盘

传统硬盘的扇区大小一般是 512B ，在 2010 年左右，硬盘厂商开始向 4K (4096B) 扇区过渡，扇区的增大可以提高数据的存储密度，增强错误检测能力，这种扇区大小大于 512B 的硬盘一般会带有 Advanced Format 标识，称为高级格式化。

在硬盘扇区大小的过渡时期，为了实现系统对不同大小扇区的兼容，高级格式化硬盘需要进行一些额外的工作，这部分是硬盘固件负责的，当系统请求 512B 扇区的读写行为时，硬盘会将其转换为对应物理扇区的读写行为，所以有了物理扇区和逻辑扇区的区别。

物理扇区是硬盘原生支持的扇区大小，由硬盘厂商决定，逻辑扇区是操作系统的最小存储扇区大小。虽然不同操作系统有不同大小的逻辑扇区，但最常见的大小就是 512B ，即便不是 512B ，它们一般都是 512B 的整数倍。

4K 是最经典的高级格式化硬盘的物理扇区大小，这类 4K 模拟 512B 的硬盘标记为 512e (512 emulation)，如果 4K 硬盘不支持模拟小扇区的读写行为，标记为 4Kn (4K native)。

```bash
# 这是虚机上运行的虚拟系统盘
$ parted -l
Model: VMware, VMware Virtual S (scsi)
Disk /dev/sda: 10.7GB
Sector size (logical/physical): 512B/512B  # 虚拟硬盘都是模拟的，模拟的设备一般都是原生的 512B 扇区大小
Partition Table: msdos
Disk Flags:

Number  Start   End     Size    Type     File system     Flags
 1      1049kB  525MB   524MB   primary  xfs             boot
 2      525MB   1599MB  1074MB  primary  linux-swap(v1)
 3      1599MB  10.7GB  9138MB  primary  xfs

# 这是实机上运行的一块硬盘，它就是 512e 的硬盘
$ parted -l
Model: ATA ST500LT012-9WS14 (scsi)
Disk /dev/sdb: 500GB
Sector size (logical/physical): 512B/4096B  # 硬盘的物理扇区大小为 4096B ，逻辑扇区大小为 512B ，这是 512e 的 AF 硬盘
Partition Table: msdos
Disk Flags:

Number  Start   End     Size    Type      File system  标志
 1      1049kB  53.7GB  53.7GB  primary   ntfs
 2      53.7GB  500GB   446GB   extended               lba
 5      53.7GB  165GB   112GB   logical   ntfs
 6      165GB   277GB   112GB   logical   ntfs
 7      277GB   389GB   112GB   logical   ntfs
 8      389GB   500GB   111GB   logical   ntfs
```

可以看到，这块希捷的 500GB 机械硬盘的物理扇区大小是 4K ，但是逻辑扇区大小是 512B ，这是在 Linux 系统上比较常见的情况，而虚拟机的物理扇区和逻辑扇区都是 512B ，现代硬盘比较少出现这种情况，尤其是大容量硬盘。

在 512e 硬盘的读写过程中，固件层做了隐式的扇区大小转换，读行为比较简单，我们需要读取的扇区只是 512B 大小，但固件是以 4K 去读取的，它要做的工作是找到目标数据所在的 4K 扇区，读取并返回只需要的 512B 部分。

在写行为时，这个情况会变得略微复杂一些，当确定了需要写入的扇区位置，需要读取整个 4K 物理扇区的内容并修改目的位置的数据，然后将整个扇区写入到硬盘中。这个过程称为 read-modify-write ，如果我们的逻辑扇区和物理扇区没有很好地适配，比如逻辑扇区被分割存储到不同的物理扇区中，这个 read-modify-write 过程将会需要多次进行，这样会严重影响硬盘的性能。为了避免这种情况，我们在硬盘分区的时候需要进行 4K 对齐以达到硬盘的最佳性能。

4K 对齐主要针对 512e 这些物理扇区和逻辑扇区大小不同的硬盘，它需要确保每个分区内都是完整的 4K 物理扇区，避免出现逻辑扇区实际存储时跨越不同物理扇区的情况。逻辑扇区的大小通常和物理扇区成整数倍的关系，这样读取时的处理速度是最高的。这也是现代硬盘在分区时一个值得注意的地方。

```bash
# 检查硬盘分区是否对齐
$ parted [Device Name] align-check opt [Partition Number]

# 实例
$ parted /dev/sda align-check opt 1
1 aligned

# 如命令返回， sda 的分区 1 已经正常对齐
```

## 常用的硬盘分区格式

对新安装的硬盘设备，包括机械硬盘和固态硬盘，一般都需要分区和格式化后才能被系统使用，在 Linux 系统中用得比较多的是 MBR 和 GPT 两种分区方式。

### MBR 分区方式

MBR 分区方式是比较经典的分区方式，这种分区格式流行的时候，硬盘扇区大小为 512B 来设计的，它只用到了硬盘的第一个扇区，这个扇区会保存系统引导代码和分区表，但实际分区表的大小只有 64B ，大部分容量由引导代码所占据，可以达到 446B ，扇区最后的 2 个字节作为标志位，有效的标志为 0xAA55 ，如果破坏掉这两个字节，整个 MBR 分区会失效，硬盘分区无法正常识别，从而导致系统启动异常。

由于存储空间有限， MBR 分区只能有四个 primary 分区或者三个 primary 分区加一个包含 N 个 logical 分区的 extended 分区，理论上 logical 分区没有上限。

```bash
# 同样是前面的两个 MBR 分区格式的硬盘
$ parted -l
Model: VMware, VMware Virtual S (scsi)
Disk /dev/sda: 10.7GB
Sector size (logical/physical): 512B/512B
Partition Table: msdos  # msdos 即为 MBR 分区格式
Disk Flags:

Number  Start   End     Size    Type     File system     Flags
 1      1049kB  525MB   524MB   primary  xfs             boot
 2      525MB   1599MB  1074MB  primary  linux-swap(v1)
 3      1599MB  10.7GB  9138MB  primary  xfs
# 这个硬盘由三个 primary 分区组成

$ parted -l
Model: ATA ST500LT012-9WS14 (scsi)
Disk /dev/sdb: 500GB
Sector size (logical/physical): 512B/4096B
Partition Table: msdos
Disk Flags:

Number  Start   End     Size    Type      File system  Flags
 1      1049kB  53.7GB  53.7GB  primary   ntfs
 2      53.7GB  500GB   446GB   extended               lba
 5      53.7GB  165GB   112GB   logical   ntfs
 6      165GB   277GB   112GB   logical   ntfs
 7      277GB   389GB   112GB   logical   ntfs
 8      389GB   500GB   111GB   logical   ntfs
# 这个硬盘由两个 primary 分区和一个 extended 分区组成，其中 logical 分区包括了四个逻辑分区
```

### GPT 分区方式

GPT 是解决了 MBR 分区方式的局限的新分区方式。

在 GPT 分区方式出现前，硬盘扇区的寻址方式已经由传统的 CHS 寻址方式向 LBA 寻址方式演进， LBA 是兼容 CHS 的寻址方式，对比原有的方式更加先进。对于系统层面来说，不需要关心硬盘的寻址方式是使用 CHS 还是 LBA ，兼容转换的工作都由硬盘完成。

GPT 使用 LBA 获取的区块来存储分区信息，其中 LBA 0 作为对传统 MBR 分区格式的兼容， LBA 1 用来保存 GPT 头， LBA 2 到 LBA 33 用来保存 GPT 分区信息，并且 LBA 1 到 LBA 33 的内容会被镜像保存在整块硬盘最后的扇区中，作为 GPT 的备份分区信息。所以 GPT 分区方式一共会占据硬盘开头的 34 个 LBA 区块和最后的 33 个 LBA 区块。

除了占据的空间大小和 MBR 不同， GPT 没有 logical 分区和 extended 分区，所有分区都是相同级别，并且理论上没有数量限制。

```bash
$ parted -l
Model: VMware, VMware Virtual S (scsi)
Disk /dev/sdb: 12.9GB
Sector size (logical/physical): 512B/512B
Partition Table: gpt
Disk Flags:

Number  Start   End     Size    File system  Name     Flags
 1      1049kB  12.9GB  12.9GB               primary

# 分区无级别，这里只是习惯命名为 primary
```

现代硬盘分区工具一般会默认以第 2048 块扇区作为起始扇区，可以由 `2048 * 512 / ( 1024 * 1024 ) = 1M` 计算得出，最终会在硬盘最开始 1M 空间之后开始分区，这样设置起始扇区会在正式的分区和分区表之间留下空白扇区，很多引导程序都会安装在这部分区域，比如 Grub ，它对系统启动是有较大意义，如果是作为普通数据盘，在 GPT 格式下，最大也只能 LBA 34 开始正式分区，这种方式的开始扇区大约会在 17kB 左右。

分区的具体操作可以参考之前的[文章](https://yuweizzz.github.io/post/practical_tips_in_linux/#%E7%A1%AC%E7%9B%98%E5%88%86%E5%8C%BA%E5%92%8C%E6%A0%BC%E5%BC%8F%E5%8C%96)。

## 硬盘的 S.M.A.R.T. 指标

现代硬盘一般还内置了状态检测和硬件信息报告输出的机制，用来作为硬盘健康的评测依据，以保障硬盘数据的安全。这项技术称为 Self Monitoring Analysis and Reporting Technology ，输出的信息一般简称为 S.M.A.R.T. 指标。

这里列出一些主要用于机械硬盘硬盘健康的相关指标：

- SMART 5 : Reallocated_Sector_Count --> 扇区重定位计数，如果物理扇区已经损坏，硬盘会自动把指向这一故障扇区的请求映射到特定的空间作为代替，这个值的增长说明硬盘已经出现坏块，如果坏块数量过多，会明显影响硬盘的读写。
- SMART 187 : Reported_Uncorrect --> 无法纠错扇区计数，这个值增长说明硬盘的部分扇区已经出现故障并且无法恢复纠正。
- SMART 188 : Command_Timeout --> 命令超时计数，对某些硬盘指令无响应的计数值，这个值增长说明硬盘变得不稳定，对这个硬盘读写有可能出现 IO Error 。
- SMART 196 : Reallocation_Event_Count --> 重定位事件计数，和 SMART 5 类似，用来记录已重映射扇区和可能重映射扇区的事件，这个值增长说明硬盘不稳定。
- SMART 197 : Current_Pending_Sector_Count --> 等候重定位的扇区计数，记录了不稳定的扇区的数量，这些扇区可能是响应过慢或者故障，这个值增长说明硬盘不稳定。
- SMART 198 : Offline_Uncorrectable --> 已经无法纠错的掉线扇区计数，和 SMART 187 类似，记录已经出错的掉线扇区数量，这个值增长说明硬盘出现坏块。

某些硬盘厂商可以对固件进行定制，设置不同的 SMART 指标，但以上几个指标在不同厂家都是类似的，它们可以作为是否更换硬盘的参考。
