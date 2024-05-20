# redhat7.2_安装GPFS4.2.0.3

## 一、环境配置

### 1、系统配置信息

| hostname | 系统版本   | IP地址          | GPFS版本 | 数据盘   |
| -------- | :--------- | --------------- | -------- | -------- |
| gpfs-01  | redhat-7.2 | 192.168.196.150 | 4.2.0.3  | /dev/sdb |
| gpfs-02  | redhat-7.2 | 192.168.196.160 | 4.2.0.3  | /dev/sdb |

### 2、/etc/hosts配置

```
[root@gpfs-01 ~]# cat /etc/hosts
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6

192.168.196.150 gpfs-01
192.168.196.160 gpfs-02
```

```
[root@gpfs-02 ~]# cat /etc/hosts
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6

192.168.196.150 gpfs-01
192.168.196.160 gpfs-02
```

### 3、安装依赖包

```
yum install gcc cpp automake binutils make ksh gcc-c++ rpm-build -y
```

```
[root@gpfs-01 ~]# yum install gcc cpp automake binutils make ksh gcc-c++ rpm-build -y
Loaded plugins: product-id, search-disabled-repos, subscription-manager
This system is not registered to Red Hat Subscription Management. You can use subscription-manager to register.
Package binutils-2.23.52.0.1-55.el7.x86_64 already installed and latest version
Package 1:make-3.82-21.el7.x86_64 already installed and latest version
Resolving Dependencies
--> Running transaction check
---> Package automake.noarch 0:1.13.4-3.el7 will be installed
--> Processing Dependency: autoconf >= 2.65 for package: automake-1.13.4-3.el7.noarch
--> Processing Dependency: perl >= 5.006 for package: automake-1.13.4-3.el7.noarch
--> Processing Dependency: /usr/bin/perl for package: automake-1.13.4-3.el7.noarch
--> Processing Dependency: perl(Carp) for package: automake-1.13.4-3.el7.noarch
--> Processing Dependency: perl(Class::Struct) for package: automake-1.13.4-3.el7.noarch
--> Processing Dependency: perl(DynaLoader) for package: automake-1.13.4-3.el7.noarch
--> Processing Dependency: perl(Errno) for package: automake-1.13.4-3.el7.noarch
--> Processing Dependency: perl(Exporter) for package: automake-1.13.4-3.el7.noarch
--> Processing Dependency: perl(File::Basename) for package: automake-1.13.4-3.el7.noarch
--> Processing Dependency: perl(File::Compare) for package: automake-1.13.4-3.el7.noarch
--> Processing Dependency: perl(File::Copy) for package: automake-1.13.4-3.el7.noarch
--> Processing Dependency: perl(File::Path) for package: automake-1.13.4-3.el7.noarch
--> Processing Dependency: perl(File::Spec) for package: automake-1.13.4-3.el7.noarch
--> Processing Dependency: perl(File::stat) for package: automake-1.13.4-3.el7.noarch
--> Processing Dependency: perl(Getopt::Long) for package: automake-1.13.4-3.el7.noarch
--> Processing Dependency: perl(IO::File) for package: automake-1.13.4-3.el7.noarch
--> Processing Dependency: perl(POSIX) for package: automake-1.13.4-3.el7.noarch
--> Processing Dependency: perl(TAP::Parser) for package: automake-1.13.4-3.el7.noarch
--> Processing Dependency: perl(Thread::Queue) for package: automake-1.13.4-3.el7.noarch
--> Processing Dependency: perl(constant) for package: automake-1.13.4-3.el7.noarch
--> Processing Dependency: perl(strict) for package: automake-1.13.4-3.el7.noarch
--> Processing Dependency: perl(threads) for package: automake-1.13.4-3.el7.noarch
--> Processing Dependency: perl(vars) for package: automake-1.13.4-3.el7.noarch
--> Processing Dependency: perl(warnings) for package: automake-1.13.4-3.el7.noarch
---> Package cpp.x86_64 0:4.8.5-4.el7 will be installed
--> Processing Dependency: libmpc.so.3()(64bit) for package: cpp-4.8.5-4.el7.x86_64
--> Processing Dependency: libmpfr.so.4()(64bit) for package: cpp-4.8.5-4.el7.x86_64
---> Package gcc.x86_64 0:4.8.5-4.el7 will be installed
--> Processing Dependency: glibc-devel >= 2.2.90-12 for package: gcc-4.8.5-4.el7.x86_64
---> Package gcc-c++.x86_64 0:4.8.5-4.el7 will be installed
--> Processing Dependency: libstdc++-devel = 4.8.5-4.el7 for package: gcc-c++-4.8.5-4.el7.x86_64
---> Package ksh.x86_64 0:20120801-22.el7_1.2 will be installed
---> Package rpm-build.x86_64 0:4.11.3-17.el7 will be installed
--> Processing Dependency: elfutils >= 0.128 for package: rpm-build-4.11.3-17.el7.x86_64
--> Processing Dependency: patch >= 2.5 for package: rpm-build-4.11.3-17.el7.x86_64
--> Processing Dependency: /usr/bin/gdb-add-index for package: rpm-build-4.11.3-17.el7.x86_64
--> Processing Dependency: bzip2 for package: rpm-build-4.11.3-17.el7.x86_64
--> Processing Dependency: perl(File::Temp) for package: rpm-build-4.11.3-17.el7.x86_64
--> Processing Dependency: system-rpm-config for package: rpm-build-4.11.3-17.el7.x86_64
--> Processing Dependency: unzip for package: rpm-build-4.11.3-17.el7.x86_64
--> Running transaction check
---> Package autoconf.noarch 0:2.69-11.el7 will be installed
--> Processing Dependency: m4 >= 1.4.14 for package: autoconf-2.69-11.el7.noarch
--> Processing Dependency: perl(Data::Dumper) for package: autoconf-2.69-11.el7.noarch
--> Processing Dependency: perl(Text::ParseWords) for package: autoconf-2.69-11.el7.noarch
---> Package bzip2.x86_64 0:1.0.6-13.el7 will be installed
---> Package elfutils.x86_64 0:0.163-3.el7 will be installed
---> Package gdb.x86_64 0:7.6.1-80.el7 will be installed
---> Package glibc-devel.x86_64 0:2.17-105.el7 will be installed
--> Processing Dependency: glibc-headers = 2.17-105.el7 for package: glibc-devel-2.17-105.el7.x86_64
--> Processing Dependency: glibc-headers for package: glibc-devel-2.17-105.el7.x86_64
---> Package libmpc.x86_64 0:1.0.1-3.el7 will be installed
---> Package libstdc++-devel.x86_64 0:4.8.5-4.el7 will be installed
---> Package mpfr.x86_64 0:3.1.1-4.el7 will be installed
---> Package patch.x86_64 0:2.7.1-8.el7 will be installed
---> Package perl.x86_64 4:5.16.3-286.el7 will be installed
--> Processing Dependency: perl-libs = 4:5.16.3-286.el7 for package: 4:perl-5.16.3-286.el7.x86_64
--> Processing Dependency: perl(Scalar::Util) >= 1.10 for package: 4:perl-5.16.3-286.el7.x86_64
--> Processing Dependency: perl(Socket) >= 1.3 for package: 4:perl-5.16.3-286.el7.x86_64
--> Processing Dependency: perl(Filter::Util::Call) for package: 4:perl-5.16.3-286.el7.x86_64
--> Processing Dependency: perl(Pod::Simple::Search) for package: 4:perl-5.16.3-286.el7.x86_64
--> Processing Dependency: perl(Pod::Simple::XHTML) for package: 4:perl-5.16.3-286.el7.x86_64
--> Processing Dependency: perl(Scalar::Util) for package: 4:perl-5.16.3-286.el7.x86_64
--> Processing Dependency: perl(Socket) for package: 4:perl-5.16.3-286.el7.x86_64
--> Processing Dependency: perl(Storable) for package: 4:perl-5.16.3-286.el7.x86_64
--> Processing Dependency: perl(Time::HiRes) for package: 4:perl-5.16.3-286.el7.x86_64
--> Processing Dependency: perl(Time::Local) for package: 4:perl-5.16.3-286.el7.x86_64
--> Processing Dependency: perl(threads::shared) for package: 4:perl-5.16.3-286.el7.x86_64
--> Processing Dependency: perl-libs for package: 4:perl-5.16.3-286.el7.x86_64
--> Processing Dependency: perl-macros for package: 4:perl-5.16.3-286.el7.x86_64
--> Processing Dependency: libperl.so()(64bit) for package: 4:perl-5.16.3-286.el7.x86_64
---> Package perl-Carp.noarch 0:1.26-244.el7 will be installed
---> Package perl-Exporter.noarch 0:5.68-3.el7 will be installed
---> Package perl-File-Path.noarch 0:2.09-2.el7 will be installed
---> Package perl-File-Temp.noarch 0:0.23.01-3.el7 will be installed
---> Package perl-Getopt-Long.noarch 0:2.40-2.el7 will be installed
--> Processing Dependency: perl(Pod::Usage) >= 1.14 for package: perl-Getopt-Long-2.40-2.el7.noarch
---> Package perl-PathTools.x86_64 0:3.40-5.el7 will be installed
---> Package perl-Test-Harness.noarch 0:3.28-3.el7 will be installed
---> Package perl-Thread-Queue.noarch 0:3.02-2.el7 will be installed
---> Package perl-constant.noarch 0:1.27-2.el7 will be installed
---> Package perl-threads.x86_64 0:1.87-4.el7 will be installed
---> Package redhat-rpm-config.noarch 0:9.1.0-68.el7 will be installed
--> Processing Dependency: dwz >= 0.4 for package: redhat-rpm-config-9.1.0-68.el7.noarch
--> Processing Dependency: perl-srpm-macros for package: redhat-rpm-config-9.1.0-68.el7.noarch
--> Processing Dependency: zip for package: redhat-rpm-config-9.1.0-68.el7.noarch
---> Package unzip.x86_64 0:6.0-15.el7 will be installed
--> Running transaction check
---> Package dwz.x86_64 0:0.11-3.el7 will be installed
---> Package glibc-headers.x86_64 0:2.17-105.el7 will be installed
--> Processing Dependency: kernel-headers >= 2.2.1 for package: glibc-headers-2.17-105.el7.x86_64
--> Processing Dependency: kernel-headers for package: glibc-headers-2.17-105.el7.x86_64
---> Package m4.x86_64 0:1.4.16-10.el7 will be installed
---> Package perl-Data-Dumper.x86_64 0:2.145-3.el7 will be installed
---> Package perl-Filter.x86_64 0:1.49-3.el7 will be installed
---> Package perl-Pod-Simple.noarch 1:3.28-4.el7 will be installed
--> Processing Dependency: perl(Pod::Escapes) >= 1.04 for package: 1:perl-Pod-Simple-3.28-4.el7.noarch
--> Processing Dependency: perl(Encode) for package: 1:perl-Pod-Simple-3.28-4.el7.noarch
---> Package perl-Pod-Usage.noarch 0:1.63-3.el7 will be installed
--> Processing Dependency: perl(Pod::Text) >= 3.15 for package: perl-Pod-Usage-1.63-3.el7.noarch
--> Processing Dependency: perl-Pod-Perldoc for package: perl-Pod-Usage-1.63-3.el7.noarch
---> Package perl-Scalar-List-Utils.x86_64 0:1.27-248.el7 will be installed
---> Package perl-Socket.x86_64 0:2.010-3.el7 will be installed
---> Package perl-Storable.x86_64 0:2.45-3.el7 will be installed
---> Package perl-Text-ParseWords.noarch 0:3.29-4.el7 will be installed
---> Package perl-Time-HiRes.x86_64 4:1.9725-3.el7 will be installed
---> Package perl-Time-Local.noarch 0:1.2300-2.el7 will be installed
---> Package perl-libs.x86_64 4:5.16.3-286.el7 will be installed
---> Package perl-macros.x86_64 4:5.16.3-286.el7 will be installed
---> Package perl-srpm-macros.noarch 0:1-8.el7 will be installed
---> Package perl-threads-shared.x86_64 0:1.43-6.el7 will be installed
---> Package zip.x86_64 0:3.0-10.el7 will be installed
--> Running transaction check
---> Package kernel-headers.x86_64 0:3.10.0-327.el7 will be installed
---> Package perl-Encode.x86_64 0:2.51-7.el7 will be installed
---> Package perl-Pod-Escapes.noarch 1:1.04-286.el7 will be installed
---> Package perl-Pod-Perldoc.noarch 0:3.20-4.el7 will be installed
--> Processing Dependency: perl(HTTP::Tiny) for package: perl-Pod-Perldoc-3.20-4.el7.noarch
--> Processing Dependency: perl(parent) for package: perl-Pod-Perldoc-3.20-4.el7.noarch
---> Package perl-podlators.noarch 0:2.5.1-3.el7 will be installed
--> Running transaction check
---> Package perl-HTTP-Tiny.noarch 0:0.033-3.el7 will be installed
---> Package perl-parent.noarch 1:0.225-244.el7 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

===================================================================================================================================================================
 Package                                         Arch                            Version                                      Repository                      Size
===================================================================================================================================================================
Installing:
 automake                                        noarch                          1.13.4-3.el7                                 local                          679 k
 cpp                                             x86_64                          4.8.5-4.el7                                  local                          5.9 M
 gcc                                             x86_64                          4.8.5-4.el7                                  local                           16 M
 gcc-c++                                         x86_64                          4.8.5-4.el7                                  local                          7.2 M
 ksh                                             x86_64                          20120801-22.el7_1.2                          local                          880 k
 rpm-build                                       x86_64                          4.11.3-17.el7                                local                          144 k
Installing for dependencies:
 autoconf                                        noarch                          2.69-11.el7                                  local                          701 k
 bzip2                                           x86_64                          1.0.6-13.el7                                 local                           52 k
 dwz                                             x86_64                          0.11-3.el7                                   local                           99 k
 elfutils                                        x86_64                          0.163-3.el7                                  local                          268 k
 gdb                                             x86_64                          7.6.1-80.el7                                 local                          2.4 M
 glibc-devel                                     x86_64                          2.17-105.el7                                 local                          1.0 M
 glibc-headers                                   x86_64                          2.17-105.el7                                 local                          661 k
 kernel-headers                                  x86_64                          3.10.0-327.el7                               local                          3.2 M
 libmpc                                          x86_64                          1.0.1-3.el7                                  local                           51 k
 libstdc++-devel                                 x86_64                          4.8.5-4.el7                                  local                          1.5 M
 m4                                              x86_64                          1.4.16-10.el7                                local                          256 k
 mpfr                                            x86_64                          3.1.1-4.el7                                  local                          203 k
 patch                                           x86_64                          2.7.1-8.el7                                  local                          110 k
 perl                                            x86_64                          4:5.16.3-286.el7                             local                          8.0 M
 perl-Carp                                       noarch                          1.26-244.el7                                 local                           19 k
 perl-Data-Dumper                                x86_64                          2.145-3.el7                                  local                           47 k
 perl-Encode                                     x86_64                          2.51-7.el7                                   local                          1.5 M
 perl-Exporter                                   noarch                          5.68-3.el7                                   local                           28 k
 perl-File-Path                                  noarch                          2.09-2.el7                                   local                           27 k
 perl-File-Temp                                  noarch                          0.23.01-3.el7                                local                           56 k
 perl-Filter                                     x86_64                          1.49-3.el7                                   local                           76 k
 perl-Getopt-Long                                noarch                          2.40-2.el7                                   local                           56 k
 perl-HTTP-Tiny                                  noarch                          0.033-3.el7                                  local                           38 k
 perl-PathTools                                  x86_64                          3.40-5.el7                                   local                           83 k
 perl-Pod-Escapes                                noarch                          1:1.04-286.el7                               local                           50 k
 perl-Pod-Perldoc                                noarch                          3.20-4.el7                                   local                           87 k
 perl-Pod-Simple                                 noarch                          1:3.28-4.el7                                 local                          216 k
 perl-Pod-Usage                                  noarch                          1.63-3.el7                                   local                           27 k
 perl-Scalar-List-Utils                          x86_64                          1.27-248.el7                                 local                           36 k
 perl-Socket                                     x86_64                          2.010-3.el7                                  local                           49 k
 perl-Storable                                   x86_64                          2.45-3.el7                                   local                           77 k
 perl-Test-Harness                               noarch                          3.28-3.el7                                   local                          302 k
 perl-Text-ParseWords                            noarch                          3.29-4.el7                                   local                           14 k
 perl-Thread-Queue                               noarch                          3.02-2.el7                                   local                           17 k
 perl-Time-HiRes                                 x86_64                          4:1.9725-3.el7                               local                           45 k
 perl-Time-Local                                 noarch                          1.2300-2.el7                                 local                           24 k
 perl-constant                                   noarch                          1.27-2.el7                                   local                           19 k
 perl-libs                                       x86_64                          4:5.16.3-286.el7                             local                          687 k
 perl-macros                                     x86_64                          4:5.16.3-286.el7                             local                           43 k
 perl-parent                                     noarch                          1:0.225-244.el7                              local                           12 k
 perl-podlators                                  noarch                          2.5.1-3.el7                                  local                          112 k
 perl-srpm-macros                                noarch                          1-8.el7                                      local                          4.7 k
 perl-threads                                    x86_64                          1.87-4.el7                                   local                           49 k
 perl-threads-shared                             x86_64                          1.43-6.el7                                   local                           39 k
 redhat-rpm-config                               noarch                          9.1.0-68.el7                                 local                           77 k
 unzip                                           x86_64                          6.0-15.el7                                   local                          166 k
 zip                                             x86_64                          3.0-10.el7                                   local                          260 k

Transaction Summary
===================================================================================================================================================================
Install  6 Packages (+47 Dependent packages)

Total download size: 54 M
Installed size: 138 M
Downloading packages:
-------------------------------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                                              162 MB/s |  54 MB  00:00:00     
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : mpfr-3.1.1-4.el7.x86_64                                                                                                                        1/53 
  Installing : libmpc-1.0.1-3.el7.x86_64                                                                                                                      2/53 
  Installing : cpp-4.8.5-4.el7.x86_64                                                                                                                         3/53 
  Installing : 1:perl-parent-0.225-244.el7.noarch                                                                                                             4/53 
  Installing : perl-HTTP-Tiny-0.033-3.el7.noarch                                                                                                              5/53 
  Installing : perl-podlators-2.5.1-3.el7.noarch                                                                                                              6/53 
  Installing : perl-Pod-Perldoc-3.20-4.el7.noarch                                                                                                             7/53 
  Installing : 1:perl-Pod-Escapes-1.04-286.el7.noarch                                                                                                         8/53 
  Installing : perl-Text-ParseWords-3.29-4.el7.noarch                                                                                                         9/53 
  Installing : perl-Encode-2.51-7.el7.x86_64                                                                                                                 10/53 
  Installing : perl-Pod-Usage-1.63-3.el7.noarch                                                                                                              11/53 
  Installing : 4:perl-libs-5.16.3-286.el7.x86_64                                                                                                             12/53 
  Installing : 4:perl-macros-5.16.3-286.el7.x86_64                                                                                                           13/53 
  Installing : perl-Storable-2.45-3.el7.x86_64                                                                                                               14/53 
  Installing : perl-Exporter-5.68-3.el7.noarch                                                                                                               15/53 
  Installing : perl-constant-1.27-2.el7.noarch                                                                                                               16/53 
  Installing : perl-Time-Local-1.2300-2.el7.noarch                                                                                                           17/53 
  Installing : perl-Socket-2.010-3.el7.x86_64                                                                                                                18/53 
  Installing : perl-Carp-1.26-244.el7.noarch                                                                                                                 19/53 
  Installing : perl-PathTools-3.40-5.el7.x86_64                                                                                                              20/53 
  Installing : perl-Scalar-List-Utils-1.27-248.el7.x86_64                                                                                                    21/53 
  Installing : 4:perl-Time-HiRes-1.9725-3.el7.x86_64                                                                                                         22/53 
  Installing : perl-File-Temp-0.23.01-3.el7.noarch                                                                                                           23/53 
  Installing : perl-File-Path-2.09-2.el7.noarch                                                                                                              24/53 
  Installing : perl-threads-shared-1.43-6.el7.x86_64                                                                                                         25/53 
  Installing : perl-threads-1.87-4.el7.x86_64                                                                                                                26/53 
  Installing : perl-Filter-1.49-3.el7.x86_64                                                                                                                 27/53 
  Installing : 1:perl-Pod-Simple-3.28-4.el7.noarch                                                                                                           28/53 
  Installing : perl-Getopt-Long-2.40-2.el7.noarch                                                                                                            29/53 
  Installing : 4:perl-5.16.3-286.el7.x86_64                                                                                                                  30/53 
  Installing : perl-Thread-Queue-3.02-2.el7.noarch                                                                                                           31/53 
  Installing : perl-Data-Dumper-2.145-3.el7.x86_64                                                                                                           32/53 
  Installing : perl-Test-Harness-3.28-3.el7.noarch                                                                                                           33/53 
  Installing : dwz-0.11-3.el7.x86_64                                                                                                                         34/53 
  Installing : zip-3.0-10.el7.x86_64                                                                                                                         35/53 
  Installing : gdb-7.6.1-80.el7.x86_64                                                                                                                       36/53 
  Installing : libstdc++-devel-4.8.5-4.el7.x86_64                                                                                                            37/53 
  Installing : unzip-6.0-15.el7.x86_64                                                                                                                       38/53 
  Installing : perl-srpm-macros-1-8.el7.noarch                                                                                                               39/53 
  Installing : redhat-rpm-config-9.1.0-68.el7.noarch                                                                                                         40/53 
  Installing : patch-2.7.1-8.el7.x86_64                                                                                                                      41/53 
  Installing : kernel-headers-3.10.0-327.el7.x86_64                                                                                                          42/53 
  Installing : glibc-headers-2.17-105.el7.x86_64                                                                                                             43/53 
  Installing : glibc-devel-2.17-105.el7.x86_64                                                                                                               44/53 
  Installing : gcc-4.8.5-4.el7.x86_64                                                                                                                        45/53 
  Installing : bzip2-1.0.6-13.el7.x86_64                                                                                                                     46/53 
  Installing : m4-1.4.16-10.el7.x86_64                                                                                                                       47/53 
  Installing : autoconf-2.69-11.el7.noarch                                                                                                                   48/53 
  Installing : elfutils-0.163-3.el7.x86_64                                                                                                                   49/53 
  Installing : rpm-build-4.11.3-17.el7.x86_64                                                                                                                50/53 
  Installing : automake-1.13.4-3.el7.noarch                                                                                                                  51/53 
  Installing : gcc-c++-4.8.5-4.el7.x86_64                                                                                                                    52/53 
  Installing : ksh-20120801-22.el7_1.2.x86_64                                                                                                                53/53 
local/productid                                                                                                                             | 1.6 kB  00:00:00     
  Verifying  : perl-HTTP-Tiny-0.033-3.el7.noarch                                                                                                              1/53 
  Verifying  : gcc-4.8.5-4.el7.x86_64                                                                                                                         2/53 
  Verifying  : perl-threads-shared-1.43-6.el7.x86_64                                                                                                          3/53 
  Verifying  : perl-Storable-2.45-3.el7.x86_64                                                                                                                4/53 
  Verifying  : mpfr-3.1.1-4.el7.x86_64                                                                                                                        5/53 
  Verifying  : perl-Exporter-5.68-3.el7.noarch                                                                                                                6/53 
  Verifying  : perl-constant-1.27-2.el7.noarch                                                                                                                7/53 
  Verifying  : perl-PathTools-3.40-5.el7.x86_64                                                                                                               8/53 
  Verifying  : gcc-c++-4.8.5-4.el7.x86_64                                                                                                                     9/53 
  Verifying  : elfutils-0.163-3.el7.x86_64                                                                                                                   10/53 
  Verifying  : automake-1.13.4-3.el7.noarch                                                                                                                  11/53 
  Verifying  : 4:perl-libs-5.16.3-286.el7.x86_64                                                                                                             12/53 
  Verifying  : m4-1.4.16-10.el7.x86_64                                                                                                                       13/53 
  Verifying  : 4:perl-macros-5.16.3-286.el7.x86_64                                                                                                           14/53 
  Verifying  : 1:perl-parent-0.225-244.el7.noarch                                                                                                            15/53 
  Verifying  : bzip2-1.0.6-13.el7.x86_64                                                                                                                     16/53 
  Verifying  : glibc-headers-2.17-105.el7.x86_64                                                                                                             17/53 
  Verifying  : kernel-headers-3.10.0-327.el7.x86_64                                                                                                          18/53 
  Verifying  : 4:perl-5.16.3-286.el7.x86_64                                                                                                                  19/53 
  Verifying  : perl-Thread-Queue-3.02-2.el7.noarch                                                                                                           20/53 
  Verifying  : perl-File-Temp-0.23.01-3.el7.noarch                                                                                                           21/53 
  Verifying  : 1:perl-Pod-Simple-3.28-4.el7.noarch                                                                                                           22/53 
  Verifying  : perl-Time-Local-1.2300-2.el7.noarch                                                                                                           23/53 
  Verifying  : perl-Pod-Perldoc-3.20-4.el7.noarch                                                                                                            24/53 
  Verifying  : patch-2.7.1-8.el7.x86_64                                                                                                                      25/53 
  Verifying  : perl-srpm-macros-1-8.el7.noarch                                                                                                               26/53 
  Verifying  : perl-Socket-2.010-3.el7.x86_64                                                                                                                27/53 
  Verifying  : perl-Carp-1.26-244.el7.noarch                                                                                                                 28/53 
  Verifying  : perl-Data-Dumper-2.145-3.el7.x86_64                                                                                                           29/53 
  Verifying  : glibc-devel-2.17-105.el7.x86_64                                                                                                               30/53 
  Verifying  : redhat-rpm-config-9.1.0-68.el7.noarch                                                                                                         31/53 
  Verifying  : rpm-build-4.11.3-17.el7.x86_64                                                                                                                32/53 
  Verifying  : perl-Scalar-List-Utils-1.27-248.el7.x86_64                                                                                                    33/53 
  Verifying  : libmpc-1.0.1-3.el7.x86_64                                                                                                                     34/53 
  Verifying  : 4:perl-Time-HiRes-1.9725-3.el7.x86_64                                                                                                         35/53 
  Verifying  : unzip-6.0-15.el7.x86_64                                                                                                                       36/53 
  Verifying  : 1:perl-Pod-Escapes-1.04-286.el7.noarch                                                                                                        37/53 
  Verifying  : perl-Pod-Usage-1.63-3.el7.noarch                                                                                                              38/53 
  Verifying  : perl-Encode-2.51-7.el7.x86_64                                                                                                                 39/53 
  Verifying  : libstdc++-devel-4.8.5-4.el7.x86_64                                                                                                            40/53 
  Verifying  : gdb-7.6.1-80.el7.x86_64                                                                                                                       41/53 
  Verifying  : perl-podlators-2.5.1-3.el7.noarch                                                                                                             42/53 
  Verifying  : perl-Getopt-Long-2.40-2.el7.noarch                                                                                                            43/53 
  Verifying  : autoconf-2.69-11.el7.noarch                                                                                                                   44/53 
  Verifying  : cpp-4.8.5-4.el7.x86_64                                                                                                                        45/53 
  Verifying  : perl-File-Path-2.09-2.el7.noarch                                                                                                              46/53 
  Verifying  : perl-threads-1.87-4.el7.x86_64                                                                                                                47/53 
  Verifying  : ksh-20120801-22.el7_1.2.x86_64                                                                                                                48/53 
  Verifying  : zip-3.0-10.el7.x86_64                                                                                                                         49/53 
  Verifying  : perl-Filter-1.49-3.el7.x86_64                                                                                                                 50/53 
  Verifying  : dwz-0.11-3.el7.x86_64                                                                                                                         51/53 
  Verifying  : perl-Text-ParseWords-3.29-4.el7.noarch                                                                                                        52/53 
  Verifying  : perl-Test-Harness-3.28-3.el7.noarch                                                                                                           53/53 

Installed:
  automake.noarch 0:1.13.4-3.el7      cpp.x86_64 0:4.8.5-4.el7    gcc.x86_64 0:4.8.5-4.el7    gcc-c++.x86_64 0:4.8.5-4.el7    ksh.x86_64 0:20120801-22.el7_1.2   
  rpm-build.x86_64 0:4.11.3-17.el7   

Dependency Installed:
  autoconf.noarch 0:2.69-11.el7                bzip2.x86_64 0:1.0.6-13.el7           dwz.x86_64 0:0.11-3.el7               elfutils.x86_64 0:0.163-3.el7          
  gdb.x86_64 0:7.6.1-80.el7                    glibc-devel.x86_64 0:2.17-105.el7     glibc-headers.x86_64 0:2.17-105.el7   kernel-headers.x86_64 0:3.10.0-327.el7 
  libmpc.x86_64 0:1.0.1-3.el7                  libstdc++-devel.x86_64 0:4.8.5-4.el7  m4.x86_64 0:1.4.16-10.el7             mpfr.x86_64 0:3.1.1-4.el7              
  patch.x86_64 0:2.7.1-8.el7                   perl.x86_64 4:5.16.3-286.el7          perl-Carp.noarch 0:1.26-244.el7       perl-Data-Dumper.x86_64 0:2.145-3.el7  
  perl-Encode.x86_64 0:2.51-7.el7              perl-Exporter.noarch 0:5.68-3.el7     perl-File-Path.noarch 0:2.09-2.el7    perl-File-Temp.noarch 0:0.23.01-3.el7  
  perl-Filter.x86_64 0:1.49-3.el7              perl-Getopt-Long.noarch 0:2.40-2.el7  perl-HTTP-Tiny.noarch 0:0.033-3.el7   perl-PathTools.x86_64 0:3.40-5.el7     
  perl-Pod-Escapes.noarch 1:1.04-286.el7       perl-Pod-Perldoc.noarch 0:3.20-4.el7  perl-Pod-Simple.noarch 1:3.28-4.el7   perl-Pod-Usage.noarch 0:1.63-3.el7     
  perl-Scalar-List-Utils.x86_64 0:1.27-248.el7 perl-Socket.x86_64 0:2.010-3.el7      perl-Storable.x86_64 0:2.45-3.el7     perl-Test-Harness.noarch 0:3.28-3.el7  
  perl-Text-ParseWords.noarch 0:3.29-4.el7     perl-Thread-Queue.noarch 0:3.02-2.el7 perl-Time-HiRes.x86_64 4:1.9725-3.el7 perl-Time-Local.noarch 0:1.2300-2.el7  
  perl-constant.noarch 0:1.27-2.el7            perl-libs.x86_64 4:5.16.3-286.el7     perl-macros.x86_64 4:5.16.3-286.el7   perl-parent.noarch 1:0.225-244.el7     
  perl-podlators.noarch 0:2.5.1-3.el7          perl-srpm-macros.noarch 0:1-8.el7     perl-threads.x86_64 0:1.87-4.el7      perl-threads-shared.x86_64 0:1.43-6.el7
  redhat-rpm-config.noarch 0:9.1.0-68.el7      unzip.x86_64 0:6.0-15.el7             zip.x86_64 0:3.0-10.el7              

Complete!
```

**注意 ：kernel-devel kernel-headers 的版本要和系统的内核版本一致，不然构建可移植层会出问题**

系统内核版本为：**3.10.0-327**

```
yum install kernel-devel kernel-headers -y
```

```
[root@gpfs-01 ~]# uname -a
Linux gpfs-01 3.10.0-327.el7.x86_64 #1 SMP Thu Oct 29 17:29:29 EDT 2015 x86_64 x86_64 x86_64 GNU/Linux
[root@gpfs-01 ~]# 
[root@gpfs-01 ~]# yum install kernel-devel kernel-headers -y
Loaded plugins: product-id, search-disabled-repos, subscription-manager
This system is not registered to Red Hat Subscription Management. You can use subscription-manager to register.
Package kernel-headers-3.10.0-327.el7.x86_64 already installed and latest version
Resolving Dependencies
--> Running transaction check
---> Package kernel-devel.x86_64 0:3.10.0-327.el7 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

===================================================================================================================================================================
 Package                                  Arch                               Version                                       Repository                         Size
===================================================================================================================================================================
Installing:
 kernel-devel                             x86_64                             3.10.0-327.el7                                local                              11 M

Transaction Summary
===================================================================================================================================================================
Install  1 Package

Total download size: 11 M
Installed size: 33 M
Downloading packages:
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : kernel-devel-3.10.0-327.el7.x86_64                                                                                                              1/1 
  Verifying  : kernel-devel-3.10.0-327.el7.x86_64                                                                                                              1/1 

Installed:
  kernel-devel.x86_64 0:3.10.0-327.el7                                                                                                                             

Complete!
```

节点gpfs-02 重复一遍操作

### 4、配置互信

```
ssh-keygen -t rsa
ssh-copy-id gpfs-01
ssh-copy-id gpfs-02
```

```
[root@gpfs-01 ~]# ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa): 
Created directory '/root/.ssh'.
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /root/.ssh/id_rsa.
Your public key has been saved in /root/.ssh/id_rsa.pub.
The key fingerprint is:
43:51:dd:5f:d9:e9:c1:49:bc:ba:6d:7b:dd:72:3c:52 root@gpfs-01
The key's randomart image is:
+--[ RSA 2048]----+
|        .... .+.=|
|         .  . .Bo|
|        .     ..+|
|       .       o.|
|        S     .  |
|         .   . E |
|              +.o|
|             o.+*|
|              o++|
+-----------------+
[root@gpfs-01 ~]# 
[root@gpfs-01 ~]# ssh-copy-id gpfs-01
The authenticity of host 'gpfs-01 (192.168.196.150)' can't be established.
ECDSA key fingerprint is 75:a0:47:d4:7d:a1:66:a0:f3:29:c2:a1:03:b2:86:f7.
Are you sure you want to continue connecting (yes/no)? yes
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
root@gpfs-01's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'gpfs-01'"
and check to make sure that only the key(s) you wanted were added.

[root@gpfs-01 ~]# ssh-copy-id gpfs-02
The authenticity of host 'gpfs-02 (192.168.196.160)' can't be established.
ECDSA key fingerprint is 75:a0:47:d4:7d:a1:66:a0:f3:29:c2:a1:03:b2:86:f7.
Are you sure you want to continue connecting (yes/no)? yes
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
root@gpfs-02's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'gpfs-02'"
and check to make sure that only the key(s) you wanted were added.
```

节点gpfs-02 重复一遍操作

## 二、GPFS安装

### 1、解压gpfs安装包

```
mkdir /root/gpfs
mv Spectrum_Scale_Advanced-4.2.0.3-x86_64-Linux-install /root/gpfs/
cd /root/gpfs/
sh Spectrum_Scale_Advanced-4.2.0.3-x86_64-Linux-install --text-only
```

```
[root@gpfs-01 ~]# ls
anaconda-ks.cfg  Spectrum_Scale_Advanced-4.2.0.3-x86_64-Linux-install
[root@gpfs-01 ~]# 
[root@gpfs-01 ~]# mkdir /root/gpfs
[root@gpfs-01 ~]# mv Spectrum_Scale_Advanced-4.2.0.3-x86_64-Linux-install /root/gpfs/
[root@gpfs-01 ~]# cd /root/gpfs/
[root@gpfs-01 gpfs]# 
[root@gpfs-01 gpfs]# sh Spectrum_Scale_Advanced-4.2.0.3-x86_64-Linux-install --text-only

Extracting License Acceptance Process Tool to /usr/lpp/mmfs/4.2.0.3 ...
tail -n +556 Spectrum_Scale_Advanced-4.2.0.3-x86_64-Linux-install | tar -C /usr/lpp/mmfs/4.2.0.3 -xvz --exclude=installer --exclude=*_rpms --exclude=*rpm  --exclude=*tgz --exclude=*deb 1> /dev/null

Installing JRE ...
tail -n +556 Spectrum_Scale_Advanced-4.2.0.3-x86_64-Linux-install | tar -C /usr/lpp/mmfs/4.2.0.3 --wildcards -xvz  ibm-java*tgz 1> /dev/null
tar -C /usr/lpp/mmfs/4.2.0.3/ -xzf /usr/lpp/mmfs/4.2.0.3/ibm-java*tgz

Invoking License Acceptance Process Tool ...
/usr/lpp/mmfs/4.2.0.3/ibm-java-x86_64-71/jre/bin/java -cp /usr/lpp/mmfs/4.2.0.3/LAP_HOME/LAPApp.jar com.ibm.lex.lapapp.LAP -l /usr/lpp/mmfs/4.2.0.3/LA_HOME -m /usr/lpp/mmfs/4.2.0.3 -s /usr/lpp/mmfs/4.2.0.3  -text_only

LICENSE INFORMATION

The Programs listed below are licensed under the following 
License Information terms and conditions in addition to the 
Program license terms previously agreed to by Client and 
IBM. If Client does not have previously agreed to license 
terms in effect for the Program, the IBM International 
Program License Agreement (Z125-3301-14) applies.

Program Name: IBM Spectrum Scale V4.2.0.2
Program Number: 5641-GPF

Program Name: IBM Spectrum Scale V4.2.0.2
Program Number: 5725-Q01

Press Enter to continue viewing the license agreement, or 
enter "1" to accept the agreement, "2" to decline it, "3" 
to print it, "4" to read non-IBM terms, or "99" to go back 
to the previous screen.
1

License Agreement Terms accepted.

Extracting Product RPMs to /usr/lpp/mmfs/4.2.0.3 ...
tail -n +556 Spectrum_Scale_Advanced-4.2.0.3-x86_64-Linux-install | tar -C /usr/lpp/mmfs/4.2.0.3 --wildcards -xvz  gpfs.adv-4.2.0-3.x86_64.rpm gpfs.adv_4.2.0-3_amd64.deb gpfs.base-4.2.0-3.x86_64.rpm gpfs.base_4.2.0-3_amd64.deb gpfs.callhome-4.2.0-1.001.noarch.rpm gpfs.callhome-ecc-client-4.2.0-1.000.noarch.rpm gpfs.callhome-jre-8.0-2.0.x86_64.rpm gpfs.crypto-4.2.0-3.x86_64.rpm gpfs.crypto_4.2.0-3_amd64.deb gpfs.docs-4.2.0-3.noarch.rpm gpfs.docs_4.2.0-3_all.deb gpfs.ext-4.2.0-3.x86_64.rpm gpfs.ext_4.2.0-3_amd64.deb gpfs.gpl-4.2.0-3.noarch.rpm gpfs.gpl_4.2.0-3_all.deb gpfs.gskit-8.0.50-47.x86_64.rpm gpfs.gskit_8.0.50-47_amd64.deb gpfs.gui-4.2.0-3.el7.x86_64.rpm gpfs.gui-4.2.0-3.sles12.x86_64.rpm gpfs.msg.en-us_4.2.0-3_all.deb gpfs.msg.en_US-4.2.0-3.noarch.rpm gpfs.gss.pmcollector-4.2.0-3.SLES12.x86_64.rpm gpfs.gss.pmcollector-4.2.0-3.el6.x86_64.rpm gpfs.gss.pmcollector-4.2.0-3.el7.x86_64.rpm gpfs.gss.pmcollector_4.2.0-3.D7.6_amd64.deb gpfs.gss.pmcollector_4.2.0-3.D8.3_amd64.deb gpfs.gss.pmcollector_4.2.0-3.U14.04_amd64.deb gpfs.gss.pmsensors-4.2.0-3.SLES12.x86_64.rpm gpfs.gss.pmsensors-4.2.0-3.el6.x86_64.rpm gpfs.gss.pmsensors-4.2.0-3.el7.x86_64.rpm gpfs.gss.pmsensors_4.2.0-3.D7.6_amd64.deb gpfs.gss.pmsensors_4.2.0-3.D8.3_amd64.deb gpfs.gss.pmsensors_4.2.0-3.U14.04_amd64.deb manifest 1> /dev/null
   - gpfs.adv-4.2.0-3.x86_64.rpm
   - gpfs.adv_4.2.0-3_amd64.deb
   - gpfs.base-4.2.0-3.x86_64.rpm
   - gpfs.base_4.2.0-3_amd64.deb
   - gpfs.callhome-4.2.0-1.001.noarch.rpm
   - gpfs.callhome-ecc-client-4.2.0-1.000.noarch.rpm
   - gpfs.callhome-jre-8.0-2.0.x86_64.rpm
   - gpfs.crypto-4.2.0-3.x86_64.rpm
   - gpfs.crypto_4.2.0-3_amd64.deb
   - gpfs.docs-4.2.0-3.noarch.rpm
   - gpfs.docs_4.2.0-3_all.deb
   - gpfs.ext-4.2.0-3.x86_64.rpm
   - gpfs.ext_4.2.0-3_amd64.deb
   - gpfs.gpl-4.2.0-3.noarch.rpm
   - gpfs.gpl_4.2.0-3_all.deb
   - gpfs.gskit-8.0.50-47.x86_64.rpm
   - gpfs.gskit_8.0.50-47_amd64.deb
   - gpfs.gui-4.2.0-3.el7.x86_64.rpm
   - gpfs.gui-4.2.0-3.sles12.x86_64.rpm
   - gpfs.msg.en-us_4.2.0-3_all.deb
   - gpfs.msg.en_US-4.2.0-3.noarch.rpm
   - gpfs.gss.pmcollector-4.2.0-3.SLES12.x86_64.rpm
   - gpfs.gss.pmcollector-4.2.0-3.el6.x86_64.rpm
   - gpfs.gss.pmcollector-4.2.0-3.el7.x86_64.rpm
   - gpfs.gss.pmcollector_4.2.0-3.D7.6_amd64.deb
   - gpfs.gss.pmcollector_4.2.0-3.D8.3_amd64.deb
   - gpfs.gss.pmcollector_4.2.0-3.U14.04_amd64.deb
   - gpfs.gss.pmsensors-4.2.0-3.SLES12.x86_64.rpm
   - gpfs.gss.pmsensors-4.2.0-3.el6.x86_64.rpm
   - gpfs.gss.pmsensors-4.2.0-3.el7.x86_64.rpm
   - gpfs.gss.pmsensors_4.2.0-3.D7.6_amd64.deb
   - gpfs.gss.pmsensors_4.2.0-3.D8.3_amd64.deb
   - gpfs.gss.pmsensors_4.2.0-3.U14.04_amd64.deb
   - manifest

Removing License Acceptance Process Tool from /usr/lpp/mmfs/4.2.0.3 ...
rm -rf  /usr/lpp/mmfs/4.2.0.3/LAP_HOME /usr/lpp/mmfs/4.2.0.3/LA_HOME

Removing JRE from /usr/lpp/mmfs/4.2.0.3 ...
rm -rf /usr/lpp/mmfs/4.2.0.3/ibm-java*tgz

==================================================================
Product rpms successfully extracted to /usr/lpp/mmfs/4.2.0.3
```

注意：节点gpfs-02无需再次操作

### 2、开始安装

进入解压后的软件目录

```
cd /usr/lpp/mmfs/4.2.0.3
```

安装rpm包

```
yum install gpfs.base-4.2.0-3.x86_64.rpm gpfs.gpl-4.2.0-3.noarch.rpm gpfs.msg.en_US-4.2.0-3.noarch.rpm gpfs.gskit-8.0.50-47.x86_64.rpm gpfs.ext-4.2.0-3.x86_64.rpm gpfs.crypto-4.2.0-3.x86_64.rpm -y
```

```
[root@gpfs-01 gpfs]# cd /usr/lpp/mmfs/4.2.0.3/
[root@gpfs-01 4.2.0.3]# ll
total 476772
-rw-r--r--. 1 root root     51700 May  5  2016 gpfs.adv_4.2.0-3_amd64.deb
-rw-r--r--. 1 root root     55161 May  5  2016 gpfs.adv-4.2.0-3.x86_64.rpm
-rw-r--r--. 1 root root  16527964 May  5  2016 gpfs.base_4.2.0-3_amd64.deb
-rw-r--r--. 1 root root  16924339 May  5  2016 gpfs.base-4.2.0-3.x86_64.rpm
-rw-r--r--. 1 root root     77396 May  5  2016 gpfs.callhome-4.2.0-1.001.noarch.rpm
-rw-r--r--. 1 root root  18956312 May  5  2016 gpfs.callhome-ecc-client-4.2.0-1.000.noarch.rpm
-rw-r--r--. 1 root root 102530400 May  5  2016 gpfs.callhome-jre-8.0-2.0.x86_64.rpm
-rw-r--r--. 1 root root    307224 May  5  2016 gpfs.crypto_4.2.0-3_amd64.deb
-rw-r--r--. 1 root root    311850 May  5  2016 gpfs.crypto-4.2.0-3.x86_64.rpm
-rw-r--r--. 1 root root    334128 May  5  2016 gpfs.docs_4.2.0-3_all.deb
-rw-r--r--. 1 root root    369656 May  5  2016 gpfs.docs-4.2.0-3.noarch.rpm
-rw-r--r--. 1 root root   2390126 May  5  2016 gpfs.ext_4.2.0-3_amd64.deb
-rw-r--r--. 1 root root   2452036 May  5  2016 gpfs.ext-4.2.0-3.x86_64.rpm
-rw-r--r--. 1 root root    599438 May  5  2016 gpfs.gpl_4.2.0-3_all.deb
-rw-r--r--. 1 root root    629768 May  5  2016 gpfs.gpl-4.2.0-3.noarch.rpm
-rw-r--r--. 1 root root  10407870 May  5  2016 gpfs.gskit_8.0.50-47_amd64.deb
-rw-r--r--. 1 root root  10374353 May  5  2016 gpfs.gskit-8.0.50-47.x86_64.rpm
-rw-r--r--. 1 root root    542408 May  5  2016 gpfs.gss.pmcollector_4.2.0-3.D7.6_amd64.deb
-rw-r--r--. 1 root root    522822 May  5  2016 gpfs.gss.pmcollector_4.2.0-3.D8.3_amd64.deb
-rw-r--r--. 1 root root    520782 May  5  2016 gpfs.gss.pmcollector-4.2.0-3.el6.x86_64.rpm
-rw-r--r--. 1 root root    401532 May  5  2016 gpfs.gss.pmcollector-4.2.0-3.el7.x86_64.rpm
-rw-r--r--. 1 root root    384485 May  5  2016 gpfs.gss.pmcollector-4.2.0-3.SLES12.x86_64.rpm
-rw-r--r--. 1 root root    521288 May  5  2016 gpfs.gss.pmcollector_4.2.0-3.U14.04_amd64.deb
-rw-r--r--. 1 root root    550702 May  5  2016 gpfs.gss.pmsensors_4.2.0-3.D7.6_amd64.deb
-rw-r--r--. 1 root root    524544 May  5  2016 gpfs.gss.pmsensors_4.2.0-3.D8.3_amd64.deb
-rw-r--r--. 1 root root    596671 May  5  2016 gpfs.gss.pmsensors-4.2.0-3.el6.x86_64.rpm
-rw-r--r--. 1 root root    346216 May  5  2016 gpfs.gss.pmsensors-4.2.0-3.el7.x86_64.rpm
-rw-r--r--. 1 root root    333366 May  5  2016 gpfs.gss.pmsensors-4.2.0-3.SLES12.x86_64.rpm
-rw-r--r--. 1 root root    531180 May  5  2016 gpfs.gss.pmsensors_4.2.0-3.U14.04_amd64.deb
-rw-r--r--. 1 root root 152632680 May  5  2016 gpfs.gui-4.2.0-3.el7.x86_64.rpm
-rw-r--r--. 1 root root 146095061 May  5  2016 gpfs.gui-4.2.0-3.sles12.x86_64.rpm
-rw-r--r--. 1 root root    167346 May  5  2016 gpfs.msg.en-us_4.2.0-3_all.deb
-rw-r--r--. 1 root root    170909 May  5  2016 gpfs.msg.en_US-4.2.0-3.noarch.rpm
drwxr-xr-x. 3 root root      4096 Mar 19 15:19 license
-rw-r--r--. 1 root root      3357 May  5  2016 manifest
[root@gpfs-01 4.2.0.3]# 
[root@gpfs-01 4.2.0.3]# yum install gpfs.base-4.2.0-3.x86_64.rpm gpfs.gpl-4.2.0-3.noarch.rpm gpfs.msg.en_US-4.2.0-3.noarch.rpm gpfs.gskit-8.0.50-47.x86_64.rpm gpfs.ext-4.2.0-3.x86_64.rpm gpfs.crypto-4.2.0-3.x86_64.rpm -y
Loaded plugins: product-id, search-disabled-repos, subscription-manager
This system is not registered to Red Hat Subscription Management. You can use subscription-manager to register.
Examining gpfs.base-4.2.0-3.x86_64.rpm: gpfs.base-4.2.0-3.x86_64
Marking gpfs.base-4.2.0-3.x86_64.rpm to be installed
Examining gpfs.gpl-4.2.0-3.noarch.rpm: gpfs.gpl-4.2.0-3.noarch
Marking gpfs.gpl-4.2.0-3.noarch.rpm to be installed
Examining gpfs.msg.en_US-4.2.0-3.noarch.rpm: gpfs.msg.en_US-4.2.0-3.noarch
Marking gpfs.msg.en_US-4.2.0-3.noarch.rpm to be installed
Examining gpfs.gskit-8.0.50-47.x86_64.rpm: gpfs.gskit-8.0.50-47.x86_64
Marking gpfs.gskit-8.0.50-47.x86_64.rpm to be installed
Examining gpfs.ext-4.2.0-3.x86_64.rpm: gpfs.ext-4.2.0-3.x86_64
Marking gpfs.ext-4.2.0-3.x86_64.rpm to be installed
Examining gpfs.crypto-4.2.0-3.x86_64.rpm: gpfs.crypto-4.2.0-3.x86_64
Marking gpfs.crypto-4.2.0-3.x86_64.rpm to be installed
Resolving Dependencies
--> Running transaction check
---> Package gpfs.base.x86_64 0:4.2.0-3 will be installed
--> Processing Dependency: net-tools for package: gpfs.base-4.2.0-3.x86_64
---> Package gpfs.crypto.x86_64 0:4.2.0-3 will be installed
---> Package gpfs.ext.x86_64 0:4.2.0-3 will be installed
---> Package gpfs.gpl.noarch 0:4.2.0-3 will be installed
---> Package gpfs.gskit.x86_64 0:8.0.50-47 will be installed
---> Package gpfs.msg.en_US.noarch 0:4.2.0-3 will be installed
--> Running transaction check
---> Package net-tools.x86_64 0:2.0-0.17.20131004git.el7 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

===================================================================================================================================================================
 Package                           Arch                      Version                                       Repository                                         Size
===================================================================================================================================================================
Installing:
 gpfs.base                         x86_64                    4.2.0-3                                       /gpfs.base-4.2.0-3.x86_64                          47 M
 gpfs.crypto                       x86_64                    4.2.0-3                                       /gpfs.crypto-4.2.0-3.x86_64                       688 k
 gpfs.ext                          x86_64                    4.2.0-3                                       /gpfs.ext-4.2.0-3.x86_64                          8.2 M
 gpfs.gpl                          noarch                    4.2.0-3                                       /gpfs.gpl-4.2.0-3.noarch                          2.6 M
 gpfs.gskit                        x86_64                    8.0.50-47                                     /gpfs.gskit-8.0.50-47.x86_64                       28 M
 gpfs.msg.en_US                    noarch                    4.2.0-3                                       /gpfs.msg.en_US-4.2.0-3.noarch                    646 k
Installing for dependencies:
 net-tools                         x86_64                    2.0-0.17.20131004git.el7                      local                                             304 k

Transaction Summary
===================================================================================================================================================================
Install  6 Packages (+1 Dependent package)

Total size: 87 M
Total download size: 304 k
Installed size: 87 M
Downloading packages:
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : net-tools-2.0-0.17.20131004git.el7.x86_64                                                                                                       1/7 
  Installing : gpfs.msg.en_US-4.2.0-3.noarch                                                                                                                   2/7 
  Installing : gpfs.gskit-8.0.50-47.x86_64                                                                                                                     3/7 
  Installing : gpfs.base-4.2.0-3.x86_64                                                                                                                        4/7 
Created symlink from /etc/systemd/system/multi-user.target.wants/gpfs.service to /usr/lpp/mmfs/lib/systemd/gpfs.service.
Created symlink from /etc/systemd/system/gpfs.service to /usr/lpp/mmfs/lib/systemd/gpfs.service.
  Installing : gpfs.ext-4.2.0-3.x86_64                                                                                                                         5/7 
  Installing : gpfs.crypto-4.2.0-3.x86_64                                                                                                                      6/7 
  Installing : gpfs.gpl-4.2.0-3.noarch                                                                                                                         7/7 
  Verifying  : gpfs.base-4.2.0-3.x86_64                                                                                                                        1/7 
  Verifying  : gpfs.gskit-8.0.50-47.x86_64                                                                                                                     2/7 
  Verifying  : net-tools-2.0-0.17.20131004git.el7.x86_64                                                                                                       3/7 
  Verifying  : gpfs.crypto-4.2.0-3.x86_64                                                                                                                      4/7 
  Verifying  : gpfs.ext-4.2.0-3.x86_64                                                                                                                         5/7 
  Verifying  : gpfs.gpl-4.2.0-3.noarch                                                                                                                         6/7 
  Verifying  : gpfs.msg.en_US-4.2.0-3.noarch                                                                                                                   7/7 

Installed:
  gpfs.base.x86_64 0:4.2.0-3         gpfs.crypto.x86_64 0:4.2.0-3    gpfs.ext.x86_64 0:4.2.0-3    gpfs.gpl.noarch 0:4.2.0-3    gpfs.gskit.x86_64 0:8.0.50-47   
  gpfs.msg.en_US.noarch 0:4.2.0-3   

Dependency Installed:
  net-tools.x86_64 0:2.0-0.17.20131004git.el7                                                                                                                      

Complete!
```

注意：节点gpfs-02无需再次操作

### 3、编写环境变量

```
vi /etc/profile
文件最后追加 以下信息：
```

```
if [ -f ~/.bashrc ]; then
        . ~/.bashrc
fi
# User specific environment and startup programs
PATH=$PATH:$HOME/bin:/usr/lpp/mmfs/bin
export PATH
```

```
source /etc/profile
```

节点gpfs-02 重复一遍操作

### 4、构建可移植层

```
/usr/lpp/mmfs/bin/mmbuildgpl --build-package 
```

```
[root@gpfs-01 4.2.0.3]# /usr/lpp/mmfs/bin/mmbuildgpl --build-package 
--------------------------------------------------------
mmbuildgpl: Building GPL module begins at Tue Mar 19 15:31:16 CST 2024.
--------------------------------------------------------
Verifying Kernel Header...
  kernel version = 31000327 (3.10.0-327.el7.x86_64, 3.10.0-327) 
  module include dir = /lib/modules/3.10.0-327.el7.x86_64/build/include 
  module build dir   = /lib/modules/3.10.0-327.el7.x86_64/build 
  kernel source dir  = /usr/src/linux-3.10.0-327.el7.x86_64/include 
  Found valid kernel header file under /usr/src/kernels/3.10.0-327.el7.x86_64/include
Verifying Compiler...
  make is present at /bin/make
  cpp is present at /bin/cpp
  gcc is present at /bin/gcc
  g++ is present at /bin/g++
  ld is present at /bin/ld
Verifying rpmbuild...
Verifying Additional System Headers...
  Verifying kernel-headers is installed ...
    Command: /bin/rpm -q kernel-headers  
    The required package kernel-headers is installed
make World ...
make InstallImages ...
make rpm ...
Wrote: /root/rpmbuild/RPMS/x86_64/gpfs.gplbin-3.10.0-327.el7.x86_64-4.2.0-3.x86_64.rpm
--------------------------------------------------------
mmbuildgpl: Building GPL module completed successfully at Tue Mar 19 15:31:25 CST 2024.
--------------------------------------------------------
```

注意：节点gpfs-02无需再次操作

### 5、安装可移植层

```
yum install /root/rpmbuild/RPMS/x86_64/gpfs.gplbin-3.10.0-327.el7.x86_64-4.2.0-3.x86_64.rpm -y
```

```
[root@gpfs-01 4.2.0.3]# yum install /root/rpmbuild/RPMS/x86_64/gpfs.gplbin-3.10.0-327.el7.x86_64-4.2.0-3.x86_64.rpm -y
Loaded plugins: product-id, search-disabled-repos, subscription-manager
This system is not registered to Red Hat Subscription Management. You can use subscription-manager to register.
Examining /root/rpmbuild/RPMS/x86_64/gpfs.gplbin-3.10.0-327.el7.x86_64-4.2.0-3.x86_64.rpm: gpfs.gplbin-3.10.0-327.el7.x86_64-4.2.0-3.x86_64
Marking /root/rpmbuild/RPMS/x86_64/gpfs.gplbin-3.10.0-327.el7.x86_64-4.2.0-3.x86_64.rpm to be installed
Resolving Dependencies
--> Running transaction check
---> Package gpfs.gplbin-3.10.0-327.el7.x86_64.x86_64 0:4.2.0-3 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

===================================================================================================================================================================
 Package                                         Arch                 Version                Repository                                                       Size
===================================================================================================================================================================
Installing:
 gpfs.gplbin-3.10.0-327.el7.x86_64               x86_64               4.2.0-3                /gpfs.gplbin-3.10.0-327.el7.x86_64-4.2.0-3.x86_64               4.5 M

Transaction Summary
===================================================================================================================================================================
Install  1 Package

Total size: 4.5 M
Installed size: 4.5 M
Downloading packages:
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : gpfs.gplbin-3.10.0-327.el7.x86_64-4.2.0-3.x86_64                                                                                                1/1 
  Verifying  : gpfs.gplbin-3.10.0-327.el7.x86_64-4.2.0-3.x86_64                                                                                                1/1 

Installed:
  gpfs.gplbin-3.10.0-327.el7.x86_64.x86_64 0:4.2.0-3                                                                                                               

Complete!
```

注意：节点gpfs-02无需再次操作

### 6、其他节点配置

节点gpfs-02先建目录

```
[root@gpfs-02 ~]# mkdir -p /root/gpfs
```

将 gpfs-01 上的包拷贝到 gpfs-02 上 ，包括可移植层

```
scp gpfs.base-4.2.0-3.x86_64.rpm gpfs.gpl-4.2.0-3.noarch.rpm gpfs.msg.en_US-4.2.0-3.noarch.rpm gpfs.gskit-8.0.50-47.x86_64.rpm gpfs.ext-4.2.0-3.x86_64.rpm gpfs.crypto-4.2.0-3.x86_64.rpm gpfs-02:/root/gpfs
```

```
scp /root/rpmbuild/RPMS/x86_64/gpfs.gplbin-3.10.0-327.el7.x86_64-4.2.0-3.x86_64.rpm gpfs-02:/root/gpfs
```

```
[root@gpfs-01 4.2.0.3]# scp gpfs.base-4.2.0-3.x86_64.rpm gpfs.gpl-4.2.0-3.noarch.rpm gpfs.msg.en_US-4.2.0-3.noarch.rpm gpfs.gskit-8.0.50-47.x86_64.rpm gpfs.ext-4.2.0-3.x86_64.rpm gpfs.crypto-4.2.0-3.x86_64.rpm gpfs-02:/root/gpfs
gpfs.base-4.2.0-3.x86_64.rpm                                                                                                     100%   16MB  16.1MB/s   00:00    
gpfs.gpl-4.2.0-3.noarch.rpm                                                                                                      100%  615KB 615.0KB/s   00:00    
gpfs.msg.en_US-4.2.0-3.noarch.rpm                                                                                                100%  167KB 166.9KB/s   00:00    
gpfs.gskit-8.0.50-47.x86_64.rpm                                                                                                  100%   10MB   9.9MB/s   00:00    
gpfs.ext-4.2.0-3.x86_64.rpm                                                                                                      100% 2395KB   2.3MB/s   00:00    
gpfs.crypto-4.2.0-3.x86_64.rpm                                                                                                   100%  305KB 304.5KB/s   00:00    
[root@gpfs-01 4.2.0.3]# 
[root@gpfs-01 4.2.0.3]# scp /root/rpmbuild/RPMS/x86_64/gpfs.gplbin-3.10.0-327.el7.x86_64-4.2.0-3.x86_64.rpm gpfs-02:/root/gpfs
gpfs.gplbin-3.10.0-327.el7.x86_64-4.2.0-3.x86_64.rpm                                                                             100% 1043KB   1.0MB/s   00:00    
```

节点gpfs-02上安装

```
cd /root/gpfs
yum install *.rpm -y
```

```
[root@gpfs-02 gpfs]# cd /root/gpfs
[root@gpfs-02 gpfs]# 
[root@gpfs-02 gpfs]# ll
total 31192
-rw-r--r--. 1 root root 16924339 Mar 19 15:48 gpfs.base-4.2.0-3.x86_64.rpm
-rw-r--r--. 1 root root   311850 Mar 19 15:48 gpfs.crypto-4.2.0-3.x86_64.rpm
-rw-r--r--. 1 root root  2452036 Mar 19 15:48 gpfs.ext-4.2.0-3.x86_64.rpm
-rw-r--r--. 1 root root   629768 Mar 19 15:48 gpfs.gpl-4.2.0-3.noarch.rpm
-rw-r--r--. 1 root root  1068296 Mar 19 15:48 gpfs.gplbin-3.10.0-327.el7.x86_64-4.2.0-3.x86_64.rpm
-rw-r--r--. 1 root root 10374353 Mar 19 15:48 gpfs.gskit-8.0.50-47.x86_64.rpm
-rw-r--r--. 1 root root   170909 Mar 19 15:48 gpfs.msg.en_US-4.2.0-3.noarch.rpm
[root@gpfs-02 gpfs]# 
[root@gpfs-02 gpfs]# yum install *.rpm -y
Loaded plugins: product-id, search-disabled-repos, subscription-manager
This system is not registered to Red Hat Subscription Management. You can use subscription-manager to register.
Examining gpfs.base-4.2.0-3.x86_64.rpm: gpfs.base-4.2.0-3.x86_64
Marking gpfs.base-4.2.0-3.x86_64.rpm to be installed
Examining gpfs.crypto-4.2.0-3.x86_64.rpm: gpfs.crypto-4.2.0-3.x86_64
Marking gpfs.crypto-4.2.0-3.x86_64.rpm to be installed
Examining gpfs.ext-4.2.0-3.x86_64.rpm: gpfs.ext-4.2.0-3.x86_64
Marking gpfs.ext-4.2.0-3.x86_64.rpm to be installed
Examining gpfs.gpl-4.2.0-3.noarch.rpm: gpfs.gpl-4.2.0-3.noarch
Marking gpfs.gpl-4.2.0-3.noarch.rpm to be installed
Examining gpfs.gplbin-3.10.0-327.el7.x86_64-4.2.0-3.x86_64.rpm: gpfs.gplbin-3.10.0-327.el7.x86_64-4.2.0-3.x86_64
Marking gpfs.gplbin-3.10.0-327.el7.x86_64-4.2.0-3.x86_64.rpm to be installed
Examining gpfs.gskit-8.0.50-47.x86_64.rpm: gpfs.gskit-8.0.50-47.x86_64
Marking gpfs.gskit-8.0.50-47.x86_64.rpm to be installed
Examining gpfs.msg.en_US-4.2.0-3.noarch.rpm: gpfs.msg.en_US-4.2.0-3.noarch
Marking gpfs.msg.en_US-4.2.0-3.noarch.rpm to be installed
Resolving Dependencies
--> Running transaction check
---> Package gpfs.base.x86_64 0:4.2.0-3 will be installed
--> Processing Dependency: net-tools for package: gpfs.base-4.2.0-3.x86_64
---> Package gpfs.crypto.x86_64 0:4.2.0-3 will be installed
---> Package gpfs.ext.x86_64 0:4.2.0-3 will be installed
---> Package gpfs.gpl.noarch 0:4.2.0-3 will be installed
---> Package gpfs.gplbin-3.10.0-327.el7.x86_64.x86_64 0:4.2.0-3 will be installed
---> Package gpfs.gskit.x86_64 0:8.0.50-47 will be installed
---> Package gpfs.msg.en_US.noarch 0:4.2.0-3 will be installed
--> Running transaction check
---> Package net-tools.x86_64 0:2.0-0.17.20131004git.el7 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

===================================================================================================================================================================
 Package                                     Arch             Version                            Repository                                                   Size
===================================================================================================================================================================
Installing:
 gpfs.base                                   x86_64           4.2.0-3                            /gpfs.base-4.2.0-3.x86_64                                    47 M
 gpfs.crypto                                 x86_64           4.2.0-3                            /gpfs.crypto-4.2.0-3.x86_64                                 688 k
 gpfs.ext                                    x86_64           4.2.0-3                            /gpfs.ext-4.2.0-3.x86_64                                    8.2 M
 gpfs.gpl                                    noarch           4.2.0-3                            /gpfs.gpl-4.2.0-3.noarch                                    2.6 M
 gpfs.gplbin-3.10.0-327.el7.x86_64           x86_64           4.2.0-3                            /gpfs.gplbin-3.10.0-327.el7.x86_64-4.2.0-3.x86_64           4.5 M
 gpfs.gskit                                  x86_64           8.0.50-47                          /gpfs.gskit-8.0.50-47.x86_64                                 28 M
 gpfs.msg.en_US                              noarch           4.2.0-3                            /gpfs.msg.en_US-4.2.0-3.noarch                              646 k
Installing for dependencies:
 net-tools                                   x86_64           2.0-0.17.20131004git.el7           local                                                       304 k

Transaction Summary
===================================================================================================================================================================
Install  7 Packages (+1 Dependent package)

Total size: 91 M
Total download size: 304 k
Installed size: 92 M
Downloading packages:
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : net-tools-2.0-0.17.20131004git.el7.x86_64                                                                                                       1/8 
  Installing : gpfs.gskit-8.0.50-47.x86_64                                                                                                                     2/8 
  Installing : gpfs.msg.en_US-4.2.0-3.noarch                                                                                                                   3/8 
  Installing : gpfs.base-4.2.0-3.x86_64                                                                                                                        4/8 
Created symlink from /etc/systemd/system/multi-user.target.wants/gpfs.service to /usr/lpp/mmfs/lib/systemd/gpfs.service.
Created symlink from /etc/systemd/system/gpfs.service to /usr/lpp/mmfs/lib/systemd/gpfs.service.
  Installing : gpfs.ext-4.2.0-3.x86_64                                                                                                                         5/8 
  Installing : gpfs.crypto-4.2.0-3.x86_64                                                                                                                      6/8 
  Installing : gpfs.gplbin-3.10.0-327.el7.x86_64-4.2.0-3.x86_64                                                                                                7/8 
  Installing : gpfs.gpl-4.2.0-3.noarch                                                                                                                         8/8 
  Verifying  : gpfs.base-4.2.0-3.x86_64                                                                                                                        1/8 
  Verifying  : gpfs.msg.en_US-4.2.0-3.noarch                                                                                                                   2/8 
  Verifying  : gpfs.gplbin-3.10.0-327.el7.x86_64-4.2.0-3.x86_64                                                                                                3/8 
  Verifying  : gpfs.gskit-8.0.50-47.x86_64                                                                                                                     4/8 
  Verifying  : net-tools-2.0-0.17.20131004git.el7.x86_64                                                                                                       5/8 
  Verifying  : gpfs.crypto-4.2.0-3.x86_64                                                                                                                      6/8 
  Verifying  : gpfs.ext-4.2.0-3.x86_64                                                                                                                         7/8 
  Verifying  : gpfs.gpl-4.2.0-3.noarch                                                                                                                         8/8 

Installed:
  gpfs.base.x86_64 0:4.2.0-3                              gpfs.crypto.x86_64 0:4.2.0-3       gpfs.ext.x86_64 0:4.2.0-3            gpfs.gpl.noarch 0:4.2.0-3     
  gpfs.gplbin-3.10.0-327.el7.x86_64.x86_64 0:4.2.0-3      gpfs.gskit.x86_64 0:8.0.50-47      gpfs.msg.en_US.noarch 0:4.2.0-3     

Dependency Installed:
  net-tools.x86_64 0:2.0-0.17.20131004git.el7                                                                                                                      

Complete!
```

## 三、GPFS集群配置

### 1、创建集群

节点gpfs-01编辑nodefile

```
[root@gpfs-01 4.2.0.3]# cat /root/gpfs/nodefile
gpfs-01:quorum-manager
gpfs-02:quorum-manager
```

关闭firewalld防火墙服务；节点gpfs-02重复一遍操作

```
[root@gpfs-01 4.2.0.3]# systemctl stop firewalld
[root@gpfs-01 4.2.0.3]# systemctl disable firewalld
Removed symlink /etc/systemd/system/dbus-org.fedoraproject.FirewallD1.service.
Removed symlink /etc/systemd/system/basic.target.wants/firewalld.service.
```

节点gpfs-01创建集群命令

```
mmcrcluster -N /root/gpfs/nodefile -p gpfs-01 -s gpfs-02 -C gpfscluster -A -r /usr/bin/ssh -R /usr/bin/scp
```

```
[root@gpfs-01 4.2.0.3]# mmcrcluster -N /root/gpfs/nodefile -p gpfs-01 -s gpfs-02 -C gpfscluster -A -r /usr/bin/ssh -R /usr/bin/scp
mmcrcluster: Performing preliminary node verification ...
mmcrcluster: Processing quorum and other critical nodes ...
mmcrcluster: Finalizing the cluster data structures ...
mmcrcluster: Command successfully completed
mmcrcluster: Warning: Not all nodes have proper GPFS license designations.
    Use the mmchlicense command to designate licenses as needed.
mmcrcluster: Propagating the cluster configuration data to all
  affected nodes.  This is an asynchronous process.
```

### 2、接受许可

```
mmchlicense server --accept -N all
```

```
[root@gpfs-01 4.2.0.3]# mmchlicense server --accept -N all

The following nodes will be designated as possessing server licenses:
        gpfs-01
        gpfs-02
mmchlicense: Command successfully completed
mmchlicense: Propagating the cluster configuration data to all
  affected nodes.  This is an asynchronous process.
```

### 3、查看集群信息

```
mmlscluster
```

```
[root@gpfs-01 4.2.0.3]# mmlscluster

GPFS cluster information
========================
  GPFS cluster name:         gpfscluster.gpfs-01
  GPFS cluster id:           5617289051275609157
  GPFS UID domain:           gpfscluster.gpfs-01
  Remote shell command:      /usr/bin/ssh
  Remote file copy command:  /usr/bin/scp
  Repository type:           CCR

 Node  Daemon node name  IP address       Admin node name  Designation
-----------------------------------------------------------------------
   1   gpfs-01           192.168.196.150  gpfs-01          quorum-manager
   2   gpfs-02           192.168.196.160  gpfs-02          quorum-manager
```

```
[root@gpfs-01 4.2.0.3]# mmlscluster --ces

GPFS cluster information
========================
  GPFS cluster name:         gpfscluster.gpfs-01
  GPFS cluster id:           5617289051275609157

mmlscluster:  There are no Cluster Export Services nodes defined.

[root@gpfs-01 4.2.0.3]# 
[root@gpfs-01 4.2.0.3]# mmlscluster --cnfs

GPFS cluster information
========================
  GPFS cluster name:         gpfscluster.gpfs-01
  GPFS cluster id:           5617289051275609157

mmlscluster:  CNFS is not defined in this cluster.
```

### 4、启动集群

**注意：启动前，有bug需要提前处理，不然会出现以下情况--机器报错卡死**

```
[root@gpfs-01 ~]# mmstartup -N gpfs-01
Wed May 15 14:38:04 CST 2024: mmstartup: Starting GPFS ...

Message from syslogd@gpfs-01 at May 15 14:39:14 ...
 kernel:BUG: soft lockup - CPU#1 stuck for 22s! [mmfsd:13409]

Message from syslogd@gpfs-01 at May 15 14:39:46 ...
 kernel:BUG: soft lockup - CPU#1 stuck for 22s! [mmfsd:13409]

Message from syslogd@gpfs-01 at May 15 14:40:14 ...
 kernel:BUG: soft lockup - CPU#1 stuck for 22s! [mmfsd:13409]

Message from syslogd@gpfs-01 at May 15 14:40:42 ...
 kernel:BUG: soft lockup - CPU#1 stuck for 22s! [mmfsd:13409]
 ...
```

查询官方 QA

```
https://www.ibm.com/support/pages/ibm-spectrum-scale-support-supervisor-mode-access-prevention-smap-feature-intel-xeon-processors
```

```
查看 CPU 是否开启 SMAP 功能
cat /proc/cpuinfo | grep smap
```

```
[root@gpfs-01 4.2.0.3]# cat /proc/cpuinfo | grep smap
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ss syscall nx pdpe1gb rdtscp lm constant_tsc arch_perfmon rep_good nopl xtopology tsc_reliable nonstop_tsc eagerfpu pni pclmulqdq ssse3 fma cx16 pcid sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand hypervisor lahf_lm abm 3dnowprefetch arat fsgsbase tsc_adjust bmi1 avx2 smep bmi2 erms invpcid avx512f rdseed adx smap clflushopt avx512cd xsaveopt xsavec xgetbv1 xsaves
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ss syscall nx pdpe1gb rdtscp lm constant_tsc arch_perfmon rep_good nopl xtopology tsc_reliable nonstop_tsc eagerfpu pni pclmulqdq ssse3 fma cx16 pcid sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand hypervisor lahf_lm abm 3dnowprefetch arat fsgsbase tsc_adjust bmi1 avx2 smep bmi2 erms invpcid avx512f rdseed adx smap clflushopt avx512cd xsaveopt xsavec xgetbv1 xsaves
```

```
[root@gpfs-02 gpfs]# cat /proc/cpuinfo | grep smap
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ss syscall nx pdpe1gb rdtscp lm constant_tsc arch_perfmon rep_good nopl xtopology tsc_reliable nonstop_tsc eagerfpu pni pclmulqdq ssse3 fma cx16 pcid sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand hypervisor lahf_lm abm 3dnowprefetch arat fsgsbase tsc_adjust bmi1 avx2 smep bmi2 erms invpcid avx512f rdseed adx smap clflushopt avx512cd xsaveopt xsavec xgetbv1 xsaves
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ss syscall nx pdpe1gb rdtscp lm constant_tsc arch_perfmon rep_good nopl xtopology tsc_reliable nonstop_tsc eagerfpu pni pclmulqdq ssse3 fma cx16 pcid sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand hypervisor lahf_lm abm 3dnowprefetch arat fsgsbase tsc_adjust bmi1 avx2 smep bmi2 erms invpcid avx512f rdseed adx smap clflushopt avx512cd xsaveopt xsavec xgetbv1 xsaves
```

#### 关闭 SMAP

编辑/etc/default/grub文件，GRUB_CMDLINE_LINUX 后添加 nosmap

```
[root@gpfs-01 4.2.0.3]# cat /etc/default/grub
GRUB_TIMEOUT=5
GRUB_DISTRIBUTOR="$(sed 's, release .*$,,g' /etc/system-release)"
GRUB_DEFAULT=saved
GRUB_DISABLE_SUBMENU=true
GRUB_TERMINAL_OUTPUT="console"
GRUB_CMDLINE_LINUX="crashkernel=auto rd.lvm.lv=rhel/root rd.lvm.lv=rhel/swap rhgb quiet nosmap"
GRUB_DISABLE_RECOVERY="true"
```

重新生成启动文件

```
grub2-mkconfig -o /boot/grub2/grub.cfg
```

```
[root@gpfs-01 4.2.0.3]# grub2-mkconfig -o /boot/grub2/grub.cfg
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-3.10.0-327.el7.x86_64
Found initrd image: /boot/initramfs-3.10.0-327.el7.x86_64.img
Found linux image: /boot/vmlinuz-0-rescue-906166d8d0654d40a90291df33fe3e5b
Found initrd image: /boot/initramfs-0-rescue-906166d8d0654d40a90291df33fe3e5b.img
done
```

重启服务器！！！

节点gpfs-02 重复一遍操作



重启后，正式启动集群

```
mmstartup -N gpfs-01
mmgetstate -aLs
```

```
[root@gpfs-01 ~]# mmstartup -N gpfs-01
Fri May 17 11:35:05 CST 2024: mmstartup: Starting GPFS ...
gpfs-01:  The GPFS subsystem is already active.
[root@gpfs-01 ~]# 
[root@gpfs-01 ~]# 
[root@gpfs-01 ~]# mmgetstate -aLs

 Node number  Node name       Quorum  Nodes up  Total nodes  GPFS state  Remarks    
------------------------------------------------------------------------------------
       1      gpfs-01            2        2          2       active      quorum node
       2      gpfs-02            2        2          2       active      quorum node

 Summary information 
---------------------
Number of nodes defined in the cluster:            2
Number of local nodes active in the cluster:       2
Number of remote nodes joined in this cluster:     0
Number of quorum nodes defined in the cluster:     2
Number of quorum nodes active in the cluster:      2
Quorum = 2, Quorum achieved
```

### 5、配置GPFS文件系统

#### 5.1在线扫盘

```
cd /sys/class/scsi_host/
for i in `ls` ;do echo '- - -' > $i/scan ;done
```

```
[root@gpfs-01 ~]# cd /sys/class/scsi_host/
[root@gpfs-01 scsi_host]# for i in `ls` ;do echo '- - -' > $i/scan ;done
[root@gpfs-01 scsi_host]# 
[root@gpfs-01 scsi_host]# lsblk
NAME          MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda             8:0    0   10G  0 disk 
├─sda1          8:1    0  200M  0 part /boot
└─sda2          8:2    0  9.8G  0 part 
  ├─rhel-root 253:0    0    8G  0 lvm  /
  └─rhel-swap 253:1    0    1G  0 lvm  [SWAP]
sdb             8:16   0    5G  0 disk 
sr0            11:0    1  3.8G  0 rom  
```

节点gpfs-02重复操作一遍

#### 5.2编辑磁盘配置文件

```
[root@gpfs-01 scsi_host]# cat /root/gpfs/nsdfiles
/dev/sdb:gpfs-01::dataAndMetadata:01:
/dev/sdb:gpfs-02::dataAndMetadata:02:
```

节点gpfs-02无需重复操作

#### 5.3生成NSD磁盘

```
mmcrnsd -F /root/gpfs/nsdfiles -v no
mmlsnsd -m
```

```
[root@gpfs-01 scsi_host]# mmcrnsd -F /root/gpfs/nsdfiles -v no
mmcrnsd: Processing disk sdb
mmcrnsd: Processing disk sdb
mmcrnsd: Propagating the cluster configuration data to all
  affected nodes.  This is an asynchronous process.
[root@gpfs-01 scsi_host]# 
[root@gpfs-01 scsi_host]# mmlsnsd -m

 Disk name    NSD volume ID      Device         Node name                Remarks       
---------------------------------------------------------------------------------------
 gpfs1nsd     C0A8C4966646F9D6   /dev/sdb       gpfs-01                  server node
 gpfs2nsd     C0A8C4A06646F9D8   /dev/sdb       gpfs-02                  server node
[root@gpfs-01 scsi_host]# 
[root@gpfs-01 scsi_host]# cat /root/gpfs/nsdfiles 
# /dev/sdb:gpfs-01::dataAndMetadata:01:
gpfs1nsd:::dataAndMetadata:01::system
# /dev/sdb:gpfs-02::dataAndMetadata:02:
gpfs2nsd:::dataAndMetadata:02::system
```

节点gpfs-02无需重复操作

#### 5.4创建GPFS文件系统

```
mmcrfs gpfs -F /root/gpfs/nsdfiles -B 512K -m 2 -r 2 -Q yes -T /gpfs
mmcrfs 磁盘描述 -F NSD 配置文件 -B 块大小 -m inode 的默认副本数 -r 每个数据块的副本数文件 -Q 挂载文件系统时自动激活配额 -T 挂载点
```

```
[root@gpfs-01 scsi_host]# mmcrfs gpfs -F /root/gpfs/nsdfiles -B 512K -m 2 -r 2 -Q yes -T /gpfs

The following disks of gpfs will be formatted on node gpfs-01:
    gpfs1nsd: size 5120 MB
    gpfs2nsd: size 5120 MB
Formatting file system ...
Disks up to size 206 GB can be added to storage pool system.
Creating Inode File
Creating Allocation Maps
Creating Log Files
Clearing Inode Allocation Map
Clearing Block Allocation Map
Formatting Allocation Map for storage pool system
Completed creation of file system /dev/gpfs.
mmcrfs: Propagating the cluster configuration data to all
  affected nodes.  This is an asynchronous process.
```

节点gpfs-02无需重复操作

#### 5.5挂载GPFS文件系统

```
mmmount /gpfs -a
```

```
[root@gpfs-01 scsi_host]# mmmount /gpfs -a
Fri May 17 14:38:28 CST 2024: mmmount: Mounting file systems ...
[root@gpfs-01 scsi_host]# 
[root@gpfs-01 scsi_host]# df -Th
Filesystem            Type      Size  Used Avail Use% Mounted on
/dev/mapper/rhel-root xfs       8.0G  6.1G  2.0G  77% /
devtmpfs              devtmpfs  903M     0  903M   0% /dev
tmpfs                 tmpfs     913M     0  913M   0% /dev/shm
tmpfs                 tmpfs     913M  8.6M  904M   1% /run
tmpfs                 tmpfs     913M     0  913M   0% /sys/fs/cgroup
/dev/sda1             xfs       197M  111M   87M  57% /boot
tmpfs                 tmpfs     183M     0  183M   0% /run/user/0
/dev/gpfs             gpfs       10G  922M  9.1G  10% /gpfs
```

```
[root@gpfs-02 scsi_host]# df -Th
Filesystem            Type      Size  Used Avail Use% Mounted on
/dev/mapper/rhel-root xfs       8.0G  5.1G  3.0G  64% /
devtmpfs              devtmpfs  903M     0  903M   0% /dev
tmpfs                 tmpfs     913M     0  913M   0% /dev/shm
tmpfs                 tmpfs     913M  8.6M  904M   1% /run
tmpfs                 tmpfs     913M     0  913M   0% /sys/fs/cgroup
/dev/sda1             xfs       197M  111M   87M  57% /boot
tmpfs                 tmpfs     183M     0  183M   0% /run/user/0
/dev/gpfs             gpfs       10G  922M  9.1G  10% /gpfs
```

GPFS文件系统容量为10G

#### 5.6关闭GPFS集群

```
mmshutdown -a
```

```
[root@gpfs-01 scsi_host]# mmshutdown -a
Fri May 17 14:41:31 CST 2024: mmshutdown: Starting force unmount of GPFS file systems
Fri May 17 14:41:36 CST 2024: mmshutdown: Shutting down GPFS daemons
gpfs-01:  Shutting down!
gpfs-02:  Shutting down!
gpfs-01:  'shutdown' command about to kill process 4280
gpfs-01:  Unloading modules from /lib/modules/3.10.0-327.el7.x86_64/extra
gpfs-02:  'shutdown' command about to kill process 4336
gpfs-02:  Unloading modules from /lib/modules/3.10.0-327.el7.x86_64/extra
gpfs-01:  Unloading module mmfs26
gpfs-02:  Unloading module mmfs26
gpfs-01:  Unloading module mmfslinux
gpfs-02:  Unloading module mmfslinux
Fri May 17 14:41:44 CST 2024: mmshutdown: Finished
[root@gpfs-01 scsi_host]# 
[root@gpfs-01 scsi_host]# df -Th
Filesystem            Type      Size  Used Avail Use% Mounted on
/dev/mapper/rhel-root xfs       8.0G  6.1G  2.0G  77% /
devtmpfs              devtmpfs  903M     0  903M   0% /dev
tmpfs                 tmpfs     913M     0  913M   0% /dev/shm
tmpfs                 tmpfs     913M  8.6M  904M   1% /run
tmpfs                 tmpfs     913M     0  913M   0% /sys/fs/cgroup
/dev/sda1             xfs       197M  111M   87M  57% /boot
tmpfs                 tmpfs     183M     0  183M   0% /run/user/0
```

```
[root@gpfs-02 scsi_host]# df -Th
Filesystem            Type      Size  Used Avail Use% Mounted on
/dev/mapper/rhel-root xfs       8.0G  5.1G  3.0G  64% /
devtmpfs              devtmpfs  903M     0  903M   0% /dev
tmpfs                 tmpfs     913M     0  913M   0% /dev/shm
tmpfs                 tmpfs     913M  8.6M  904M   1% /run
tmpfs                 tmpfs     913M     0  913M   0% /sys/fs/cgroup
/dev/sda1             xfs       197M  111M   87M  57% /boot
tmpfs                 tmpfs     183M     0  183M   0% /run/user/0
```

## 四、GPFS磁盘扩容

### 1、在线扫盘

```
cd /sys/class/scsi_host/
for i in `ls` ;do echo '- - -' > $i/scan ;done
```

```
[root@gpfs-01 scsi_host]# cd /sys/class/scsi_host/
[root@gpfs-01 scsi_host]# for i in `ls` ;do echo '- - -' > $i/scan ;done
[root@gpfs-01 scsi_host]# 
[root@gpfs-01 scsi_host]# 
[root@gpfs-01 scsi_host]# lsblk
NAME          MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda             8:0    0   10G  0 disk 
├─sda1          8:1    0  200M  0 part /boot
└─sda2          8:2    0  9.8G  0 part 
  ├─rhel-root 253:0    0    8G  0 lvm  /
  └─rhel-swap 253:1    0    1G  0 lvm  [SWAP]
sdb             8:16   0    5G  0 disk 
└─sdb1          8:17   0    5G  0 part 
sdc             8:32   0    5G  0 disk 
sr0            11:0    1  3.8G  0 rom  
```

节点gpfs-02重复操作一遍

### 2、编辑新的NSD文件

```
[root@gpfs-01 scsi_host]# cat /root/gpfs/nsdfiles2
/dev/sdc:gpfs-01::dataAndMetadata:01:
/dev/sdc:gpfs-02::dataAndMetadata:02:
```

### 3、生成新的NSD磁盘

```
mmcrnsd -F /root/gpfs/nsdfiles2
```

```
[root@gpfs-01 scsi_host]# mmcrnsd -F /root/gpfs/nsdfiles2
mmcrnsd: Processing disk sdc
mmcrnsd: Processing disk sdc
mmcrnsd: Propagating the cluster configuration data to all
  affected nodes.  This is an asynchronous process.
[root@gpfs-01 scsi_host]# 
[root@gpfs-01 scsi_host]# cat /root/gpfs/nsdfiles2
# /dev/sdc:gpfs-01::dataAndMetadata:01:
gpfs3nsd:::dataAndMetadata:01::system
# /dev/sdc:gpfs-02::dataAndMetadata:02:
gpfs4nsd:::dataAndMetadata:02::system
```

### 4、查看NSD磁盘

```
mmlsnsd
```

```
[root@gpfs-01 scsi_host]# mmlsnsd

 File system   Disk name    NSD servers                                    
---------------------------------------------------------------------------
 gpfs          gpfs1nsd     gpfs-01                  
 gpfs          gpfs2nsd     gpfs-02                  
 (free disk)   gpfs3nsd     gpfs-01                  
 (free disk)   gpfs4nsd     gpfs-02                  

[root@gpfs-01 scsi_host]# 
[root@gpfs-01 scsi_host]# mmlsdisk gpfs
disk         driver   sector     failure holds    holds                            storage
name         type       size       group metadata data  status        availability pool
------------ -------- ------ ----------- -------- ----- ------------- ------------ ------------
gpfs1nsd     nsd         512          01 Yes      Yes   ready         up           system       
gpfs2nsd     nsd         512          02 Yes      Yes   ready         up           system
```

### 5、扩容NSD磁盘

```
mmadddisk gpfs -F /root/gpfs/nsdfiles2
```

```
[root@gpfs-01 scsi_host]# mmadddisk gpfs -F /root/gpfs/nsdfiles2

The following disks of gpfs will be formatted on node gpfs-01:
    gpfs3nsd: size 5120 MB
    gpfs4nsd: size 5120 MB
Extending Allocation Map
Checking Allocation Map for storage pool system
Completed adding disks to file system gpfs.
mmadddisk: Propagating the cluster configuration data to all
  affected nodes.  This is an asynchronous process.
[root@gpfs-01 scsi_host]# 
[root@gpfs-01 scsi_host]# mmlsnsd 

 File system   Disk name    NSD servers                                    
---------------------------------------------------------------------------
 gpfs          gpfs1nsd     gpfs-01                  
 gpfs          gpfs2nsd     gpfs-02                  
 gpfs          gpfs3nsd     gpfs-01                  
 gpfs          gpfs4nsd     gpfs-02                  

[root@gpfs-01 scsi_host]# 
[root@gpfs-01 scsi_host]# mmlsdisk gpfs
disk         driver   sector     failure holds    holds                            storage
name         type       size       group metadata data  status        availability pool
------------ -------- ------ ----------- -------- ----- ------------- ------------ ------------
gpfs1nsd     nsd         512          01 Yes      Yes   ready         up           system       
gpfs2nsd     nsd         512          02 Yes      Yes   ready         up           system       
gpfs3nsd     nsd         512          01 Yes      Yes   ready         up           system       
gpfs4nsd     nsd         512          02 Yes      Yes   ready         up           system 
[root@gpfs-01 scsi_host]# df -h
Filesystem             Size  Used Avail Use% Mounted on
/dev/mapper/rhel-root  8.0G  6.1G  2.0G  77% /
devtmpfs               903M     0  903M   0% /dev
tmpfs                  913M     0  913M   0% /dev/shm
tmpfs                  913M  8.6M  904M   1% /run
tmpfs                  913M     0  913M   0% /sys/fs/cgroup
/dev/sda1              197M  111M   87M  57% /boot
tmpfs                  183M     0  183M   0% /run/user/0
/dev/gpfs               20G  1.1G   19G   6% /gpfs
```

GPFS文件系统容量扩大为20G

### 6、rebalance文件系统

```
mmrestripefs gpfs -b
```

```
[root@gpfs-01 scsi_host]# mmdf gpfs -d
disk                disk size  failure holds    holds              free KB             free KB
name                    in KB    group metadata data        in full blocks        in fragments
--------------- ------------- -------- -------- ----- -------------------- -------------------
Disks in storage pool: system (Maximum disk size allowed is 191 GB)
gpfs1nsd              5242880       01 Yes      Yes         4770304 ( 91%)          1120 ( 0%) 
gpfs3nsd              5242880       01 Yes      Yes         5176320 ( 99%)           880 ( 0%) 
gpfs2nsd              5242880       02 Yes      Yes         4770304 ( 91%)          1120 ( 0%) 
gpfs4nsd              5242880       02 Yes      Yes         5176320 ( 99%)           880 ( 0%) 
                -------------                         -------------------- -------------------
(pool total)         20971520                              19893248 ( 95%)          4000 ( 0%)

                =============                         ==================== ===================
(total)              20971520                              19893248 ( 95%)          4000 ( 0%)
[root@gpfs-01 scsi_host]# 
[root@gpfs-01 scsi_host]# mmrestripefs gpfs -b
Scanning file system metadata, phase 1 ... 
Scan completed successfully.
Scanning file system metadata, phase 2 ... 
Scan completed successfully.
Scanning file system metadata, phase 3 ... 
Scan completed successfully.
Scanning file system metadata, phase 4 ... 
Scan completed successfully.
Scanning user file metadata ...
 100.00 % complete on Fri May 17 15:39:49 2024  (     65792 inodes with total        570 MB data processed)
Scan completed successfully.
[root@gpfs-01 scsi_host]# 
[root@gpfs-01 scsi_host]# mmdf gpfs -d        
disk                disk size  failure holds    holds              free KB             free KB
name                    in KB    group metadata data        in full blocks        in fragments
--------------- ------------- -------- -------- ----- -------------------- -------------------
Disks in storage pool: system (Maximum disk size allowed is 191 GB)
gpfs1nsd              5242880       01 Yes      Yes         4946432 ( 94%)          1120 ( 0%) 
gpfs3nsd              5242880       01 Yes      Yes         4961792 ( 95%)          1632 ( 0%) 
gpfs2nsd              5242880       02 Yes      Yes         4945408 ( 94%)          1120 ( 0%) 
gpfs4nsd              5242880       02 Yes      Yes         4964864 ( 95%)          1376 ( 0%) 
                -------------                         -------------------- -------------------
(pool total)         20971520                              19818496 ( 95%)          5248 ( 0%)

                =============                         ==================== ===================
(total)              20971520                              19818496 ( 95%)          5248 ( 0%)
```

### 7、动态减盘

```
mmrestripefs gpfs -b
mmdeldisk gpfs gpfs4nsd
```

```
[root@gpfs-01 scsi_host]# mmrestripefs gpfs -b
Scanning file system metadata, phase 1 ... 
Scan completed successfully.
Scanning file system metadata, phase 2 ... 
Scan completed successfully.
Scanning file system metadata, phase 3 ... 
Scan completed successfully.
Scanning file system metadata, phase 4 ... 
Scan completed successfully.
Scanning user file metadata ...
 100.00 % complete on Fri May 17 15:41:39 2024  (     65792 inodes with total        565 MB data processed)
Scan completed successfully.
[root@gpfs-01 scsi_host]# 
[root@gpfs-01 scsi_host]# mmdeldisk gpfs gpfs4nsd
Deleting disks ...
Scanning file system metadata, phase 1 ... 
Scan completed successfully.
Scanning file system metadata, phase 2 ... 
Scan completed successfully.
Scanning file system metadata, phase 3 ... 
Scan completed successfully.
Scanning file system metadata, phase 4 ... 
Scan completed successfully.
Scanning user file metadata ...
 100.00 % complete on Fri May 17 15:41:55 2024  (     65792 inodes with total        570 MB data processed)
Scan completed successfully.
Checking Allocation Map for storage pool system
tsdeldisk completed.
mmdeldisk: Propagating the cluster configuration data to all
  affected nodes.  This is an asynchronous process.
```

```
[root@gpfs-01 scsi_host]# mmlsnsd 

 File system   Disk name    NSD servers                                    
---------------------------------------------------------------------------
 gpfs          gpfs1nsd     gpfs-01                  
 gpfs          gpfs2nsd     gpfs-02                  
 gpfs          gpfs3nsd     gpfs-01                  
 (free disk)   gpfs4nsd     gpfs-02                  

[root@gpfs-01 scsi_host]# 
[root@gpfs-01 scsi_host]# mmlsdisk gpfs
disk         driver   sector     failure holds    holds                            storage
name         type       size       group metadata data  status        availability pool
------------ -------- ------ ----------- -------- ----- ------------- ------------ ------------
gpfs1nsd     nsd         512          01 Yes      Yes   ready         up           system       
gpfs2nsd     nsd         512          02 Yes      Yes   ready         up           system       
gpfs3nsd     nsd         512          01 Yes      Yes   ready         up           system
[root@gpfs-01 scsi_host]# mmdf gpfs -d
disk                disk size  failure holds    holds              free KB             free KB
name                    in KB    group metadata data        in full blocks        in fragments
--------------- ------------- -------- -------- ----- -------------------- -------------------
Disks in storage pool: system (Maximum disk size allowed is 191 GB)
gpfs1nsd              5242880       01 Yes      Yes         4953600 ( 94%)           880 ( 0%) 
gpfs3nsd              5242880       01 Yes      Yes         4992512 ( 95%)          1632 ( 0%) 
gpfs2nsd              5242880       02 Yes      Yes         4769792 ( 91%)          1632 ( 0%) 
                -------------                         -------------------- -------------------
(pool total)         15728640                              14715904 ( 94%)          4144 ( 0%)

                =============                         ==================== ===================
(total)              15728640                              14715904 ( 94%)          4144 ( 0%)
[root@gpfs-01 scsi_host]# 
[root@gpfs-01 scsi_host]# df -h
Filesystem             Size  Used Avail Use% Mounted on
/dev/mapper/rhel-root  8.0G  6.1G  2.0G  77% /
devtmpfs               903M     0  903M   0% /dev
tmpfs                  913M     0  913M   0% /dev/shm
tmpfs                  913M  8.6M  904M   1% /run
tmpfs                  913M     0  913M   0% /sys/fs/cgroup
/dev/sda1              197M  111M   87M  57% /boot
tmpfs                  183M     0  183M   0% /run/user/0
/dev/gpfs               15G  989M   15G   7% /gpfs
```

GPFS文件系统容量缩减为15G

**注意：现将gpfs4nsd 重新加回去**

新建/root/gpfs/nsdfiles3文件

```
[root@gpfs-01 scsi_host]# cat /root/gpfs/nsdfiles3
gpfs4nsd:::dataAndMetadata:02::system
```

```
mmadddisk gpfs -F /root/gpfs/nsdfiles3
```

```
[root@gpfs-01 scsi_host]# mmadddisk gpfs -F /root/gpfs/nsdfiles3

The following disks of gpfs will be formatted on node gpfs-01:
    gpfs4nsd: size 5120 MB
Extending Allocation Map
Checking Allocation Map for storage pool system
Completed adding disks to file system gpfs.
mmadddisk: Propagating the cluster configuration data to all
  affected nodes.  This is an asynchronous process.
```

```
[root@gpfs-01 scsi_host]# mmlsnsd 

 File system   Disk name    NSD servers                                    
---------------------------------------------------------------------------
 gpfs          gpfs1nsd     gpfs-01                  
 gpfs          gpfs2nsd     gpfs-02                  
 gpfs          gpfs3nsd     gpfs-01                  
 gpfs          gpfs4nsd     gpfs-02                  

[root@gpfs-01 scsi_host]# 
[root@gpfs-01 scsi_host]# mmlsdisk gpfs
disk         driver   sector     failure holds    holds                            storage
name         type       size       group metadata data  status        availability pool
------------ -------- ------ ----------- -------- ----- ------------- ------------ ------------
gpfs1nsd     nsd         512          01 Yes      Yes   ready         up           system       
gpfs2nsd     nsd         512          02 Yes      Yes   ready         up           system       
gpfs3nsd     nsd         512          01 Yes      Yes   ready         up           system       
gpfs4nsd     nsd         512          02 Yes      Yes   ready         up           system       
[root@gpfs-01 scsi_host]# 
[root@gpfs-01 scsi_host]# mmdf gpfs -d
disk                disk size  failure holds    holds              free KB             free KB
name                    in KB    group metadata data        in full blocks        in fragments
--------------- ------------- -------- -------- ----- -------------------- -------------------
Disks in storage pool: system (Maximum disk size allowed is 191 GB)
gpfs1nsd              5242880       01 Yes      Yes         4953600 ( 94%)           880 ( 0%) 
gpfs3nsd              5242880       01 Yes      Yes         4992512 ( 95%)          1632 ( 0%) 
gpfs2nsd              5242880       02 Yes      Yes         4769792 ( 91%)          1632 ( 0%) 
gpfs4nsd              5242880       02 Yes      Yes         5176320 ( 99%)           880 ( 0%) 
                -------------                         -------------------- -------------------
(pool total)         20971520                              19892224 ( 95%)          5024 ( 0%)

                =============                         ==================== ===================
(total)              20971520                              19892224 ( 95%)          5024 ( 0%)
[root@gpfs-01 scsi_host]# 
[root@gpfs-01 scsi_host]# mmrestripefs gpfs -b
Scanning file system metadata, phase 1 ... 
Scan completed successfully.
Scanning file system metadata, phase 2 ... 
Scan completed successfully.
Scanning file system metadata, phase 3 ... 
Scan completed successfully.
Scanning file system metadata, phase 4 ... 
Scan completed successfully.
Scanning user file metadata ...
 100.00 % complete on Fri May 17 15:53:07 2024  (     65792 inodes with total        570 MB data processed)
Scan completed successfully.
[root@gpfs-01 scsi_host]# 
[root@gpfs-01 scsi_host]# mmdf gpfs -d        
disk                disk size  failure holds    holds              free KB             free KB
name                    in KB    group metadata data        in full blocks        in fragments
--------------- ------------- -------- -------- ----- -------------------- -------------------
Disks in storage pool: system (Maximum disk size allowed is 191 GB)
gpfs1nsd              5242880       01 Yes      Yes         5105152 ( 97%)           880 ( 0%) 
gpfs3nsd              5242880       01 Yes      Yes         4835328 ( 92%)          1632 ( 0%) 
gpfs2nsd              5242880       02 Yes      Yes         5103616 ( 97%)          1136 ( 0%) 
gpfs4nsd              5242880       02 Yes      Yes         4835328 ( 92%)          1632 ( 0%) 
                -------------                         -------------------- -------------------
(pool total)         20971520                              19879424 ( 95%)          5280 ( 0%)

                =============                         ==================== ===================
(total)              20971520                              19879424 ( 95%)          5280 ( 0%)
[root@gpfs-01 scsi_host]# 
[root@gpfs-01 scsi_host]# df -h
Filesystem             Size  Used Avail Use% Mounted on
/dev/mapper/rhel-root  8.0G  6.1G  2.0G  77% /
devtmpfs               903M     0  903M   0% /dev
tmpfs                  913M     0  913M   0% /dev/shm
tmpfs                  913M  8.6M  904M   1% /run
tmpfs                  913M     0  913M   0% /sys/fs/cgroup
/dev/sda1              197M  111M   87M  57% /boot
tmpfs                  183M     0  183M   0% /run/user/0
/dev/gpfs               20G  1.1G   19G   6% /gpfs
```

## 五、其他命令的简单使用

1、查看文件系统

```
[root@gpfs-01 scsi_host]# mmdf gpfs -d
disk                disk size  failure holds    holds              free KB             free KB
name                    in KB    group metadata data        in full blocks        in fragments
--------------- ------------- -------- -------- ----- -------------------- -------------------
Disks in storage pool: system (Maximum disk size allowed is 191 GB)
gpfs1nsd              5242880       01 Yes      Yes         5108224 ( 97%)           880 ( 0%) 
gpfs3nsd              5242880       01 Yes      Yes         4837888 ( 92%)          1632 ( 0%) 
gpfs2nsd              5242880       02 Yes      Yes         5110784 ( 97%)           880 ( 0%) 
gpfs4nsd              5242880       02 Yes      Yes         4835328 ( 92%)          1632 ( 0%) 
                -------------                         -------------------- -------------------
(pool total)         20971520                              19892224 ( 95%)          5024 ( 0%)

                =============                         ==================== ===================
(total)              20971520                              19892224 ( 95%)          5024 ( 0%)
```

2、查看文件系统配置信息

```
[root@gpfs-01 scsi_host]# mmlsfs gpfs
flag                value                    description
------------------- ------------------------ -----------------------------------
 -f                 16384                    Minimum fragment size in bytes
 -i                 4096                     Inode size in bytes
 -I                 16384                    Indirect block size in bytes
 -m                 2                        Default number of metadata replicas
 -M                 2                        Maximum number of metadata replicas
 -r                 2                        Default number of data replicas
 -R                 2                        Maximum number of data replicas
 -j                 cluster                  Block allocation type
 -D                 nfs4                     File locking semantics in effect
 -k                 all                      ACL semantics in effect
 -n                 32                       Estimated number of nodes that will mount file system
 -B                 524288                   Block size
 -Q                 user;group;fileset       Quotas accounting enabled
                    user;group;fileset       Quotas enforced
                    none                     Default quotas enabled
 --perfileset-quota No                       Per-fileset quota enforcement
 --filesetdf        No                       Fileset df enabled?
 -V                 15.01 (4.2.0.0)          File system version
 --create-time      Fri May 17 14:37:18 2024 File system creation time
 -z                 No                       Is DMAPI enabled?
 -L                 4194304                  Logfile size
 -E                 Yes                      Exact mtime mount option
 -S                 No                       Suppress atime mount option
 -K                 whenpossible             Strict replica allocation option
 --fastea           Yes                      Fast external attributes enabled?
 --encryption       No                       Encryption enabled?
 --inode-limit      65792                    Maximum number of inodes
 --log-replicas     0                        Number of log replicas
 --is4KAligned      Yes                      is4KAligned?
 --rapid-repair     Yes                      rapidRepair enabled?
 --write-cache-threshold 0                   HAWC Threshold (max 65536)
 -P                 system                   Disk storage pools in file system
 -d                 gpfs1nsd;gpfs2nsd;gpfs3nsd;gpfs4nsd  Disks in file system
 -A                 yes                      Automatic mount option
 -o                 none                     Additional mount options
 -T                 /gpfs                    Default mount point
 --mount-priority   0                        Mount priority
```

3、查看挂载信息

```
[root@gpfs-01 scsi_host]# mmlsmount all -L
                                            
File system gpfs is mounted on 2 nodes:
  192.168.196.150 gpfs-01                   
  192.168.196.160 gpfs-02
```

4、扩容inode

查看到 --inode-limit 的值为 65792

修改--inode-limit 

mmchfs gpfs --inode-limit 66000

```
[root@gpfs-01 scsi_host]# mmlsfs gpfs
flag                value                    description
------------------- ------------------------ -----------------------------------
 -f                 16384                    Minimum fragment size in bytes
 -i                 4096                     Inode size in bytes
 -I                 16384                    Indirect block size in bytes
 -m                 2                        Default number of metadata replicas
 -M                 2                        Maximum number of metadata replicas
 -r                 2                        Default number of data replicas
 -R                 2                        Maximum number of data replicas
 -j                 cluster                  Block allocation type
 -D                 nfs4                     File locking semantics in effect
 -k                 all                      ACL semantics in effect
 -n                 32                       Estimated number of nodes that will mount file system
 -B                 524288                   Block size
 -Q                 user;group;fileset       Quotas accounting enabled
                    user;group;fileset       Quotas enforced
                    none                     Default quotas enabled
 --perfileset-quota No                       Per-fileset quota enforcement
 --filesetdf        No                       Fileset df enabled?
 -V                 15.01 (4.2.0.0)          File system version
 --create-time      Fri May 17 14:37:18 2024 File system creation time
 -z                 No                       Is DMAPI enabled?
 -L                 4194304                  Logfile size
 -E                 Yes                      Exact mtime mount option
 -S                 No                       Suppress atime mount option
 -K                 whenpossible             Strict replica allocation option
 --fastea           Yes                      Fast external attributes enabled?
 --encryption       No                       Encryption enabled?
 --inode-limit      65792                    Maximum number of inodes
 --log-replicas     0                        Number of log replicas
 --is4KAligned      Yes                      is4KAligned?
 --rapid-repair     Yes                      rapidRepair enabled?
 --write-cache-threshold 0                   HAWC Threshold (max 65536)
 -P                 system                   Disk storage pools in file system
 -d                 gpfs1nsd;gpfs2nsd;gpfs3nsd;gpfs4nsd  Disks in file system
 -A                 yes                      Automatic mount option
 -o                 none                     Additional mount options
 -T                 /gpfs                    Default mount point
 --mount-priority   0                        Mount priority
[root@gpfs-01 scsi_host]# 
[root@gpfs-01 scsi_host]# 
[root@gpfs-01 scsi_host]# mmchfs gpfs --inode-limit 66000
Set maxInodes for inode space 0 to 66048
Fileset root changed.
[root@gpfs-01 scsi_host]# mmlsfs gpfs
flag                value                    description
------------------- ------------------------ -----------------------------------
 -f                 16384                    Minimum fragment size in bytes
 -i                 4096                     Inode size in bytes
 -I                 16384                    Indirect block size in bytes
 -m                 2                        Default number of metadata replicas
 -M                 2                        Maximum number of metadata replicas
 -r                 2                        Default number of data replicas
 -R                 2                        Maximum number of data replicas
 -j                 cluster                  Block allocation type
 -D                 nfs4                     File locking semantics in effect
 -k                 all                      ACL semantics in effect
 -n                 32                       Estimated number of nodes that will mount file system
 -B                 524288                   Block size
 -Q                 user;group;fileset       Quotas accounting enabled
                    user;group;fileset       Quotas enforced
                    none                     Default quotas enabled
 --perfileset-quota No                       Per-fileset quota enforcement
 --filesetdf        No                       Fileset df enabled?
 -V                 15.01 (4.2.0.0)          File system version
 --create-time      Fri May 17 14:37:18 2024 File system creation time
 -z                 No                       Is DMAPI enabled?
 -L                 4194304                  Logfile size
 -E                 Yes                      Exact mtime mount option
 -S                 No                       Suppress atime mount option
 -K                 whenpossible             Strict replica allocation option
 --fastea           Yes                      Fast external attributes enabled?
 --encryption       No                       Encryption enabled?
 --inode-limit      66048                    Maximum number of inodes
 --log-replicas     0                        Number of log replicas
 --is4KAligned      Yes                      is4KAligned?
 --rapid-repair     Yes                      rapidRepair enabled?
 --write-cache-threshold 0                   HAWC Threshold (max 65536)
 -P                 system                   Disk storage pools in file system
 -d                 gpfs1nsd;gpfs2nsd;gpfs3nsd;gpfs4nsd  Disks in file system
 -A                 yes                      Automatic mount option
 -o                 none                     Additional mount options
 -T                 /gpfs                    Default mount point
 --mount-priority   0                        Mount priority
```

可以看到设置成了最大限制值

MaxNumInodes和 NumInodesToPreallocate 值可以指定后缀，例如 100K或 2M。请注意，为了优化文件系统操作，inode 的数量实际创建的值可能大于指定的值。

