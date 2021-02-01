Build on K1 Power9 Linux, RedHat 7.6 (Kernel 4.14.0-115.8.1.el7a.ppc64le) with advance-toolchain-at11.0

### 1. About Build ENV #########################################################################################

# lscpu |grep -i arch
Architecture:          ppc64le
Model name:            POWER9 (architected), altivec supported

# uname -r
4.14.0-115.8.1.el7a.ppc64le

# uname -m
ppc64le

### 2. Build mysql    ##########################################################################################

Install dependencies
# yum -y install bison boost-devel coreutils checkpolicy binutils cmake gcc-c++ gcc glibc-devel make \
       libcurl-devel ncurses-devel systemtap-sdt-devel libevent-devel glibc-common git groff-base tar \
       jemalloc-devel libaio-devel cracklib-devel zlib-devel xz-devel systemd-devel java-1.8.0-openjdk \
       java-1.8.0-openjdk-headless Judy-devel krb5-devel libxml2-devel libxml2 unixODBC-devel unixODBC \
       openssl-devel pam-devel pkgconfig readline-devel ruby policycoreutils-python thrift-devel \
       libzstd-devel snappy-devel numactl-devel pcre2 pcre2-devel

# ## yum install nmon perf dstat #useful tools for performance monitoring

Install advance-toolchain-at11.0
# yum install advance-toolchain-at11.0
# export PATH=/opt/at11.0/bin:$PATH
# type gcc
gcc is /opt/at11.0/bin/gcc
[root@db1 src]# gcc --version
gcc (GCC) 7.4.1 20191016 (Advance-Toolchain-at11.0) [revision 277075]

# tar zxf mariadb-10.5.8.tar.gz
# cd mariadb-10.5.8/
# mkdir -p build; cd build
# ##cmake -LAH ..  
# cmake .. \
  -DBUILD_CONFIG=mysql_release \
  -DCMAKE_BUILD_TYPE=RelWithDebInfo \
  -DCMAKE_C_COMPILER=`which gcc` \
  -DCMAKE_C_FLAGS="-O3 -mcpu=native -mtune=native -mcmodel=large" \
  -DCMAKE_CXX_COMPILER=`which g++` \
  -DCMAKE_CXX_FLAGS="-O3 -mcpu=native -mtune=native -mcmodel=large" \
  -DCMAKE_INSTALL_PREFIX=/opt/mariadb_at/10.5.8 \
  -DCMAKE_LINKER=`which gcc` \
  -DCMAKE_AR=`which gcc-ar` \
  -DCMAKE_NM=`which gcc-nm` \
  -DCMAKE_RANLIB=`which gcc-ranlib` \
  -DWITH_READLINE=ON \
  -DENABLED_LOCAL_INFILE=ON \
  -DWITH_EMBEDDED_SERVER=ON \
  -DWITH_SSL=system \
  -DWITH_ZLIB=system \
  -DWITH_PCRE=bundle \
  -DPLUGIN_PERFSCHEMA=YES \
  -DWITH_JEMALLOC=system \
  -DWITH_NUMA=ON \
2>&1 | tee config.log

# make -j 32 VERBOSE=1 && make install


# cd /opt/mariadb_at && tar zcf MariaDB-server-10.5.8-1.el7.ppc64le.at11gcc.bin.tar.gz ./10.5.8
