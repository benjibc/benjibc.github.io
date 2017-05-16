---
layout: post
title: [JetsonTX2] Getting started with Jetson TX2
---

Today I finally got started with the Jetson TX2 with all the caffe2 stuff installed! Making the note here that what is super weird to me is that GLOG will not log to console unless I enable it to also log to stderr

```
nvidia@tegra-ubuntu:~/caffe2/build/caffe2/binaries$ ./inspect_gpus --alsologtostderr=1
I0516 04:02:33.330822  2149 inspect_gpus.cc:23] Querying device ID = 0
I0516 04:02:33.331353  2149 common_gpu.cc:155] 
Device id:                     0
Major revision number:         6
Minor revision number:         2
Name:                          GP10B
Total global memory:           8235577344
Total shared memory per block: 49152
Total registers per block:     32768
Warp size:                     32
Maximum memory pitch:          2147483647
Maximum threads per block:     1024
Maximum dimension of block:    1024, 1024, 64
Maximum dimension of grid:     2147483647, 65535, 65535
Clock rate:                    1300500
Total constant memory:         65536
Texture alignment:             512
Concurrent copy and execution: Yes
Number of multiprocessors:     2
Kernel execution timeout:      No
I0516 04:02:33.331439  2149 inspect_gpus.cc:38] Access pattern: 
+ 
```
