##  一、centos 7 配置国内yum镜像源

### 1、配置阿里源

根据官网的说明，分别有 CentOS 6、CentOS 7、CentOS 8等配置操作步骤。

* 备份，将 `CentOS-Base.repo` 文件改为`CentOS-Base.repo.backup`

  ```bash
  cp /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
  ```

* 下载新的 `http://mirrors.aliyun.com/repo/Centos-7.repo`,并命名为`CentOS-Base.repo`

  ```bash
  wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
  或者
  curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
  ```

* 清除缓存

  ```bash
  yum clean all     # 清除系统所有的yum缓存
  yum makecache     # 生成yum缓存
  ```

  

### 2、配置清华镜像仓库

地址： https://mirrors.cnnic.cn/

页面提供了 `CentOS5`，`CentOS6`、`CentOS7` 的镜像仓库配置，下面列出的是`CentOS7`的配置。

* 首先备份 CentOS-Base.repo

  ```bash
  cp /etc/yum.repos.d/CentOS-Base.repo  /etc/yum.repos.d/CentOS-Base.repo.bak
  ```

* 之后启用 TUNA 软件仓库， 将`清华大学镜像仓库信息`写入 `/etc/yum.repos.d/CentOS-Base.repo`

  ```bash
  vim /etc/yum.repos.d/CentOS-Base.repo
  ```

* 将 CentOS-Base.repo 中的内容 更新为 下面的内容：

  ```bash
  # CentOS-Base.repo
  #
  # The mirror system uses the connecting IP address of the client and the
  # update status of each mirror to pick mirrors that are updated to and
  # geographically close to the client.  You should use this for CentOS updates
  # unless you are manually picking other mirrors.
  #
  # If the mirrorlist= does not work for you, as a fall back you can try the
  # remarked out baseurl= line instead.
  #
  #
  
  [base]
  name=CentOS-$releasever - Base
  baseurl=https://mirrors.tuna.tsinghua.edu.cn/centos/$releasever/os/$basearch/
  #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os
  gpgcheck=1
  gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
  
  #released updates
  [updates]
  name=CentOS-$releasever - Updates
  baseurl=https://mirrors.tuna.tsinghua.edu.cn/centos/$releasever/updates/$basearch/
  #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates
  gpgcheck=1
  gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
  
  #additional packages that may be useful
  [extras]
  name=CentOS-$releasever - Extras
  baseurl=https://mirrors.tuna.tsinghua.edu.cn/centos/$releasever/extras/$basearch/
  #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=extras
  gpgcheck=1
  gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
  
  #additional packages that extend functionality of existing packages
  [centosplus]
  name=CentOS-$releasever - Plus
  baseurl=https://mirrors.tuna.tsinghua.edu.cn/centos/$releasever/centosplus/$basearch/
  #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=centosplus
  gpgcheck=1
  enabled=0
  gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
  ```

* 清除缓存

  ```bash
  yum clean all     # 清除系统所有的yum缓存
  yum makecache     # 生成yum缓存
  ```

### 3、epel源 安装和配置

* 查看可用的epel源

  ```bash
  [root@zhu /]yum list | grep epel-release
  epel-release.noarch                         7-11                       extras 
  
  ```

* 安装 epel

  ```bash
  yum install -y epel-release
  ```

* 配置阿里镜像提供的epel源

  ```bash
  wget -O /etc/yum.repos.d/epel-7.repo  http://mirrors.aliyun.com/repo/epel-7.repo
  ```

* 清除缓存

  ```bash
  yum clean all     # 清除系统所有的yum缓存
  yum makecache     # 生成yum缓存
  ```

  

### 4、查看yum源

* 查看所有的yum源：

  ```bash
  yum repolist all
  ```

* 查看可用的yum源：

  ```bash
  yum repolist enabled
  ```

  