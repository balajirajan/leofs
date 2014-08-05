LeoFS - The Lion of Storage Systems
===================================

![LeoFS Logo](http://leo-project.net/leofs/docs/_static/leofs-logo-small.png)

LeoFS is a highly available, distributed, eventually consistent object/blob store.
If you are searching a storage system that is able to store huge amount and various kind of files such as photo, movie, log data and so on, LeoFS is suitable for that.

LeoFS is supporting the following features:

* **S3-API Support**
  * LeoFS is an Amazon S3 compatible storage system.
  * Switch to LeoFS to decrease your cost from more expensive public-cloud solutions.
* **Large Object Support**
  * LeoFS can handle files with more than GB
* **Multi Data Center Replication**
  * LeoFS is a highly scalable, fault-tolerant distributed file system without SPOF.
  * LeoFS's cluster can be viewed as ONE-HUGE storage. It consists of a set of loosely connected nodes.
  * We can build a global scale storage system with easy operations

We can access LeoFS server using <a target="_blank" href="http://www.leofs.org/docs/s3_client.html">S3 clients and S3 client libries of each programming language</a>.

Slide
-------

The presentation - <a href="https://www.slideshare.net/rakutentech/scaling-and-high-performance-storage-system-leofs" title="Scaling and High Performance Storage System: LeoFS" target="_blank">Scaling and High Performance Storage System: LeoFS</a>  was given at Erlang User Conference 2014 in Stockholm on June 2014

GOALs
-------

* LeoFS aims to provide all of 3-HIGHs as follow:
  * HIGH Reliability
     * Nine nines - Operating ratios is 99.9999999%
  * High Scalability
     * Build huge-cluster at low cost
  * HIGH Cost Performance
     * Fast - Over 10Gbps
     * A lower cost than other storage
     * Provide easy management and easy operation

Further Reference
-------------------

* <a target="_blank" href="http://leo-project.net/leofs/docs/">LeoFS Documentation</a>.


Build LeoFS with LeoFS Packages
-------------------------------

LeoFS packages have been already provided on the Web. You're able to easily install LeoFS on your environments.

* LeoProject
    * <a target="_blank" href="http://leo-project.net/leofs/download.html">CentOS-6.x</a>
    * <a target="_blank" href="http://leo-project.net/leofs/download.html">Ubuntu-12.10, 13.10 and 14.04</a>
* Community
    * <a target="_blank" href="http://www.freshports.org/databases/leofs">FreeBSD</a>

<a target="_blank" href="http://leo-project.net/leofs/docs/getting_started.html">Here</a> is the installation manual.


Build LeoFS From Source (For Developers)
----------------------------------------

Here, we explain how to build LeoFS from source code.

First, you have to install the following packages to build Erlang and LeoFS.

```bash
## [CentOS]
$ sudo yum install libuuid-devel cmake check check-devel
## [Ubuntu]
$ sudo apt-get install build-essential libtool libncurses5-dev libssl-dev cmake check
```

Then, install Erlang.

```bash
##
## 1. Install libatomic
##
$ wget http://www.hpl.hp.com/research/linux/atomic_ops/download/libatomic_ops-7.2d.tar.gz
$ tar xzvf libatomic_ops-7.2d.tar.gz
$ cd libatomic_ops-7.2d
$ ./configure --prefix=/usr/local
$ make
$ sudo make install

##
## 2. Install Erlang (R16B03-1)
##
$ wget http://www.erlang.org/download/otp_src_R16B03-1.tar.gz
$ tar xzf otp_src_R16B03-1.tar.gz
$ cd otp_src_R16B03-1
$ ./configure --prefix=/usr/local/erlang/R16B03-1 \
              --enable-smp-support \
              --enable-m64-build \
              --enable-halfword-emulator \
              --enable-kernel-poll \
              --without-javac \
              --disable-native-libs \
              --disable-hipe \
              --disable-sctp \
              --enable-threads \
              --with-libatomic_ops=/usr/local
$ make
$ sudo make install

##
## 3. Set PATH
##
$ vi ~/.profile
    ## append the follows:
    export ERL_HOME=/usr/local/erlang/R16B03-1
    export PATH=$PATH:$ERL_HOME/bin

$ source ~/.profile
```

Then, clone source of LeoFS and libraries from GitHub.

```bash
$ git clone https://github.com/leo-project/leofs.git
$ cd leofs
$ git checkout -b develop remotes/origin/develop
$ ./rebar get-deps
$ ./git_checkout.sh develop
```

Then, build LeoFS with the following commands.

```bash
$ make clean
$ make
$ make release
```

Now, you can find the LeoFS package as follow.

```bash
$ ls package/
leo_gateway/  leo_manager_0/  leo_manager_1/  leo_storage/  README.md
```

Then, we can start and access LeoFS with the following commands.

```bash
$ package/leo_manager_0/bin/leo_manager start
$ package/leo_manager_1/bin/leo_manager start
$ package/leo_storage/bin/leo_storage start
$ package/leo_gateway/bin/leo_gateway start
$ telnet localhost 10010
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.
status
[System config]
                System version : 1.0.0
                    Cluster Id : leofs_1
                         DC Id : dc_1
                Total replicas : 1
           # of successes of R : 1
           # of successes of W : 1
           # of successes of D : 1
 # of DC-awareness replicas    : 0
                     ring size : 2^128
             Current ring hash : 941f20fc
                Prev ring hash : 000000-1
[Multi DC replication settings]
         max # of joinable DCs : 2
            # of replicas a DC : 1

[Node(s) state]
-------+--------------------------+--------------+----------------+----------------+----------------------------
 type  |           node           |    state     |  current ring  |   prev ring    |          updated at
-------+--------------------------+--------------+----------------+----------------+----------------------------
  S    | storage_0@127.0.0.1      | attached     |                |                | 2014-04-16 10:09:59 +0900
```

Build a LeoFS Cluster
---------------------

You can easily build a LeoFS cluster.

Please refer <a target="_blank" href="http://www.leofs.org/docs/getting_started.html#quick-start-2-cluster">here</a>.

Configure LeoFS
---------------

About the configuration of LeoFS, please refer <a target="_blank" href="http://www.leofs.org/docs/configuration.html">here</a>.

Benchmarking
------------

You can benchmark LeoFS with <a target="_blank" href="https://github.com/basho/basho_bench">Basho Bench</a>.

<a target="_blank" href="http://www.leofs.org/docs/benchmark.html">Here</a> is a documentation to benchmark LeoFS.

Milestones
-----------

* *DONE* - 0.16 (Oct 2013)
    * Increase compatibility S3-APIs#4
        * the bucket ACLs
    * Web GUI Console (Option)
       * Support whole LeoFS Manager's commands
* *On Going* - 1.0 (Nov 2013 - Apr 2014)
    * Multi Data Center Replication
    * Increase compatibility S3-APIs#5
        * Other bucket operations
    * QoS System Phase-1 (LeoInsight - Option)
       * Support *statistics/analyzer*
* 1.1/1.2 (May 2014 - Sept)
    * OpenStack Integration
        * Support for OpenStack Swift-API
    * Increase compatibility S3-APIs#6
        * Objects Expiration into the bucket
        * Versioning
    * Job Scheduler on the Manager
        * Support *auto-compaction*
    * QoS System Phase-2 (LeoInsight - Option)
       * Support *notifier*
    * Web GUI Console (Option)
        * LeoInsight(QoS) Integration
        * Support Log analysis/search


## Sponsors

LeoProject/LeoFS is sponsored by [Rakuten, Inc.](http://global.rakuten.com/corp/) and [Rakuten Institute of Technology](http://rit.rakuten.co.jp/).
