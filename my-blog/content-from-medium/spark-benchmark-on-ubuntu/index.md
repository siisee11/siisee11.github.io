---
title: "SPARK benchmark on ubuntu"
description: "ubuntu에서 spark benchmark 실행하기"
date: "2019-11-27T02:36:25.035Z"
categories: []
published: true
canonical_link: https://medium.com/@siisee111/spark-benchmark-on-ubuntu-d01171506676
redirect_from:
  - /spark-benchmark-on-ubuntu-d01171506676
---

ubuntu에서 spark benchmark 실행하기

Spark benchmark에 대하여 잘 몰라서 추후에 계속 추가할 예정

일단 가장 쉽게 돌려볼 수 있는 방법이다.

[**IBM/spark-tpc-ds-performance-test**  
_Data Science Experience is now Watson Studio. Although some images in this code pattern may show the service as Data…_github.com](https://github.com/IBM/spark-tpc-ds-performance-test "https://github.com/IBM/spark-tpc-ds-performance-test")[](https://github.com/IBM/spark-tpc-ds-performance-test)

위의 깃헙 레파지토리를 이용하였다.

-   Git clone

```
$ git clone   https://github.com/IBM/spark-tpc-ds-performance-test
```

-   Env file setting

```
$ cd spark-tpc-ds-performance-test
$ vim bin/tpcdsenv.sh

# edit SPARK_HOME
# 'echo $SPARK_HOME' on shell would show you path
```

-   Run

```
$ bin/tpcdsspark.sh

==============================================
TPC-DS On Spark Menu
----------------------------------------------
SETUP
(1) Create spark tables
RUN
(2) Run a subset of TPC-DS queries
(3) Run All (99) TPC-DS Queries
CLEANUP
(4) Cleanup
(Q) Quit
----------------------------------------------
Please enter your choice followed by [ENTER]:
```

1을 먼저 실행하고, 2번 혹은 3번으로 밴치마크를 실행할 수 있다.
