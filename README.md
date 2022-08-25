# Installation-ROS2-Foxy-Fitzroy-on-Jetson-Nano
Installing ROS2 Foxy Fitzroy on Jetson Nano


## Installation Ubuntu on Jetson Nano
First download [xubuntu](https://forums.developer.nvidia.com/t/xubuntu-20-04-focal-fossa-l4t-r32-3-1-custom-image-for-the-jetson-nano/121768) and [balena](https://www.balena.io/etcher/), after that, should be burn Ubuntu OS into flash memory or card using balena software. Then, need to Install the Ubuntu system on Jetson Nano

## Installation ROS2 Foxy Fitzroy on Jetson Nano

In Ubuntu oprating system open the command line (Terminal) and start to Install the follwing 

### Set locale
Make sure you have a locale which supports `UTF-8`. If you are in a minimal environment (such as a docker container), the locale may be something minimal like `POSIX`. We test with the following settings. However, it should be fine if you’re using a different UTF-8 supported locale.
```
locale
```
```
sudo apt update && sudo apt install locales
```
```
sudo locale-gen en_US en_US.UTF-8
```
```
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
```
```
export LANG=en_US.UTF-8
```
```
locale
```


### Setup Sources
You will need to add the ROS 2 apt repository to your system. First, make sure that the Ubuntu Universe repository is enabled by checking the output of this command
```
apt-cache policy | grep universe
```

This should output a line like the one below:
```
500 http://us.archive.ubuntu.com/ubuntu focal/universe amd64 Packages
    release v=20.04,o=Ubuntu,a=focal,n=focal,l=Ubuntu,c=universe,b=amd64
```

If you don’t see an output line like the one above, then enable the Universe repository with these instructions.
```
sudo apt install software-properties-common
```
```
sudo add-apt-repository universe
```

Now add the ROS 2 apt repository to your system. First authorize our GPG key with apt
```
sudo apt update && sudo apt install curl gnupg2 lsb-release
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key  -o /usr/share/keyrings/ros-archive-keyring.gpg
```

Then add the repository to your sources list
```
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(source /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
```

### Install ROS 2 packages
Update your apt repository caches after setting up the repositories
```
sudo apt update
```

ROS 2 packages are built on frequently updated Ubuntu systems. It is always recommended that you ensure your system is up to date before installing new packages
```
sudo apt upgrade
```

Desktop Install (Recommended): ROS, RViz, demos, tutorials
```
sudo apt install ros-foxy-desktop
```

ROS-Base Install (Bare Bones): Communication libraries, message packages, command line tools. No GUI tools
```
sudo apt install ros-foxy-ros-base
```


### Environment setup
#### Sourcing the setup script
Set up your environment by sourcing the following file
```
source /opt/ros/foxy/setup.bash
```

#### Try some examples
##### Talker-listener
If you installed `ros-foxy-desktop` above you can try some examples

In one terminal, source the setup file and then run a C++ `talker`:
```
source /opt/ros/foxy/setup.bash
ros2 run demo_nodes_cpp talker
```
![1](https://user-images.githubusercontent.com/90250848/186642018-3bcb1680-36c1-44d3-a03d-f2539fe87448.PNG)


In another terminal source the setup file and then run a Python `listener`:
```
source /opt/ros/foxy/setup.bash
ros2 run demo_nodes_py listener
```
![2](https://user-images.githubusercontent.com/90250848/186641972-06d617fc-426a-4bc9-b0c0-2e1d7f8956c0.PNG)


You should see the `talker` saying that it’s `Publishing` messages and the `listener` saying `I heard` those messages. This verifies both the C++ and Python APIs are working properly

