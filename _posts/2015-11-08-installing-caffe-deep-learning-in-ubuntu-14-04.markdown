---
layout: post
title: "Installing Caffe - Deep Learning in Ubuntu 14.04"
modified:
categories: 
excerpt:
tags: []
image:
  feature:
date: 2015-11-08T11:19:37+05:30
---
Caffe is a deep learning framework made by Berkely Vision and Learning Center(BVLC).The project is created by Yangqing Jia created the project during his PhD at UC Berkeley. Caffe is released under the BSD 2-Clause license.

Caffe is apure C++/CUDA architecture for deep learning with command line,Python & MATLAB interfaces.

Here I am going explain how we can install caffe in a personal laptop without CUDA support. I am using Inter INSPIRON dual core CPU with no spectial graphics card.

NB: It is better to have a freshly installed Ubuntu 14.04. Also install OpenCV latest version. You can follow the following tutorial to install OpenCV ihttp://www.samontab.com/web/2014/06/installing-opencv-2-4-9-in-ubuntu-14-04-lts/


First clone the latest release of caffe from its official repositary.

git clone https://github.com/BVLC/caffe.git
#add these following lines in the end of your .bashrc file
export PYTHONPATH=$PYTHONPATH:$HOME/caffe/python
export PYTHONPATH=$PYTHONPATH:$HOME/caffe/python/caffe


Install the python packages required for pycaffe.Install Anaconda python distribution since it install all necessary python wrappers required for the caffe

Download latest Anaconda distribution from https://www.continuum.io/downloads
.And run the following commands to add path in ~/.bashrc


bash Anaconda-2.1.0-Linux-x86_64.sh
#add the following line at the end to your .bashrc
export PATH=/home/suryateja/anaconda/bin:$PATH

Or install all python dependencies from dependencies file is in caffe/python

cd python/
for req in $(cat requirements.txt); do sudo pip install $req; done

install hdf5 via apt-get

sudo apt-get install libhdf5-serial-dev

Download and install gflags on your system. gflags or formerly google command line flags package contains a C++ library that implements command line flags for processing.

wget https://github.com/schuhschuh/gflags/archive/master.zip
unzip master.zip
cd gflags-master
mkdir build && cd build
export CXXFLAGS="-fPIC" && cmake .. && make VERBOSE=1</pre>
<pre>make
sudo make install

Download google-glog and install. You can also install through apt-get. And install snappy.


tar -xvzf google-glog_0.3.3.orig.tar.gz
cd glog-0.3.3/
./configure
make
sudo make install
agi libgoogle-glog-dev
agi libsnappy-dev

Get leveldb onto your machine and lmdb with its dependencies.

git clone https://code.google.com/p/leveldb/
cd leveldb
make
cp --preserve=links libleveldb.* /usr/local/lib
sudo cp --preserve=links libleveldb.* /usr/local/lib
sudo cp -r include/leveldb /usr/local/include/
ldconfig
sudo ldconfig
sudo apt-get install libffi-dev python-dev build-essential
sudo apt-get install liblmdb-dev


Download and install Atlas and Lapack. and Change the scaling for all of your CPU cores to replace the single word in the file with ‘performance’.

gksu emacs /sys/devices/system/cpu/cpu0/cpufreq/scaling_governor
gksu emacs /sys/devices/system/cpu/cpu1/cpufreq/scaling_governor
gksu emacs /sys/devices/system/cpu/cpu2/cpufreq/scaling_governor
gksu emacs /sys/devices/system/cpu/cpu3/cpufreq/scaling_governor
 
bunzip2 -c atlas3.10.2.tar.bz2 | tar xfm -
mv ATLAS ATLAS3.10.2
cd ATLAS3.10.2
mkdir Linux_C2D64SSE3
cd Linux_C2D64SSE3
../configure -b 64 -D c -DPentiumCPS=2400 --prefix=/home/suryateja/lib/atlas --with-netlib-lapack-tarfile=/home/suryateja/Downloads/lapack-3.5.0.tgz
 
make build
make check
make ptcheck
make time
sudo make install



Now uncomment the CPU_ONLY := 1 in Makefile.config (Keep a backup of Makefile ) 

cd caffe
cp Makefile.config.example Makefile.config
make all
make test
make runtest


If you are lucky the caffe is installed now.
NB: If some libraries are not installing from command prompt try to install it through synaptic package manager.




