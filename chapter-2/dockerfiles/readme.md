# kill container in m1 mac when Ctrl + C fails
```shell
docker kill <container>
OR 
# if you have only one container
docker kill $(docker ps --quiet --latest)
```


Note: 
Order the dockerfile layers from least likely to change to most likely to change in order to optimize
the image size for pushing and pulling