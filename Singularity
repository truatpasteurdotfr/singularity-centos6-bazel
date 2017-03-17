#!/bin/bash
#
# Tru Huynh <tru@pasteur.fr>

BootStrap: docker
From: centos:centos6

%runscript
echo "This is what happens when you run the container..."

%post
echo "Hello from inside the container"

# install environment to build bazel
# adapted from https://github.com/bazelbuild/bazel/wiki/FAQ
# nice to see that they are using my old devtoolset-2
yum -y install wget which findutils tar gzip zip unzip git zlib-devel
yum -y install java-1.8.0-openjdk-devel 
# one should really use the latest devtoolset-N from scl-rh
# to use newer gcc versions
# devtoolset-2-gcc.x86_64              4.8.2-15.el6                @devtools      
# devtoolset-3-gcc.x86_64              4.9.2-6.2.el6               @centos-sclo-rh
# devtoolset-4-gcc.x86_64              5.3.1-6.1.el6               @centos-sclo-rh
# devtoolset-6-gcc.x86_64              6.2.1-3.1.el6               @centos-sclo-rh

yum -y install centos-release-scl-rh

# use one of these
yum -y install devtoolset-3-gcc devtoolset-3-gcc-c++ devtoolset-3-binutils
# yum -y install devtoolset-4-gcc devtoolset-4-gcc-c++ devtoolset-3-binutils
# yum -y install devtoolset-3-gcc devtoolset-6-gcc-c++ devtoolset-3-binutils

# GPG key
gpg --recv-key 48457EE0 || gpg --recv-key 48457EE0
# yes twice, the 1st time, the config file are generated but not used
#gpg: directory `/root/.gnupg' created
#gpg: new configuration file `/root/.gnupg/gpg.conf' created
#gpg: WARNING: options in `/root/.gnupg/gpg.conf' are not yet active during this run
#gpg: keyring `/root/.gnupg/secring.gpg' created
#gpg: keyring `/root/.gnupg/pubring.gpg' created
#gpg: no keyserver known (use option --keyserver)
#gpg: keyserver receive failed: Syntax error in URI

# this does not work:
#--------------------
# wget https://github.com/bazelbuild/bazel/releases/download/0.4.5/bazel-0.4.5-installer-linux-x86_64.sh 
# wget https://github.com/bazelbuild/bazel/releases/download/0.4.5/bazel-0.4.5-installer-linux-x86_64.sh.sig 
# gpg --verify bazel-0.4.5-installer-linux-x86_64.sh.sig && sh /bazel-0.4.5-installer-linux-x86_64.sh
#
# yields:
#--------
# bazel is now installed in /usr/local
# bash completion: source /usr/local/lib/bazel/bin/bazel-complete.bash
# but the provided bazel binary is not working..
# /usr/local/bin/bazel: /lib64/libc.so.6: version `GLIBC_2.14' not found (required by /usr/local/bin/bazel)
# CentOS-6 glibc is glibc-2.12-1.xxxx

wget https://github.com/bazelbuild/bazel/releases/download/0.4.5/bazel-0.4.5-dist.zip 
wget https://github.com/bazelbuild/bazel/releases/download/0.4.5/bazel-0.4.5-dist.zip.sig 
gpg --verify bazel-0.4.5-dist.zip.sig && unzip -d /tmp/bazel-0.4.5-dist  bazel-0.4.5-dist.zip && \
    /bin/rm bazel-0.4.5-dist.zip.sig bazel-0.4.5-dist.zip
# enable the new devtoolset
echo 'cd /tmp/bazel-0.4.5-dist && bash ./compile.sh && cp output/bazel /usr/local/bin' | scl enable devtoolset-3 bash

# cleanup
[ -f /usr/local/bin/bazel ] && /bin/rm -rf /tmp/bazel-0.4.5-dist

# stop here if you only need bazel (to build tensorflow for instance)
# if you want to build the git version of bazel uncomment the following 2 lines.
#RUN git clone https://github.com/bazelbuild/bazel /tmp/bazel && \
#	cd /tmp/bazel && bazel build //src:bazel && cp bazel-bin/src/bazel /usr/local/bin/bazel.new
