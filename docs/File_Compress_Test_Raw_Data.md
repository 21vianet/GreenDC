# Host Information
**Kernel**: 5.4.0-81-generic  
**OS**: Ubuntu 20.04.3 LTS  
**CPU**: 24cores logic
```
processor       : 23
vendor_id       : GenuineIntel
cpu family      : 6
model           : 63
model name      : Intel(R) Xeon(R) CPU E5-2620 v3 @ 2.40GHz
stepping        : 2
microcode       : 0x46
cpu MHz         : 1200.314
cache size      : 15360 KB
physical id     : 1
siblings        : 12
core id         : 5
cpu cores       : 6
apicid          : 27
initial apicid  : 27
fpu             : yes
fpu_exception   : yes
cpuid level     : 15
wp              : yes
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant_tsc arch_perfmon pebs bts rep_good nopl xtopology nonstop_tsc cpuid aperfmperf pni pclmulqdq dtes64 ds_cpl vmx smx est tm2 ssse3 sdbg fma cx16 xtpr pdcm pcid dca sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand lahf_lm abm cpuid_fault epb invpcid_single pti ssbd ibrs ibpb stibp tpr_shadow vnmi flexpriority ept vpid ept_ad fsgsbase tsc_adjust bmi1 avx2 smep bmi2 erms invpcid cqm xsaveopt cqm_llc cqm_occup_llc dtherm arat pln pts md_clear flush_l1d
bugs            : cpu_meltdown spectre_v1 spectre_v2 spec_store_bypass l1tf mds swapgs itlb_multihit
bogomips        : 4801.49
clflush size    : 64
cache_alignment : 64
address sizes   : 46 bits physical, 48 bits virtual
```
**Memory**: 128G 
```
root@b28r1013cs01-in-f2:~# cat /proc/meminfo 
MemTotal:       131928064 kB
MemFree:        127252112 kB
MemAvailable:   130246552 kB
Buffers:          513852 kB
Cached:          3210248 kB
SwapCached:            0 kB
Active:          1114972 kB
Inactive:        2731304 kB
Active(anon):     132968 kB
Inactive(anon):      184 kB
Active(file):     982004 kB
Inactive(file):  2731120 kB
Unevictable:       19044 kB
Mlocked:           19044 kB
SwapTotal:       8388604 kB
SwapFree:        8388604 kB
Dirty:                 0 kB
Writeback:             0 kB
AnonPages:        141440 kB
Mapped:           165364 kB
Shmem:              2752 kB
KReclaimable:     245472 kB
Slab:             475076 kB
SReclaimable:     245472 kB
SUnreclaim:       229604 kB
KernelStack:        7536 kB
PageTables:         3556 kB
NFS_Unstable:          0 kB
Bounce:                0 kB
WritebackTmp:          0 kB
CommitLimit:    74352636 kB
Committed_AS:    1788876 kB
VmallocTotal:   34359738367 kB
VmallocUsed:      139476 kB
VmallocChunk:          0 kB
Percpu:            33792 kB
HardwareCorrupted:     0 kB
AnonHugePages:         0 kB
ShmemHugePages:        0 kB
ShmemPmdMapped:        0 kB
FileHugePages:         0 kB
FilePmdMapped:         0 kB
CmaTotal:              0 kB
CmaFree:               0 kB
HugePages_Total:       0
HugePages_Free:        0
HugePages_Rsvd:        0
HugePages_Surp:        0
Hugepagesize:       2048 kB
Hugetlb:               0 kB
DirectMap4k:      355064 kB
DirectMap2M:     6889472 kB
DirectMap1G:    128974848 kB
```
**Docker**: 
```
Client: Docker Engine - Community
 Version:           20.10.22
 API version:       1.41
 Go version:        go1.18.9
 Git commit:        3a2c30b
 Built:             Thu Dec 15 22:28:08 2022
 OS/Arch:           linux/amd64
 Context:           default
 Experimental:      true

Server: Docker Engine - Community
 Engine:
  Version:          20.10.22
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.18.9
  Git commit:       42c8b31
  Built:            Thu Dec 15 22:25:58 2022
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.6.15
  GitCommit:        5b842e528e99d4d4c1686467debf2bd4b88ecd86
 runc:
  Version:          1.1.4
  GitCommit:        v1.1.4-0-g5fd4c4d
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
```
**Libvirt**:
```
root@b28r1013cs01-in-f2:~# virsh  --version
6.0.0
```
**Qemu**:
```
root@b28r1013cs01-in-f2:~# qemu-system-x86_64 -version
QEMU emulator version 4.2.1 (Debian 1:4.2-3ubuntu6.24)
Copyright (c) 2003-2019 Fabrice Bellard and the QEMU Project developers
```

## Open Performance Switch
```
sudo cpufreq-set -g performance
```
关闭swap分区
```
swapoff -a
#remove the swap line in /etc/fstab
vi /etc/fstab
```

# CPU PXZ
## Testing
压缩是云工作负载中经常使用的组件。PXZ是一个使用LZMA算法的并行无损数据压缩实用程序。我们使用Parallel PXZ 4.999.9beta (build 20230112)来压缩enwik9，这是一个1GB的Wikipedia转储文件，经常用于压缩基准测试。为了专注于压缩而不是I/O，我们使用了32个线程，输入文件缓存在RAM中，输出通过管道传输到/dev/null。我们使用压缩级别2。
### Host 
#### Installation
```
apt-get install -y git gcc liblzma-dev unzip
git clone https://github.com/jnovy/pxz.git
cd pxz
make
make install
#### Download the data
wget -c http://mattmahoney.net/dc/enwik9.zip -P data
cd data
unzip enwik9.zip
mkdir -p /mnt/data
mount -t tmpfs -o size=2048m tmpfs /mnt/data
cp enwik9.zip /mnt/data
```

#### Commands
```shell
time pxz -T24 -c /mnt/data/enwik9  >/dev/null
```
#### Result
|  次数   |                    结果                           |
|  ----  | ------------------------------------------------  |
| 1  | real    0m58.773s <br>user    16m35.508s <br>sys     0m13.542s |
| 2  | real    0m58.179s <br>user    16m33.636s <br>sys     0m10.952s |
| 3  | real    0m58.990s <br>user    16m41.675s <br>sys     0m11.822s|
| 4  | real    1m0.353s  <br>user    16m42.017s <br>sys     0m12.083s|
| 5  | real    0m59.069s <br>user    16m44.156s <br>sys     0m13.276s|
| 6  | real    0m59.332s <br>user    16m41.055s <br>sys     0m12.166s|
| 7  | real    0m58.982s <br>user    16m37.540s <br>sys     0m11.263s|
| 8  | real    0m57.939s <br>user    16m36.843s <br>sys     0m10.283s|
| 9  | real    0m58.028s <br>user    16m40.307s <br>sys     0m10.927s|
| 10 | real    1m0.134s <br>user    16m44.135s <br>sys     0m13.162s|

### Container
#### Installation
Dockerfile:
```
FROM ubuntu:20.04
RUN apt update && apt install -y gcc git liblzma-dev unzip make
RUN git clone https://github.com/jnovy/pxz.git
RUN cd pxz && make && make install
```
```shell
docker build --build-arg http_proxy=http://172.22.50.204:3128 --build-arg https_proxy=http://172.22.50.204:3128 -t ubuntu:20.04-pxz .
```
镜像size： 359MB

#### Commands
```shell
docker run -it --rm -v /mnt/data/:/mnt ubuntu:20.04-pxz bash
# In Container shell, run the following commands.
time pxz -T24 -c /mnt/data/enwik9  >/dev/null
```
#### Result
|  次数   |                    结果                           |
|  ----  | ------------------------------------------------  |
| 1  | real    0m59.845s <br>user    16m51.136s <br>sys     0m11.596s |
| 2  | real    0m59.517s <br>user    16m52.007s <br>sys     0m12.203s |
| 3  | real    0m59.045s <br>user    16m50.992s <br>sys     0m12.039s|
| 4  | real    0m59.571s <br>user    16m49.573s <br>sys     0m12.483s|
| 5  | real    0m59.373s <br>user    16m50.407s <br>sys     0m12.667s|
| 6  | real    1m0.209s <br>user    16m51.988s <br>sys     0m13.996s|
| 7  | real    1m0.243s <br>user    16m51.385s <br>sys     0m12.240s|
| 8  | real    0m59.417s <br>user    16m54.045s <br>sys     0m11.948s|
| 9  | real    0m58.430s <br>user    16m52.021s <br>sys     0m12.267s|
| 10 | real    0m58.728s <br>user    16m49.158s <br>sys     0m10.222s|

### Virtual Machine
#### Installation
```
# wget http://cloud-images.ubuntu.com/focal/20230111/focal-server-cloudimg-amd64-disk-kvm.img
wget http://cloud-images.ubuntu.com/focal/20230111/focal-server-cloudimg-amd64.img
cat >meta-data <<EOF
instance-id: ubuntu-pxz
local-hostname: ubuntu-pxz
EOF
cat >user-data <<EOF
#cloud-config
users:
  - default
  - name: jamlee
    homedir: /home/jamlee
    sudo: ALL=(ALL) NOPASSWD:ALL
    groups: users, admin
    lock_passwd: false
    shell: /bin/bash
    passwd: "$6$rounds=4096$nCLwFcSq$MKfDalSnEKRQh8LwvNPsMo6gQVnJBtlfuPCkV/atr5j8U5e87UoKkA9u1ZeNIpvNg7FO44IE04l6g3a91hgZV0"
system_info:
    default_user:
      name: ubuntu
      home: /home/ubuntu
password: ubuntu
chpasswd:
    expire: false
hostname: ubuntu-pxz
ssh_pwauth: yes
apt:
  http_proxy: http://172.22.50.204:3128
  https_proxy: http://172.22.50.204:3128
# runs apt-get update
package_update: true

# runs apt-get upgrade
package_upgrade: false

# installing additional packages
packages:
  - make
  - gcc
  - git
  - liblzma-dev
  - unzip

# run some commands on first boot
runcmd:
  - git clone https://github.com/jnovy/pxz.git && cd pxz && make && make install

# after system comes up first time. 
final_message: "The system is finally up, after $UPTIME seconds"
EOF
# ubuntu用户的密码是ubuntu，jamlee的密码是possible
# install the virt-install commands
# apt-get install -y virt-manager 版本问题不支持--cloud-init参数
git clone https://github.com/virt-manager/virt-manager.git
cd virt-manager/
git checkout v3.1.0
apt install libvirt-python
apt-get install gettext
sed -i 's/#group = "root"/group = "root"/g' /etc/libvirt/qemu.conf
sed -i 's/#user = "root"/user = "root"/g' /etc/libvirt/qemu.conf
systemctl restart libvirtd
# install scripts
virt-install \
  --name ubuntu-pxz \
  --memory 122880 \
  --vcpus 24 \
  --cpu host \
  --os-type linux \
  --os-variant ubuntu20.04 \
  --network bridge=virbr0 \
  --boot hd \
  --nographics --disk size=10,backing_store=$PWD"/focal-server-cloudimg-amd64.img",bus=virtio \
  --cloud-init user-data=$PWD/user-data,meta-data=${PWD}/meta-data

virsh console ubuntu-pxz
# if the virtual machine init failed, you should install the package by userself.
# in the virtualmachine run the following commands.
swapoff -a
mkdir -p /mnt/data
mount -t tmpfs -o size=2048m tmpfs /mnt/data
# in another console, copy the data into /mnt/data directory.
scp /mnt/data/enwik9 ubuntu@192.168.122.12:/mnt/data

# destroy scripts
# virsh destroy ubuntu-pxz
# virsh undefine ubuntu-pxz
# rm -rf /var/lib/libvirt/images/ubuntu-pxz.qcow2

```
镜像size：2.5G

#### Commands

```shell
# run the testing commands.
time pxz -T24 -c /mnt/data/enwik9  >/dev/null
```

#### Result
|  次数   |                    结果                           |
|  ----  | ------------------------------------------------  |
| 1  | real    1m9.062s <br>user    19m56.796s <br>sys     0m14.888s |
| 2  | real    1m11.042s <br>user    19m59.304s <br>sys     0m12.451s |
| 3  | real    1m11.573s <br>user    19m58.155s <br>sys     0m12.642s |
| 4  | real    1m11.439s <br>user    19m58.356s <br>sys     0m17.123s |
| 5  | real    1m11.350s <br>user    20m1.946s <br>sys     0m12.505s |
| 6  | real    1m10.739s <br>user    19m55.354s <br>sys     0m12.236s |
| 7  | real    1m9.189s <br>user    19m54.941s <br>sys     0m11.906s |
| 8  | real    1m9.218s <br>user    20m0.755s <br>sys     0m11.991s |
| 9  | real    1m10.649s <br>user    19m53.747s <br>sys     0m12.397s |
| 10 | real    1m9.720s <br>user    19m51.436s <br>sys     0m12.941s |

# Relate Issue
https://gitlab.dev.21vianet.com/sbg2-rd-center/openstack/galley/-/issues/156