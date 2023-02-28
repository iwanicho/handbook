[Essential Command Line Ubuntu]

# Initial setup
      $ sudo apt update
      $ sudo apt autoremove
      $ sudo apt upgrade
      $ sudo reboot

# Install and enable SSH
      $ sudo apt install openssh-server
      $ sudo systemctl enable ssh.service
      $ sudo systemctl start ssh.service
      $ sudo dpkg-reconfigure openssh-server

# Checking the IP Address on the network
      $ hostname -I

# Autoconnect wifi (ubuntu)
      1) $ sudo nano /etc/netplan/50-cloud-init.yaml
      2) then write down the connection as follow:

network:
    ethernets:
        eth0:
            dhcp4: true
            optional: true
    version: 2
    wifis:
        renderer: networkd
        wlan0:
            dhcp4: true
            optional: true
            access-points:
                "Haiiii":
                    password: "gogetbronze"
        wlan0:
            dhcp4: true
            optional: true
            access-points:
                "Kontrakan50":
                    password: "apaansih"
        wlan0:
            dhcp4: true
            optional: true
            access-points:
                "ITBdeLabo_1":
                    password: "Delabo0220!"

      3) ctrl + x, y, enter
      4) $ sudo netplan generate
      5) $ sudo netplan apply
      5) $ sudo reboot

# Autoconnect wifi (raspberrypi)
      1) $ sudo nano /etc/netwoek/interfaces
      2) then write down the command as follow:

auto wlan0
allow-hotplug wlan0
iface wlan0 inet dhcp
wpa-con /etc/wpa_supplicant/wpa_supplicant.conf
iface deafult inet dhcp

      3) ctrl + x, y, enter
      4) $ sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
      5) then write down the connection as follow:

network={
      ssid="network_A"
      psk="password_A"
      key_mgmt=WPA-PSK
      priority=1
}
network={
      ssid="network_B"
      psk="password_B"
      key_mgmt=WPA-PSK
      priority=2
}

      5) $ sudo netplan apply
      5) $ sudo reboot


# Install X2Go Server (https://www.howtoforge.com/tutorial/x2go-server-ubuntu-14-04)
      1) $ sudo apt-get install software-properties-common
      2) $ sudo add-apt-repository ppa:x2go/stable
      3) $ sudo apt-get update
      4) $ sudo apt-get install x2goserver x2goserver-xsession
      5) $ sudo apt-get install x2gomatebindings

# Install ROS
      $ sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
      $ sudo apt install curl # if you haven't already installed curl
      $ curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
      $ sudo apt-get update
      $ sudo apt install ros-melodic-desktop-full
      $ echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc
      $ source ~/.bashrc
      $ sudo apt install python-rosdep python-rosinstall python-rosinstall-generator python-wstool build-essential
      $ sudo rosdep init
      $ rosdep update

# Install ROS package
      $ sudo apt-get install ros-<ros version>-<package-name>

# Make a directory
      $ mkdir directory_name

# Open directory
      $ cd directory_name

# List content of a directory
      $ ls directory_name

# Make a file
      $ touch filename.extension

# Make a executable code for python
      $ chmod +x python_filename.py

# Edit a file
      $ gedit file_directory

# Install vscode "sudo snap install code --classic"
# Open vscode
      $ code .

# Checking RAM Usage
      $ free -h

# Python Virtual Environment
1) install python3
      $ sudo apt-get install python3
2) install pip
      $ sudo apt-get install python3-pip
3) install virtualenv package
      $ sudo pip3 install virtualenv
      $ sudo apt install python3-virtualenv
4) create virtual environment
      $ virtualenv <venv_name>
5) activate virtual environment
      $ source <venv_name>/bin/activate
6) install python package using pip
      $ pip install <package_name>
7) deactivate venv
      $ deactivate

# Network Mapping (search RasPi IP address)
1) install nmap package
      $ sudo apt-get install nmap
2) retrieve the local IP address of your current computer
      $ hostname -I
3) scan the subnet of the router from 1 to 255 [0/24] (change accordingly)
      $ sudo nmap -sn 192.168.18.0/24
4) if no hostname called "Raspberry Pi" detected, do this (change accordingly)
      $ sudo nmap -sP 192.168.18.0/24 | awk '/^Nmap/{ip=$NF}/B4:CB:FB/{print ip}'

# Arduino IDE upload: permission denied (change accordingly)
      $ sudo chmod a+rw /dev/tty*