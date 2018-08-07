# Mirth Connect on Docker

Build a Docker image containing a Mirth Connect Server running with a Derby DB

[Download Mirth Connect 3.5.1](http://downloads.mirthcorp.com/connect/3.5.1.b194/mirthconnect-3.5.1.b194-unix.tar.gz) ```.tar.gz``` file and drop it on the same location as the Dockerfile. Build it:
```
$ docker build -t alexsantos/mirth-docker .
```
