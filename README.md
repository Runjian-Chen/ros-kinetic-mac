# ros-kinetic-mac
Build ros kinetic on Mac(mojave 10.14.2)

## Step for installation

- Install homebrew in command line
```console
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)
```

- Even though we use brew to install many dependences which ros needs, we have to uninstall all python installed by brew
```console
brew uninstall python
brew uninstall python@2
```

- Check
```console
brew info python
```

- To install the bag pcl_ros, we need to copy file 'Python.framework' in the following path
```console
/System/Library/Frameworks/Python.framework
```
- And paste in the following path (mkdir python@2 and Frameworks)
```console
/usr/local/opt/python@2/Frameworks/
```
if
- Add repositories
```console
brew tap ros/deps
brew tap osrf/simulation
```

- Set up environment valuable
```console
export PATH=/usr/local/bin:$PATH
source ~/.bashrc
```

- Set up environment valuable for python
```console
mkdir -p ~/Library/Python/2.7/lib/python/site-packages
echo "$(brew --prefix)/lib/python2.7/site-packages" >> ~/Library/Python/2.7/lib/python/site-packages/homebrew.pth
```

- Follow the instructions on the website below to install pip independently (not with brew!)
```console
https://pip.readthedocs.io/en/stable/installing/
```

- Install python tools
```console
sudo -H pip install -U wstool rosdep rosinstall rosinstall_generator rospkg catkin-pkg Distribute sphinx
```

- Init
```console
sudo rosdep init
rosdep update
```

- Build up workspace
```console
mkdir Path_that_you_use/ros_catkin_ws
cd Path_that_you_use/ros_catkin_ws
```

- Download installer(replace destop_full with desktop or ros_comm to download different installer if you need)
```console
rosinstall_generator desktop_full --rosdistro kinetic --deps --wet-only --tar > kinetic-desktop-full-wet.rosinstall
```

- Install
```console
wstool init -j8 src kinetic-desktop-full-wet.rosinstall
```

- Analyse dependence (add --skip-keys="~" to skip everything that fails to install and install those with brew, linking them as well)
```console
rosdep install --from-paths src --ignore-src --rosdistro kinetic -y --skip-keys="google-mock" --skip-keys="python-wxtools" --skip-keys="qt" --skip-keys="gazebo" --skip-keys libogre-dev --skip-keys "libqt5-core libqt5-gui libqt5-opengl libqt5-opengl-dev libqt5-widgets qt5-qmake qtbase5-dev python-qt5-bindings-webkit"
```

- Build
```console
./src/catkin/bin/catkin_make_isolated --install -DCMAKE_BUILD_TYPE=Release -DCMAKE_FIND_FRAMEWORK=LAST -DCATKIN_ENABLE_TESTING=0
```

- Set up environment valuable for roscore
```console
export ROS_HOSTNAME=localhost
export ROS_MASTER_URI=http://localhost:11311
```

- Add this to ~/.bash_profile and source it or reopen the console
```console
source Path_that_you_use/ros_catkin_ws/devel_isolated/setup.bash
```

- To git rid of the influence brought by SIP, we need to copy file all the files in the following path
```console
Path_that_you_use/ros_catkin_ws/install_isolated/lib
```
- And paste in the following path
```console
/usr/local/lib
```

>
>
>

## Error and solutions
- Linking error:
```console
ld: library not found for ~~~
```
```console
Solution : 
1) install the dependence if you haven't installed it
2) if you install it via brew : brew link ~~~
3) if you don't install it via brew : export LIBRARY_PATH=$LIBRARY_PATH: (place the path you install it here)
4) if it is a dynamic lib : replace "export LIBRARY_PATH=$LIBRARY_PATH:" to the command the link dynamic lib
```

- OpenCV error in cv_bridge and image_geometry package:
```console
Undefined symbols for architecture x86_64:
  "cv::~~~
```
```console
Solution : 
Change this in the CMakeLists.txt in src/~~~/cv_bridge or  src/~~~/image_geometry :
From "find_package(OpenCV REQUIRED" to "find_package(OpenCV 3.4.3 REQUIRED"
replace 3.4.3 to the version of opencv you install
```

- Could not find the following Boost libraries: boost_python3
```console
Solution : 
1) brew install boost-python3 and link it
2) if this does not solve the problem, link it to /usr/local/lib/ : cd /usr/local/lib/ && sudo ln -s libboost_python36.dylib libboost_python3.dylib
```

- Fail to build camera_calibration_parsers
```console
error: a space is required between consecutive right angle brackets
```

```console
Solution : 
Add this to the CMakeLists.txt camera_calibration_parsers in : set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
```

- Error in building rospack
```console
~/ros_catkin_ws/src/rospack/include/rospack/rospack.h:108:10:
fatal error:’boost/tr1/unordered_set.hpp’ file not found
```

```console
Solution : change ’boost/tr1/unordered_set.hpp’ in every head or cpp files to ’boost/unordered_set.hpp’ (yeah just delete 'tr1/'
```

- logError or logWarn not in the scope
```console
Solution : Replace every "logError" or "logWarn cmake found with "CONSOLE_BRIDGE_logError" or "CONSOLE_BRIDGE_logWarn"
```


