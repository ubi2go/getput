# prerequisites

* python3
* modules: boto, swiftclient, md5hash, requests

# run simple s3 test


## export environment variables

```console
    export RGW_ACCESS_ID=access id
    export RGW_SECRET_KEY=secret key
    export RGW_HOST=address of RGW host (e.g. s3.test.org )
    export RGW_PORT=RGW host port (e.g. 443)
```

## execute a simple test

* connect via s3 protocol (--s3)

* container name: test (-ctest)

* object name prefix: simpletest (-osimpletest)

* containter/object numbers as a value OR range: (-n100)

* tests to run: p (put), g (get), d (delete)

```console
    python3 getput --s3  -ctest -osimpletest -n100 -s1k -tp,g,d
```

output:
```console
Rank Test  Clts Proc  OSize  Start     End        MB/Sec   Ops   Ops/Sec Errs Latency  Median    LatRange   %CPU Ret
0    put      1    1   1.0k  11:03:28  11:03:37     0.01   100     10.81    0   0.092   0.049  0.01-00.36   0.76   0
0    get      1    1   1.0k  11:03:38  11:03:40     0.06   100     56.71    0   0.018   0.003  0.00-00.20   3.64   0
0    del      1    1   1.0k  11:03:40  11:03:45     0.02   100     20.43    0   0.049   0.007  0.01-00.33   0.51   0
```

# run extended s3 test

* test multiple object sizes: 4k, 1M,4M,16M,64M,128M
* run test mit different parallel processes: 1,2,4,8,16
  (note: the test client need enough CPU, RAM and NETWORK ressources to run multiple processes parallel)
* report latency distributions at this granularity: --ldist=1 

```console
    python3 getput --s3 -cextended -oext -n1000 -s4k,1M,4M,16M,64M,128M --procs=1,2,4,8,16 -tp,g,d --ldist=1 --utc
```
# run distributed s3 test

## suite settings file

gpsuite-simple.conf

```
[simplte-test-s3]
comment  = simple test configuration
options  = --s3 
type     = simple
cname    = cont-name
oname    = obj-name
sizes    = 1k,10k,100k,1m,10m
procs    = 1,2,4,8
creds    = gpsuite-s3-creds
nodes    = gpsuite-s3-nodes
username = root
maxnodes = 1
runtime  = 120
synctime = 5
utc      = 1
```

## list suites

```console
    python3 gpsuite --list --config gpsuite-simple.conf
    simplte-test-s3       simple test configuration
```

## execute test

```console
    gpsuite --suite simplte-test-s3 --config gpsuite-simple.conf 
```
