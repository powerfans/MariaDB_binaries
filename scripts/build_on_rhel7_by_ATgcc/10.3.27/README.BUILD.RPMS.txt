Build on K1 Power9 Linux, RedHat 7.6 (Kernel 4.14.0-115.8.1.el7a.ppc64le) with advance-toolchain-at11.0

### 1. About Build ENV #########################################################################################

# lscpu |grep -i arch
Architecture:          ppc64le
Model name:            POWER9 (architected), altivec supported

# uname -r
4.14.0-115.8.1.el7a.ppc64le

# uname -m
ppc64le

### 2. Build RPMS for mariadb ####################################################################################

Install dependencies
# yum -y install bison boost-devel coreutils checkpolicy binutils cmake gcc-c++ gcc glibc-devel make \
       libcurl-devel ncurses-devel systemtap-sdt-devel libevent-devel glibc-common git groff-base tar \
       jemalloc-devel libaio-devel cracklib-devel zlib-devel xz-devel systemd-devel java-1.8.0-openjdk \
       java-1.8.0-openjdk-headless Judy-devel krb5-devel libxml2-devel libxml2 unixODBC-devel unixODBC \
       openssl-devel pam-devel pkgconfig readline-devel ruby policycoreutils-python thrift-devel \
       libzstd-devel snappy-devel numactl-devel
# ## yum install nmon perf dstat #useful tools for performance monitoring

Install advance-toolchain-at11.0
# yum install advance-toolchain-at11.0
# export PATH=/opt/at11.0/bin:$PATH
# type gcc
gcc is /opt/at11.0/bin/gcc
[root@db1 src]# gcc --version
gcc (GCC) 7.4.1 20191016 (Advance-Toolchain-at11.0) [revision 277075]


### Default %{optflags} is not optimized.
# rpm -Uvh MariaDB-10.3.27-1.el7.centos.src.rpm
# ##
# ## ls -la mariadb-10.3.27.tar.gz
# ## -rw-r--r-- 1 root root 72875692 Jan 11 11:06 mariadb-10.3.27.tar.gz
# ## ls -la /root/rpmbuild/SOURCES/MariaDB-10.3.27.tar.gz 
# ## -rw-r--r-- 1 root root 72663348 Nov 10 18:03 /root/rpmbuild/SOURCES/MariaDB-10.3.27.tar.gz
# ## replace mariadb-10.3.27.tar.gz with tar.gz source
# rm -f /root/rpmbuild/SOURCES/MariaDB-10.3.27.tar.gz
# cp mariadb-10.3.27.tar.gz /root/rpmbuild/SOURCES/MariaDB-10.3.27.tar.gz
# cd /root/rpmbuild/SPECS
# sed -i 's/\-DRPM=centos74/\-DMARIADB_MACHINE_TYPE=ppc64le \-DRPM=el7 \-DWITH_READLINE=ON \-DENABLED_LOCAL_INFILE=ON \-DWITH_EMBEDDED_SERVER=ON \-DWITH_SSL=system \-DWITH_ZLIB=system \-DWITH_PCRE=bundle \-DPLUGIN_PERFSCHEMA=YES \-DWITH_JEMALLOC=system \-DWITH_NUMA=ON \-DCMAKE_C_FLAGS="\-O3 \-mcpu=native \-mtune=native \-mcmodel=large" \-DCMAKE_CXX_FLAGS="\-O3 \-mcpu=native \-mtune=native \-mcmodel=large" /g' MariaDB.spec
# sed -i 's/^make/make VERBOSE=1/g' MariaDB.spec
# rpmbuild -v -bb MariaDB.spec 2>&1 | tee build.log

