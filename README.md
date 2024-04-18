我的环境是WSL2装的Ubuntu22-04

下载最新的版本v8.0.32.25,修改源码

~/GreatSQL/storage/perfschema/pfs_dd_version.h:236  static const uint PFS_DD_VERSION = 80032;  <-- 80025

~/GreatSQL/sql/dd/info_schema/metadata.h:241        static const uint IS_DD_VERSION = 80030;   <-- 80023

1 从github上下载源码并解压, git clone 使用https协议握手不成功，我重新配置了ssh协议，比较方便,开始以为是下载的代码和clone的不太一样，后面发现是一样的，可以直接下载更省事 (就无需下面配置ssh的步骤)
    
    ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
    
    cat ~/.ssh/id_rsa.pub 复制公钥贴到自己github账号中的ssh-key里面(新建一个)
    
    eval "$(ssh-agent -s)"

    ssh-add ~/.ssh/id_rsa

    git config --global user.name "Your Name"
    
    git config --global user.email "your_email@example.com"



2 没有cmake 安装 sudo apt install cmake

3 安装libjemalloc库 sudo apt-get install libjemalloc-dev

4 安装LDAP开发库 sudo apt install libldap2-dev

手动下载两个关联repo

5 ~/GreatSQL/extra$ rm -rf ./libkmip/
 
  ~/GreatSQL/extra$ git clone --depth=1 git@github.com:Percona-Lab/libkmip.git
  
6 ~/GreatSQL/extra$ rm -rf ./coredumper/
  
  ~/GreatSQL/extra$ git clone --depth=1 git@github.com:Percona-Lab/coredumper.git
  
```
2 $ cd greatsql-8.0.25-16
  $ mkdir -p build
  $ cd build

sudo cmake -DLDAP_LIBRARY="/usr/lib/x86_64-linux-gnu/libldap.so" -DLDAP_INCLUDE_DIR="/usr/include" .. \
-DDOWNLOAD_BOOST=1 \
-DWITH_BOOST=/opt/boost \
-DCMAKE_INSTALL_PREFIX=/usr/local/GreatSQL-8.0.25-16-Linux-glibc2.28-x86_64 \
-DWITH_ZLIB=bundled \
-DWITH_NUMA=ON \
-DCMAKE_EXE_LINKER_FLAGS="-ljemalloc" \
-DBUILD_CONFIG=mysql_release \
-DWITH_TOKUDB=OFF \
-DWITH_ROCKSDB=OFF \
-DMAJOR_VERSION=8 \
-DMINOR_VERSION=0 \
-DPATCH_VERSION=25 \
-DWITH_UNIT_TESTS=OFF \
-DWITH_NDBCLUSTER=OFF \
-DWITH_SSL=system \
-DWITH_SYSTEMD=ON \
-DWITH_LDAP=OFF \
-DWITH_AUTHENTICATION_LDAP=OFF \
-DWITH_DEBUG=1 \
-DCMAKE_BUILD_TYPE=Debug \
&& sudo make -j2 VERBOSE=1 && sudo make install

```
