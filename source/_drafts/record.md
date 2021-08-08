# ubuntu
- 终端管理器：nautilus

## [共享文件夹](https://blog.csdn.net/qq_43419705/article/details/108690683)

**创建共享文件夹**
- 创建一个共享文件夹，然后右键选择“本地网络共享”
- 安装功能包samba
  - 如果前面勾选了修改权限，那么iPhone是可以向Ubuntu传输文件的。方法是选择一张图片->“左下角分享”->“存储到‘文件’”->“服务器ip”->“共享文件夹”->“存储”。
  - 假如上传文件失败，需要给共享文件夹权限：`chmod 777 xxxx`
**创建共享文件账号**
- 共享的文件夹，其实并不是Ubuntu系统下的目录，而是挂载的目录。
- 安装软件：`sudo apt-get install -y smb*`
- 创建smb账号：`sudo smbpasswd -a 账号名`
  - 这个账号名必须是Ubuntu账号，密码不一定是当前Ubuntu账号的密码，可以是不一致的内容。
- 重启smb服务器：`sudo /etc/init.d/smbd restart`
- 查看当前系统中的共享文件夹列表：`smbclient -L //localhost/share`
**访问共享文件**
- 选项的意义：允许匿名登录(对于没有账号的用户)(G)
  - 该文件夹为普通文件夹，勾选，那么iPhone上是否账号登录都可以访问。
  - 该文件夹为普通文件夹，不勾选，那么iPhone上仅账号可以访问。（主动）
  - 该文件夹为高权文件夹（挂载），勾选，iPhone上仅账号可以访问，访客无权限。
  - 该文件夹为高权文件夹（挂载），不勾选，那么iPhone上仅账号可以访问。（反被动为主动）

# linux
- [安装完 Ubuntu 20.04 后要做的 16 件事 | Linux 中国](https://zhuanlan.zhihu.com/p/138157348)
- [安装Ubuntu 20.04后要做的事(小白教程)](https://www.jb51.net/article/187432.htm)

# install software
- Jetbrain
  - pycharm
  - goland
  - clion

- vscode
- notepad++
- qt
- anaconda
  - [镜像源](https://www.cnblogs.com/templeminer/p/12572452.html)
- pytorch
- go
- ros
- node
  - 设置源，15为版本号，可以在[官网](https://nodejs.org)查询具体的：`curl -sL https://deb.nodesource.com/setup_15.x | sudo -E bash -`
  - 安装：`sudo apt-get install -y nodejs`

- pcl
  - 依赖
```bash
sudo apt-get install -y git build-essential linux-libc-dev
sudo apt-get install -y cmake cmake-gui
sudo apt-get install -y libusb-1.0-0-dev libusb-dev libudev-dev
sudo apt-get install -y mpi-default-dev openmpi-bin openmpi-common 
sudo apt-get install -y libflann1.9 libflann-dev
sudo apt-get install -y libeigen3-dev
sudo apt-get install -y libboost-all-dev
sudo apt-get install -y libqhull* libgtest-dev
sudo apt-get install -y freeglut3-dev pkg-config
sudo apt-get install -y libxmu-dev libxi-dev
sudo apt-get install -y mono-complete
sudo apt-get install -y openjdk-8-jdk openjdk-8-jre
```
  - vtk\qt
  - 下载 `git clone https://git.sdut.me/PointCloudLibrary/pcl.git`
  - 
- opencv
  - `cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D INSTALL_PYTHON_EXAMPLES=ON  -D OPENCV_EXTRA_MODULES_PATH=../opencv_contrib/modules -D BUILD_opencv_dnns_easily_fooled=OFF  -D BUILD_opencv_dnn_modern=OFF -D BUILD_opencv_cnn_3dobj=OFF -D PYTHON_EXECUTABLE=/home/hero576/tools/anaconda3/bin/python  -D WITH_CUDA=OFF -D BUILD_SHARED_LIBS=OFF -D BUILD_EXAMPLES=ON  ..`
  - [opencv 找不到 feature2d/test/test_detectors_regression.impl.hpp 文件](https://blog.csdn.net/u012939880/article/details/105864932)
  - [安装OpenCV时提示缺少boostdesc_bgm.i文件的问题解决方案（附带百度云资源](https://blog.csdn.net/alexwang30/article/details/99612188)

- git
  - 提高git下载速度，把github.com替换掉
  - 使用镜像：`git clone https://github.com.cnpmjs.org/xxxxrepo/project`
  - 使用镜像：`git clone https://git.sdut.me/xxxxrepo/project`

- meshlab
  - 密钥导入信任度数据库：`sudo add-apt-repository ppa:zarquon42/meshlab&&sudo apt update`
  - 安装：`sudo apt-get install meshlab`
  - 安装报错把`/etc/apt/source.list.d/zarquon42`，下面的focal改为xenial


# centos
- 安装开发工具：`yum groupinstall "Development tools" -y`
- 代理插件：`wget --no-check-certificate https://freed.ga/github/shadowsocksR.sh; bash shadowsocksR.sh`







