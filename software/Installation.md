
| No | Software       | Main Function                                      |
| -- | -------------- | -------------------------------------------------- |
| 1  | srsRAN Project | RAN + Core network                                 |
| 2  | SoapySDR       | Abstraction API for various SDRs                   |
| 3  | BladeRF        | SDR hardware for transmitting/receiving RF signals |
| 4  | SoapyBladeRF   | SoapySDR plugin for BladeRF                        |
| 5  | UHD            | Driver/API for Ettus USRP SDRs                     |
| 6  | SoapyUHD       | SoapySDR plugin for USRP                           |
| 7  | oran-sc-ric    | Near-RT RIC platform for running xApps / RIC       |




## 1.srsRAN_Project

**dependencies Commands**

        sudo apt-get install cmake make gcc g++ pkg-config libfftw3-dev libmbedtls-dev libsctp-dev libyaml-cpp-dev libgtest-dev

**Main Commands**

    git clone https://github.com/srsRAN/srsRAN_Project.git
    cd srsRAN_Project
    mkdir build
    cd build
    cmake ../
    make -j $(nproc)
    make test -j $(nproc)
    sudo make install


## 2.SoapySDR

**dependencies Commands**

    sudo apt-get install cmake g++ libpython3-dev python3-numpy swig

**Main Commands**

    git clone https://github.com/pothosware/SoapySDR.git
    cd SoapySDR
    mkdir build
    cd build
    cmake ..
    make -j`nproc`
    sudo make install -j`nproc`
    sudo ldconfig


## 3.BladeRF

**dependencies Commands**

    sudo apt-get install -y libcurl4-openssl-dev \
    build-essential cmake g++ libpython3-dev python3-numpy swig \
    libusb-1.0-0-dev libtecla1 libtecla-dev libedit-dev \
    libcurl4-openssl-dev pkg-config git

**Main Commands**

    git clone https://github.com/Nuand/bladeRF.git
    cd bladeRF
    mkdir -p build
    cd build
    cmake ..
    make -j$(nproc)
    sudo make install
    sudo ldconfig
 
**Build host utilities (bladerf-cli, bladerf-fpga-hostedx40.rbf, etc.)**

    cd ../host
    mkdir -p build
    cd build
    cmake -DCMAKE_BUILD_TYPE=Release \
          -DCMAKE_INSTALL_PREFIX=/usr/local \
          -DINSTALL_UDEV_RULES=ON ..
    make -j$(nproc)
    sudo make install
    sudo ldconfig
    

## 4.SoapyBladeRF

**Terminal Commands**

    git clone https://github.com/pothosware/SoapyBladeRF.git
    cd SoapyBladeRF
    mkdir build
    cd build
    cmake ..
    make
    sudo make install


## 5.UHD

**dependencies Commands**

**Ububtu 24.04**

       sudo apt-get -y install autoconf automake build-essential ccache cmake cpufrequtils doxygen ethtool fort77 g++ gir1.2-gtk-3.0 git gobject-introspection gpsd gpsd-clients inetutils-tools libasound2-dev libboost-all-dev libcomedi-dev libcppunit-dev libfftw3-bin libfftw3-dev libfftw3-doc libfontconfig1-dev libgmp-dev libgps-dev libgsl-dev liblog4cpp5-dev libncurses5 libncurses5-dev libpulse-dev libqt5opengl5-dev libqwt-qt5-dev libsdl1.2-dev libtool libudev-dev libusb-1.0-0 libusb-1.0-0-dev libusb-dev libxi-dev libxrender-dev libzmq3-dev libzmq5 ncurses-bin python3-cheetah python3-click python3-click-plugins python3-click-threading python3-dev python3-docutils python3-gi python3-gi-cairo python3-gps python3-lxml python3-mako python3-numpy python3-opengl python3-pyqt5 python3-requests python3-scipy python3-setuptools python3-six python3-sphinx python3-yaml python3-zmq python3-ruamel.yaml swig wget python3-pygccxml libjs-mathjax python3-pyqtgraph

**Ububtu 22.04**

       sudo apt-get -y install autoconf automake build-essential ccache cmake cpufrequtils doxygen ethtool fort77 g++ gir1.2-gtk-3.0 git gobject-introspection gpsd gpsd-clients inetutils-tools libasound2-dev libboost-all-dev libcomedi-dev libcppunit-dev libfftw3-bin libfftw3-dev libfftw3-doc libfontconfig1-dev libgmp-dev libgps-dev libgsl-dev liblog4cpp5-dev libncurses5 libncurses5-dev libpulse-dev libqt5opengl5-dev libqwt-qt5-dev libsdl1.2-dev libtool libudev-dev libusb-1.0-0 libusb-1.0-0-dev libusb-dev libxi-dev libxrender-dev libzmq3-dev libzmq5 ncurses-bin python3-cheetah python3-click python3-click-plugins python3-click-threading python3-dev python3-docutils python3-gi python3-gi-cairo python3-gps python3-lxml python3-mako python3-numpy python3-opengl python3-pyqt5 python3-requests python3-scipy python3-setuptools python3-six python3-sphinx python3-yaml python3-zmq python3-ruamel.yaml swig wget

**Ububtu 20.04**

       sudo apt-get -y install autoconf automake build-essential ccache cmake cpufrequtils doxygen ethtool fort77 g++ gir1.2-gtk-3.0 git gobject-introspection gpsd gpsd-clients inetutils-tools libasound2-dev libboost-all-dev libcomedi-dev libcppunit-dev libfftw3-bin libfftw3-dev libfftw3-doc libfontconfig1-dev libgmp-dev libgps-dev libgsl-dev liblog4cpp5-dev libncurses5 libncurses5-dev libpulse-dev libqt5opengl5-dev libqwt-qt5-dev libsdl1.2-dev libtool libudev-dev libusb-1.0-0 libusb-1.0-0-dev libusb-dev libxi-dev libxrender-dev libzmq3-dev libzmq5 ncurses-bin python3-cheetah python3-click python3-click-plugins python3-click-threading python3-dev python3-docutils python3-gi python3-gi-cairo python3-gps python3-lxml python3-mako python3-numpy python3-numpy-dbg python3-opengl python3-pyqt5 python3-requests python3-scipy python3-setuptools python3-six python3-sphinx python3-yaml python3-zmq python3-ruamel.yaml swig wget

**Main Commands on Ubuntu 22.04**

    git clone https://github.com/EttusResearch/uhd.git -b UHD-4.5
    cd uhd/host
    mkdir build
    cd build
    cmake ..
    make
    sudo make install
    sudo ldconfig

**Main Commands on Ubuntu 24.04**

    git clone https://github.com/EttusResearch/uhd.git
    cd uhd/host
    mkdir build
    cd build
    cmake ..
    make
    sudo make install
    sudo ldconfig


## 6.SoapyUHD

**Main Commands**

    git clone https://github.com/pothosware/SoapyUHD.git
    cd SoapyUHD
    mkdir build
    cd build
    cmake ..
    make
    sudo make install

## 7.oran-sc-ric

**Main Commands**

    git clone https://github.com/srsran/oran-sc-ric


























    
