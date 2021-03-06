# Docker image for hls4ml with Vivado

Put the Vivado installation archive here and provide the path to license server.
For example:

```
docker build --network=host -t hls4ml-with-vivado --build-arg LICENSE_SERVER="1234@myserver" .
docker run -it hls4ml-with-vivado bash
```

By default, version 2018.2 of Vivado is used.

## Using other versions of Vivado

To install specific version of Vivado, first generate the silent installation configuration file from the Vivado installation folder by running:

```
./xsetup -b configGen
```

Choose the products/devices the you would like to install by editing the generated file. Name the file `install_config.txt` and place it in the directory with `Dockerfile`. Edit the `Dockerfile` to add the folder of your Vivado installation and build the image using the command provided above. 

## GUI support

By default, the image is built without X11 libraries needed to launch Vivado HLS GUI. To add GUI support, pass `--build-arg GUI_SUPPORT=1` to the build command. For example:

```
docker build --network=host -t hls4ml-with-vivado --build-arg LICENSE_SERVER="1234@myserver" --build-arg GUI_SUPPORT=1 .
```

To launch GUI apps in Docker container, map `/tmp/.X11-unix` and `DISPLAY` environment variable from host to the container, e.g.,

```
docker run -it -e DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix hls4ml-with-vivado
```

If your X11 session requires a valid user, `Xauthority` file must be mapped into the container. This file is either in the user's home directory (`$HOME/.Xauthority`) or its location is spcified in the `XAUTHORITY` environment variable. For example:

```
docker run -it -e DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix -v $HOME/.Xauthority:/home/hls4ml/.Xauthority hls4ml-with-vivado
```

## Customizing the default user

Default user (named *hls4ml*) cah have its *id* and *group* changed to match a specific user on host machine with `USER_ID` and `GROUP_ID` build arguments. Useful if you want to add a shared volume. For example:

```
docker build --network=host -t hls4ml-with-vivado --build-arg LICENSE_SERVER="1234@myserver" --build-arg USER_ID=`id -u` --build-arg GROUP_ID=`id -g` .
```