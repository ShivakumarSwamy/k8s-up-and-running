# kill container in m1 mac when Ctrl + C fails
```shell
docker kill <container>
OR 
# if you have only one container
docker kill $(docker ps --quiet --latest)
```

# build image
```shell
docker build --tag <image_name> .
```

# run image and remove on exit
```shell
docker run --rm -p 3000:3000 image_name
```

Note: 
Order the dockerfile layers from least likely to change to most likely to change in order to optimize
the image size for pushing and pulling

# running kuard
```shell
docker run --name kuard -p 8080:8080 --memory 200m --memory-swap 1G --cpu-shares 1024 gcr.io/kuar-demo/kuard-amd64:blue
OR 
docker run --name kuard -p 8080:8080 gcr.io/kuar-demo/kuard-amd64:blue
```