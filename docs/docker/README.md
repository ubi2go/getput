This is the getput benchmarking tool suite.

# Dependencies:

python3-swiftclient
python3-boto

# Build docker image

```
    docker build --tag python3-getput --network=host .
```

# Installation:

git clone https://github.com/markseger/getput.git
<BR>cd getput
<BR>sudo python setup.py install

# Start getput as docker container


## create os.env
RGW_ACCESS_ID=<access id>
RGW_SECRET_KEY=<secret key>
RGW_HOST=<endpoint domainname>
RGW_PORT=port (e.g. 443)


```
   docker run --env-file os.env --network=host python3-getput -n1 -tp,d -c1 -on -s4k --s3
```
# Documentation:

/usr/share/doc/gptools/getting-started.txt
<BR>/usr/share/doc/gptools/Introduction.pdf

# Notes:

Version 2 of the swift client fixes the problem with longer PUT latencies
for small objects BUT you need to make sure you have the latest version
