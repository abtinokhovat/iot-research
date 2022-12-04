FROM amd64/ubuntu:20.04
LABEL "cloud.randd.team" = "ns3_3.36.1 image" \
    version = "1.0" \
    description = "Base docker image of ns3 3.36.1 with Ubuntu 20.04 LTS" 

# create ns3-user user
RUN groupadd -r ns3-user && \
	useradd -m -d /home/ns3-user -g ns3-user ns3-user
RUN echo "ns3-user:ns3-user" | chpasswd

# update apt package manager
RUN apt update

# 1.minimal requirements
RUN DEBIAN_FRONTEND=noninteractive apt install -y g++ python3 cmake ninja-build git python3-pip 

# 2.Ccache is a compiler cache optimization that will speed up builds across multiple ns-3 directories, at the cost of up to an extra 5 GB of disk space used in the cache.
RUN DEBIAN_FRONTEND=noninteractive apt install -y ccache

# 3.minimal requirements for Python visualizer and bindings (ns-3.37 and newer): cppyy Python module and Pyviz dependencies
USER ns3-user
RUN python3 -m pip install --user cppyy
USER root
RUN DEBIAN_FRONTEND=noninteractive apt install -y gir1.2-goocanvas-2.0 python3-gi python3-gi-cairo python3-pygraphviz gir1.2-gtk-3.0 ipython3 python3-setuptools

# 4.Netanim animator: qt5 development tools are needed for Netanim animator; qt4 will also work but we have migrated to qt5. qt6 compatibility is not tested.
RUN DEBIAN_FRONTEND=noninteractive apt install -y qtbase5-dev qtchooser qt5-qmake qtbase5-dev-tools

# 5.Support for MPI-based distributed emulation
RUN DEBIAN_FRONTEND=noninteractive apt install -y openmpi-bin openmpi-common openmpi-doc libopenmpi-dev

# 6.Debugging:
RUN DEBIAN_FRONTEND=noninteractive apt install -y gdb valgrind

# 7.Support for utils/check-style-clang-format.py code style check program (since ns-3.37):
RUN DEBIAN_FRONTEND=noninteractive apt install -y clang-format

# 8.Doxygen and related inline documentation:
RUN DEBIAN_FRONTEND=noninteractive apt install -y doxygen graphviz imagemagick
#RUN DEBIAN_FRONTEND=noninteractive apt install -y texlive texlive-extra-utils texlive-latex-extra texlive-font-utils dvipng latexmk

# 9.The ns-3 manual and tutorial are written in reStructuredText for Sphinx (doc/tutorial, 
# doc/manual, doc/models), and figures typically in dia (also needs the texlive packages above):
RUN DEBIAN_FRONTEND=noninteractive apt install -y python3-sphinx dia

# 10. GNU Scientific Library (GSL) support for more accurate 802.11b WiFi error models (not needed for OFDM):
RUN DEBIAN_FRONTEND=noninteractive apt install -y gsl-bin libgsl-dev libgslcblas0

# 11. To read pcap packet traces
RUN DEBIAN_FRONTEND=noninteractive apt install -y tcpdump

# 12. Database support for statistics framework
RUN DEBIAN_FRONTEND=noninteractive apt install -y sqlite sqlite3 libsqlite3-dev

# 13. Xml-based version of the config store (requires libxml2 >= version 2.7)
RUN DEBIAN_FRONTEND=noninteractive apt install -y libxml2 libxml2-dev

# 14. Support for generating modified python bindings (ns-3.36 and earlier):
RUN DEBIAN_FRONTEND=noninteractive apt install -y libc6-dev libc6-dev-i386 libclang-dev llvm-dev automake

# 15. A GTK-based configuration system
RUN DEBIAN_FRONTEND=noninteractive apt install -y libgtk-3-dev

# 16. To experiment with virtual machines and ns-3
RUN DEBIAN_FRONTEND=noninteractive apt install -y vtun lxc uml-utilities

# 17. Support for openflow module (requires libxml2-dev if not installed above) and Boost development libraries
RUN DEBIAN_FRONTEND=noninteractive apt install -y libxml2 libxml2-dev libboost-all-dev
  

# install wget
RUN DEBIAN_FRONTEND=noninteractive apt install -y \ 
    wget           

#RUN mkdir -p /usr/ns3
WORKDIR /usr

# download and extract ns3
RUN wget https://www.nsnam.org/releases/ns-allinone-3.37.tar.bz2 && \
	tar -jxvf ns-allinone-3.37.tar.bz2

# full access to ns3 directory
RUN chown -fR ns3-user:ns3-user ns-allinone-3.37/

# 18. install dpdk dependecies
RUN DEBIAN_FRONTEND=noninteractive apt-get -y install dpdk dpdk-dev libdpdk-dev dpdk-igb-uio-dkms

# 19. install mercurial
RUN DEBIAN_FRONTEND=noninteractive apt install -y mercurial

# 20. clone BRITE and build
RUN hg clone http://code.nsnam.org/BRITE && cd BRITE && make && cd ../

# 21. clone openflow and build
RUN hg clone http://code.nsnam.org/openflow && cd openflow && ./waf configure && ./waf build && cd ../

# 22. clone click and build
RUN git clone https://github.com/kohler/click && cd click/ && ./configure --disable-linuxmodule --enable-nsclick --enable-wifi && make && cd ../


# RUN cd ns-allinone-3.37/ns-3.37 


# RUN ./ns3 configure --build-profile=debug --disable-werror --enable-examples --enable-tests 

# RUN ln -s /usr/ns-allinone-3.37/ns-3.37 /usr/ns3

# RUN apt clean && \
# 	rm -rf /var/lib/apt/lists/* && \
# 	rm ns-allinone-3.37.tar.bz2
