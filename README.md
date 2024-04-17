1 从github上下载源码并解压

2 没有cmake 安装 cmake

3 安装libjemalloc库 sudo apt-get install libjemalloc-dev

4 安装LDAP开发库 sudo apt install libldap2-dev

```
2 $ cd greatsql-8.0.25-16
  $ mkdir -p build
  $ cd build

sudo cmake .. \
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
&& make -j2 VERBOSE=1 && make install

```
