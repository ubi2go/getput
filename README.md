# GETPUT
This is the getput benchmarking tool suite for SWIFT and S3 object storage.

## Dependencies

* python3
* python-requests
* python-swiftclient
* python-boto

## Installation

### Linux
```console
git clone https://github.com/ubi2go/getput.git
cd getput
sudo python3 setup.py install
```

### Docker

https://github.com/ubi2go/getput/tree/master/docs/docker

## Documentation

/usr/share/doc/gptools/getting-started.txt
<BR>/usr/share/doc/gptools/Introduction.pdf

## Notes

Version 2 of the swift client fixes the problem with longer PUT latencies
for small objects BUT you need to make sure you have the latest version
of python-requests which is not currently in the apt-get repository as of
this writing.  Running getput with that version results in slower 1K PUTs
than 2K PUTs.  I've found installing requests-2.2.1 from the tarball fixes
the problem.
