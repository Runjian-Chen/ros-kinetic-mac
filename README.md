# ros-kinetic-mac
Build ros kinetic on Mac(mojave)

- Install homebrew in command line
```console
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)
```

- Even though we use brew to install many dependences which ros needs, we have to uninstall all python installed by brew
```console
brew uninstall python
brew uninstall python@2
```

- Check if there is only python which the systems relies on
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

