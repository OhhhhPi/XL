
This repository is provided under the terms of MIT license.


## Contents

* General code base
  - src/combined:  input mutator
  - src/combined/FSCQ-consistency-exec.cpp: FUSE-based executor

* Checkers
  - src/emulator: in-house crash consistency checker


## System requirements

Ubuntu 18.04


## Setup

### 1. All setup should be done under `src`
```
$ cd src
```

### 2. Install dependencies
```
./dep.sh
```

### 3. Compile for each file system
```
make fscq
```



### 4. Set up environments
```
$ sudo ./prepare_fuzzing.sh
$ ./prepare_env.sh
```

### 5. Run fuzzing (single / multiple instance)

1. terminal 1
```
./combined/fscq-cc /tmp/mnt /tmp/prog ./emulator/fscq.py /tmp/mosbench/tmpfs-separate/0/disk.img 
```

2. terminal 2
```
AFL_NO_FORKSRV=1 AFL_FAST_CAL=1 ./combined/afl-fscq-syscall/afl-fuzz -S fscq1 -t 1000000 -m none -f /tmp/prog -i seed -o output -k -e /tmp/mosbench/tmpfs-separate/0/disk.img -p /tmp/mosbench/tmpfs-separate/0/tmp.img -u 1 -- ./fscq/src/fscq /tmp/mosbench/tmpfs-separate/0/tmp.img -f -o big_writes,atomic_o_trunc,use_ino /tmp/mnt
```

### 6. Important note

It is highly encouraged that you use separate input, output, log directories for each file system, unless you are running fuzzers in parallel. If you reuse the same directories from previous testings of other file systems, it won't work properly.


