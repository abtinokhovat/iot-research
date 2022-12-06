# ns3_docker
Docker container for ns3

# Pre-requisites
- `make`
- `docker`

# Specifics
This project provides both `Dockerfile` and `Makefile` for allowing [ns3 3.7](https://www.nsnam.org/releases/ns-3-37/) to be deployed using a docker container built using _Ubuntu 20.04 LTS_ as base image.

Docker image is created and run using the user `ns3-user`.

The path `/usr/ns3` can be used as launching pad for any user-developed (i.e. _yours_) code to run. It is a symbolic link to actual ns3 versioned source code. This mechanism is chosen so as to allow forward compatibility in case of changes to base ns3 version you may wish to do do.

Contributions to the project are welcome.

_Networking funtionailty outside of docker container is not yet verified._

# Docker file structure 
### **Ubuntu/Debian/Mint**

1. **minimal requirements for release 3.36 and later:**
    
    ```bash
     apt install g++ python3 cmake ninja-build git python3-pip mercurial
    ```
    
2. Ccache is a compiler cache optimization that will speed up builds across multiple ns-3 directories, at the cost of up to an extra 5 GB of disk space used in the cache.
    
    ```bash
     apt install ccache
    ```
    
3. **minimal requirements for Python visualizer and bindings (ns-3.37 and newer):** cppyy Python module and Pyviz dependencies
    
    ```bash
    python3 -m pip install --user cppyy
    apt install gir1.2-goocanvas-2.0 python3-gi python3-gi-cairo python3-pygraphviz gir1.2-gtk-3.0 ipython3 python3-setuptools
    ```
    
4. **Netanim animator:** qt5 development tools are needed for Netanim animator; qt4 will also work but we have migrated to qt5. qt6 compatibility is not tested.
    
    ```bash
     apt install qtbase5-dev qtchooser qt5-qmake qtbase5-dev-tools
    ```
    
5. For ns-3.28 and earlier releases, PyViz is based on GTK+ 2, GooCanvas, and GraphViz:
    
    ```bash
    apt install python-pygraphviz python-kiwi python-pygoocanvas libgoocanvas-dev ipython
    ```
    
6. Support for MPI-based distributed emulation
    
    ```bash
    apt install openmpi-bin openmpi-common openmpi-doc libopenmpi-dev
    ```
    
7. Debugging:
    
    ```bash
     apt install gdb valgrind
    ```
    
8. Support for utils/check-style-clang-format.py code style check program (since ns-3.37):
    
    ```bash
    apt install clang-format
    ```
    
9. Doxygen and related inline documentation:
    
    ```bash
     apt install doxygen graphviz imagemagick
     apt install texlive texlive-extra-utils texlive-latex-extra texlive-font-utils dvipng latexmk
    ```
    
10. The ns-3 manual and tutorial are written in reStructuredText for Sphinx (doc/tutorial, doc/manual, doc/models), and figures typically in dia (also needs the texlive packages above):
    
    ```bash
     apt install python3-sphinx dia
    ```
    
11. GNU Scientific Library (GSL) support for more accurate 802.11b WiFi error models (not needed for OFDM):
    
    ```bash
     apt install gsl-bin libgsl-dev libgslcblas0
    ```
    
12. To read pcap packet traces
    
    ```bash
    apt install tcpdump
    ```
    
13. Database support for statistics framework
    
    ```bash
    apt install sqlite sqlite3 libsqlite3-dev
    ```
    
14. Xml-based version of the config store (requires libxml2 >= version 2.7)
    
    ```bash
    apt install libxml2 libxml2-dev
    ```
    
15. Support for generating modified python bindings (ns-3.36 and earlier):
    
    ```bash
     apt install cmake libc6-dev libc6-dev-i386 libclang-dev llvm-dev automake python3-pip
     python3 -m pip install --user cxxfilt
    ```
    
16. A GTK-based configuration system
    
    ```bash
     apt install libgtk-3-dev
    ```
    
17. To experiment with virtual machines and ns-3
    
    ```
     apt install vtun lxc uml-utilities
    ```
    
18. Support for openflow module (requires libxml2-dev if not installed above) and Boost development libraries
    
    ```
    apt install libxml2 libxml2-dev libboost-all-dev
    ```
    
19. For adding brite integration we have to build brite from a mercurial repository
    
    ```bash
    hg clone http://code.nsnam.org/BRITE 
    cd BRITE
    make
    ```
    
    [BRITE Integration - Model Library](https://www.nsnam.org/docs/models/html/brite.html)
    
20. For adding openflow integration we have to build openflow from a mercurial repository and from source code.
    
    ```bash
    hg clone http://code.nsnam.org/openflow
    cd openflow
    ./waf configure
    ./waf build
    ```
    
    [OpenFlow switch support - Model Library](https://www.nsnam.org/docs/models/html/openflow-switch.html)
    
21. Dependencies of DPDK
    
    ```bash
    apt-get -y install dpdk dpdk-dev libdpdk-dev dpdk-igb-uio-dkms
    ```
    
    [DPDK NetDevice - Model Library](https://www.nsnam.org/docs/models/html/dpdk-net-device.html)
    
22. For building click module we have to build it from it’s repository
    
    ```bash
    git clone https://github.com/kohler/click
    cd click/
    ./configure --disable-linuxmodule --enable-nsclick --enable-wifi
    make
    ```
    
    [Click Modular Router Integration - Model Library](https://www.nsnam.org/docs/models/html/click.html)