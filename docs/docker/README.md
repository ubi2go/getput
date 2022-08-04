# Build docker image

```console
    docker build --tag python3-getput --network=host .
```

# Start getput as docker container

## create os.env file
```
RGW_ACCESS_ID=<access id>
RGW_SECRET_KEY=<secret key>
RGW_HOST=<endpoint domainname>
RGW_PORT=port (e.g. 443)
```

## run docker container
```console
   docker run --env-file os.env --network=host python3-getput -n1 -tp,g,d -cdocker -oexample -s4k --s3
```
# Documentation:

/usr/share/doc/gptools/getting-started.txt
<BR>/usr/share/doc/gptools/Introduction.pdf

# Notes:

Version 2 of the swift client fixes the problem with longer PUT latencies
for small objects BUT you need to make sure you have the latest version
