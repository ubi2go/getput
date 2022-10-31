# prerequisites

* python3
* modules: boto, swiftclient, md5hash, requests

# run simple s3 test


## export environment variables

```console
    export RGW_ACCESS_ID=access id
    export RGW_SECRET_KEY=secret key
    export RGW_HOST=address of RGW host (e.g. s3.test.org )
    export RGW_PORT=RGW host port (e.g. 80)
```

## execute a simple test

* connect via s3 protocol (--s3)

* connect via http protocol (--insecure)

* container name: test (-ctest)

* object name prefix: simpletest (-osimpletest)

* containter/object numbers as a value OR range: (-n100)

* tests to run: p (put), g (get), d (delete)

```console
    python3 getput --s3  -ctest -osimpletest -n100 -s1k -tp,g,d --insecure
```

output:
```console
Rank Test  Clts Proc  OSize  Start     End        MB/Sec   Ops   Ops/Sec Errs Latency  Median    LatRange   %CPU Ret
0    put      1    1   1.0k  11:03:28  11:03:37     0.01   100     10.81    0   0.092   0.049  0.01-00.36   0.76   0
0    get      1    1   1.0k  11:03:38  11:03:40     0.06   100     56.71    0   0.018   0.003  0.00-00.20   3.64   0
0    del      1    1   1.0k  11:03:40  11:03:45     0.02   100     20.43    0   0.049   0.007  0.01-00.33   0.51   0
```
