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

## test node configuration

### create a ssh key
```console
ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa): y
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in y
Your public key has been saved in y.pub
The key fingerprint is:
SHA256:Nc658XkxMgRGDjS7W0UBhmXsjGQKalCbekupExwtpYU root@demo
The key's randomart image is:
+---[RSA 3072]----+
| .oo   .+=O.o.   |
|.E=o.   +O.o     |
| =oo . +.++ o    |
|..=.  . .=o=     |
|oo+     S * o o  |
| = .     o + + o |
|o .     . . o .  |
| .           .   |
|                 |
+----[SHA256]-----+
```

### add the public key to all test-nodes 

In our example is the user root, not recommended for production systems. Only used in the test environment.

```console
    cat ~/.ssh/id_rsa.pub | ssh USER@HOST "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
```

### login in via ssh and key to all the nodes 

accept the connection at the first try 

### clone the git repository 

git clone https://github.com/ubi2go/getput.git
cd getput
sudo python3 setup.py install

### time synchronization of all nodes

install ntp,ntpd or chrony and sync the nodes to a single time source.

## create node file

gpsuite-s3-nodes
```
#<list of hosts: each host a new line>
test-node1
test-node2
...
```

## create creds file

gpsuite-s3-creds

```
RGW_ACCESS_ID=<access_id>
RGW_SECRET_KEY=<secret_key>
RGW_HOST=<endpoint domainname>
RGW_PORT=<port e.g. 443>
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
