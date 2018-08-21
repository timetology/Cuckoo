#Cuckoo-ESX

#Ubuntu 16.04 Server

#Change interface names back to eth#
Edit your /etc/default/grub changing the line from

GRUB_CMDLINE_LINUX=""
to

GRUB_CMDLINE_LINUX="net.ifnames=0 biosdevname=0"
and, finally:

$ sudo update-grub
and reboot your system:

$ sudo reboot


#Install Core Dependencies
sudo apt-get update
sudo apt-get upgrade –y
sudo apt-get install python python-pip mongodb python-sqlalchemy python-bson python-dpkt python-jinja2 python-magic python-pymongo python-gridfs python-bottle python-pefile python-chardet python-django libffi-dev libssl-dev -y

sudo apt-get install open-vm-tools

#Add Cuckoo to cuckoo group
sudo usermod -a -G cuckoo cuckoo

sudo apt-get install gcc make pkg-config libxml2-dev libgnutls-dev libdevmapper-dev libcurl4-gnutls-dev python-dev libpciaccess-dev libxen-dev libnl-3-dev uuid-dev xsltproc libnl-route-3-dev libxml2-utils -y

wget https://libvirt.org/sources/libvirt-4.6.0.tar.xz
tar xJf libvirt-4.6.0.tar.xz libvirt-4.6.0/
cd libvirt-4.6.0/
./configure --prefix=/usr --localstatedir=/var --sysconfdir=/etc --with-esx=yes
make
sudo make install

### SNAPSHOT - Libvirt-installed ###


sudo apt-get install python-dev libffi-dev libssl-dev python-virtualenv python-setuptools libjpeg-dev zlib1g-dev swig mongodb -y

#Install tcpdump (if not already installed)
sudo apt-get install tcpdump libcap2-bin apparmor-utils -y
sudo aa-disable /usr/sbin/tcpdump
sudo setcap cap_net_raw,cap_net_admin=eip /usr/sbin/tcpdump

#Install pydeep
sudo apt-get install ssdeep python-pyrex libfuzzy-dev -y
git clone https://github.com/kbandla/pydeep.git
cd pydeep
sudo python setup.py install

#Install yara
sudo apt-get install libpcre3 libpcre3-dev autoconf libtool libjansson-dev libmagic-dev -y
wget https://github.com/VirusTotal/yara/archive/v3.8.0.tar.gz
tar zxf v3.8.0.tar.gz yara-3.8.0
cd yara-3.8.0/
./bootstrap.sh
./configure --enable-cuckoo --enable-magic --enable-dotnet
make
sudo make install

#Yara-python
sudo pip install yara-python

## SNAPSHOT!! - Pre-MITMProxy

sudo pip install pillow
sudo pip install distorm3
sudo pip install pycrypto
sudo apt-get install python-imaging -y

sudo -H pip install openpyxl
sudo pip install ujson

sudo apt-get install mitmproxy -y

#Volatility -- SKIPPPED

#git clone https://github.com/volatilityfoundation/volatility.git
#cd volatility
#sudo python setup.py install

#mcrypto
sudo -H pip install m2crypto==0.24.0

#guacd
sudo apt-get install libguac-client-rdp0 libguac-client-vnc0 libguac-client-ssh0 guacd -y

### SNAPSHOT - Pre-Cuckoo Install ###

#Install Cuckoo in virtualenv
virtualenv venv
. venv/bin/activate
pip install -U pip setuptools
pip install -U cuckoo

