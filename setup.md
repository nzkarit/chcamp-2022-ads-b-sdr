These are some instruction I put together to get the VM up and running. I have been working on on Ubuntu 22.04 Desktop.

# Install the relevant helper tools for the VM
1. Virtualbox `sudo apt install virtualbox-guest-utils virtualbox-guest-x11`
1. Reboot
1. Now clipboard, resize etc should work

# PreReq
1. `sudo apt install git`
1. `git clone https://github.com/nzkarit/chcamp-2022-ads-b-sdr.git`
1. `sudo apt install cmake libusb-dev libusb-1.0-0-dev open-vm-tools build-essential librtlsdr-dev pkg-config python3-numpy`
1. `sudo apt-get install librtlsdr-dev libusb-1.0-0-dev pkg-config debhelper`

# Setup RTLSDR
1. `git clone git://git.osmocom.org/rtl-sdr.git`
1. `cd rtl-sdr`
1. `mkdir build`
1. `cd build`
1. `cmake ../ -DINSTALL_UDEV_RULES=ON`
1. `make`
1. `sudo make install`
1. `sudo ldconfig`
1. `sudo cp ../rtl-sdr.rules /etc/udev/rules.d/`
1. `sudo vi /etc/modprobe.d/blacklist-rtl.conf`
1. Add `blacklist dvb_usb_rtl28xxu`
1. Reboot

## Test RTLSDR
1. Plug in an rtlsdr
1. `rtl_test`

# Setup HackRF
1. `sudo apt install hackrf`

## Test HackRF
1. `sudo hackrf_info`

# Setup dump1090
1. `sudo apt install dump1090-mutability`
1. Don't select auto start
1. `sudo mkdir /run/dump1090-mutability`

## Test dump1090
1. `sudo dump1090-mutability --net --write-json /run/dump1090-mutability/ --write-json-every 1`
1. Browse to http://localhost/dump1090/
1. If there are planes overhead with ADS-B you should see them plotted on the map

# Setup ADSB Out
1. `git clone https://github.com/nzkarit/ADSB-Out.git`

## Test ADSB Out
1. `sudo dump1090-mutability --net --write-json /run/dump1090-mutability/ --write-json-every 1 --freq 915000000`
1. Browse to http://localhost:8080/
1. `cd ADSB-Out`
1. `./ADSB_Encoder.py`
1. `sudo hackrf_transfer -t Samples_256K.iq8s -f 915000000 -s 2000000 -x 10`
1. In browser observe a plane on the map
