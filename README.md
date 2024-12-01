# OAI CN5G

## OAI gNB and OAI nrUE pre-requisites
***Build UHD from source:***
```
sudo apt install -y autoconf automake build-essential ccache cmake cpufrequtils doxygen ethtool g++ git inetutils-tools libboost-all-dev libncurses-dev libusb-1.0-0 libusb-1.0-0-dev libusb-dev python3-dev python3-mako python3-numpy python3-requests python3-scipy python3-setuptools python3-ruamel.yaml
git clone https://github.com/EttusResearch/uhd.git ~/uhd
cd ~/uhd
git checkout v4.7.0.0
cd host
mkdir build
cd build
cmake ../
make -j $(nproc)
make test # This step is optional
sudo make install
sudo ldconfig
sudo uhd_images_downloader

```
**Build OAI gNB and OAI nrUE**
---
In the OpenAirInterface (OAI) project, the terms gNB and nrUE refer to implementations of 5G components that are critical for building and testing 5G 
Networks.

gNB stands for Next-Generation Node B, which is the 5G base station in a 5G network. It provides the interface between the 5G core network (5GC) and the User Equipment (UE)

nrUE stands for New Radio User Equipment, which is the software implementation of a 5G user device. It simulates or acts as the mobile equipment connecting to the gNB in a 5G network.


```
git clone https://gitlab.eurecom.fr/oai/openairinterface5g.git ~/openairinterface5g
cd ~/openairinterface5g
git checkout develop

# Install OAI dependencies
cd ~/openairinterface5g/cmake_targets
./build_oai -I

# nrscope dependencies
sudo apt install -y libforms-dev libforms-bin

# Build OAI gNB
cd ~/openairinterface5g/cmake_targets
./build_oai -w USRP --ninja --nrUE --gNB --build-lib "nrscope" -C
```
***Workflow:***
---


  <ul>
<li>nrUE connects to the gNB over the 5G NR radio interface.</li>
<li>The gNB communicates with the 5G Core (OAI 5GC) for session setup, authentication, and data transfer.</li>
<li>Data flows between nrUE and the core network via the gNB.</li>

</ul>

**Run OAI gNB**
---
OAI gNB runs by default in standalone (SA) mode.
RFsimulator

```
cd ~/openairinterface5g/cmake_targets/ran_build/build
sudo ./nr-softmodem -O ../../../targets/PROJECTS/GENERIC-NR-5GC/CONF/gnb.sa.band78.fr1.106PRB.usrpb210.conf --gNBs.[0].min_rxtxtime 6 --rfsim

```
**OAI UE**
---
Run OAI nrUE
RFsimulator
Important notes:

This should be run on the same host as the OAI gNB
It only applies when running OAI gNB with RFsimulator

Run OAI nrUE with RFsimulator
```
cd ~/openairinterface5g/cmake_targets/ran_build/build
sudo ./nr-uesoftmodem -r 106 --numerology 1 --band 78 -C 3619200000 --uicc0.imsi 001010000000001 --rfsim
```
# Reference
https://gitlab.eurecom.fr/oai/openairinterface5g/-/blob/develop/doc/NR_SA_Tutorial_OAI_nrUE.md#rfsimulator

