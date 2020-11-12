obs-bmusb
=========
`obs-bmusb` is a Linux plugin for [OBS studio][obs] that provides a Source for capturing from the BlackMagic USB3 cards
Intensity Shuttle and UltraStudio SDI via the [bmusb][bmusb] driver.


[obs]: https://obsproject.com/
[bmusb]: https://git.sesse.net/?p=bmusb;a=summary

I forked this to work on this README for now, maybe as I get more familiar with c++ I could help out in other ways.

---
Build Instructions:
---

First make sure you have the prereq libraries

`libusb-dev`
`bmusb`

I struggled to build bmusb forever and eventually realised there were binaries in my distros package manager. Ubuntu bionic/focal/groovy have this package in the "universe (sources)" repository.

Then you will need to build obs-studio from source with this git cloned into obs-studio/plugins.
I would recommend making this as a portable install as you might already have it installed through a package manager on your system, and this keeps things separate. follow instructions here: 

https://obsproject.com/wiki/install-instructions#linux-portable-mode-all-distros

but add the step to clone this plugin into the plugins folder like so:
```
wget https://cdn-fastly.obsproject.com/downloads/cef_binary_3770_linux64.tar.bz2
tar -xjf ./cef_binary_3770_linux64.tar.bz2
git clone --recursive https://github.com/obsproject/obs-studio.git
cd obs-studio/plugins
git clone https://github.com/s-ol/obs-bmusb.git
```
Edit CMakeLists.txt & add this line at the bottom:

`add_subfolder(obs-bmusb)` save and exit, then

```
mkdir ../build && cd ../build
cmake -DUNIX_STRUCTURE=0 -DCMAKE_INSTALL_PREFIX="${HOME}/obs-studio-portable" -DBUILD_BROWSER=ON -DCEF_ROOT_DIR="../../cef_binary_3770_linux64" ..
make -j4 && make install
```
If it all goes well you now have a copy of obs-studio with the obs-bmusb plugin!


If you encounter an error like so:
`Package libusb was not found` you might need to use pkg-config to add the path to the environment:

`pkg-config --cflags --libs /usr/local/Cellar/libusb/1.0.20/lib/pkgconfig/libusb-1.0.pc`
