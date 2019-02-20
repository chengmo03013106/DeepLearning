# DeepLearning
跟风儿学习


# 包，源码下载：

# 搭建环境：

## ************* yum 部分：
CentOS 7以上 环境搭建
gcc 4.8.5
yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
yum install protobuf-devel leveldb-devel snappy-devel boost-devel hdf5-devel
暂时不用安装 opencv-devel
yum remove opencv-devel*
pkg-config --modversion opencv
yum install gflags-devel glog-devel lmdb-devel


## ************* OpenBLAS

## ************* Boost >= 1.55，自己使用的是1.69 *************************

Boost >= 1.55
用rpm -qa boost 查看CentOS的boost版本
boost-1.53.0-27.el7.x86_64
删除Linux自带的boost
rpm -e boost-devel-1.53.0-27.el7.x86_64 （删除依赖）
rpm -e boost-1.53.0-27.el7.x86_64
[root@bogon my_build_dir]# rpm -qa boost （只是查看安装包的版本）
boost-1.53.0-27.el7.x86_64
==========================
1.下载 boost
2. 将文件解压在/usr/local/目录下
tar -xzvf boost_1_68_0.tar.gz
3. 进入/usr/local/boost/ 目录， 在terminal中输入
./bootstrap.sh
4.
./b2 install --prefix=/usr/local/ （暂时用这个，将boost安装到路径下面的lib和include）
5.
添加环境变量（刚改完要重启或者注销一下来更新刚修改过的环境变量）
两种方法：（1）修改/etc/profie文件 末尾添加
export BOOST_INCLUDE=/usr/local/include/boost
export BOOST_LIB=/usr/local/lib
source /etc/profie
如何用程序调用boost，输出版本？？？？？
验证boost的版本是否满足>=1.55(暂时查不到)

在编译boost的时候需要注意，加上额外对python的支持，从而生成libboost_python3.a/ liboost_python3.so

首先，配置boost的时候执行：
bash bootstrap.sh --with-python=/usr/local/bin/python3.7 --with-python-root= --with-python-version=3.7，我用python3的版本是3.7.2
注意！ 这里的 --with-python=选项，给定的是绝对路径，而不是执行路径。
然后需要执行：
export CPLUS_INCLUDE_PATH="$CPLUS_INCLUDE_PATH:/usr/local/include/python3.7m/"
从而使编译环境可以知道python3安装路径，否则我一直报错:

/usr/local/include/python3.7m/

## ************* caffe
required opencv3

下载：git clone git://github.com/BVLC/caffe.git
安装：
cd caffe
cp Makefile.config.example Makefile.config
# Adjust Makefile.config (for example, if using Anaconda Python, or if cuDNN isdesired)
注意：首先配置Makefile.config文件！

vi /etc/profile
LD_LIBRARY_PATH=/opt/OpenBLAS/lib:/usr/local/lib64
export LD_LIBRARY_PATH
source /etc/profile

vi ~/.bashrc
export LD_LIBRARY_PATH="/root/anaconda3/lib:$LD_LIBRARY_PATH"
source ~/.bashrc

cp /root/anaconda3/lib/libicui18n.so.58 /usr/lib
make clean
make all

make pycaffe

配置环境变量
vi /etc/profile
LD_LIBRARY_PATH=/opt/OpenBLAS/lib:/usr/local/lib64:/usr/local/lib
source /etc/profile

Python路径
vi ~/.bashrc
export PYTHONPATH=/opt/installFile/caffe/python:$PYTHONPATH
export PYTHONPATH=/usr/include:$PYTHONPATH
source ~/.bashrc
验证：
python命令行执行：import google.protobuf，不报错

## ************* opencv3.4.3
cmake -D CMAKE_EXE_LINKER_FLAGS="-std=c++11" .

如果不加 -std=c++11，编译会出现如下错误：
/home/chengmo/anaconda3/lib/libicui18n.so.58: undefined reference to `__cxa_throw_bad_array_new_length@CXXABI_1.3.8'

yum install libpng-devel
yum install gtk2-devel

缺什么，装什么，一路make
make install，否则 make caffe的时候，有问题


