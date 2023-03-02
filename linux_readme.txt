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

# Kill infinite looping program
1) ctrl + z [move program to background]
2) $ kill %1  [kill the 1st program in background]
3) $ fg [move program to foreground to check]


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
6) $ sudo reboot


# Autoconnect wifi (raspberrypi)
1) $ sudo nano /etc/netwoek/interfaces
2) then write down the command as follow:

allow-hotplug wlan0
auto wlan0
iface wlan0 inet dhcp
wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf

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

6) $ sudo nano /boot/config.txt
7) then write down the command as follow:

dtoverlay=enable-wifi

8) ctrl + x, y, enter
6) $ sudo reboot


# Install X2Go Server (https://www.howtoforge.com/tutorial/x2go-server-ubuntu-14-04)
      $ sudo apt-get install software-properties-common
      $ sudo add-apt-repository ppa:x2go/stable
      $ sudo apt-get update
      $ sudo apt-get install x2goserver x2goserver-xsession
      $ sudo apt-get install x2gomatebindings

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

# Install vscode
      $ sudo snap install code --classic

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
3) scan the subnet of the router from 1 to 255 [0/24] (change xx accordingly)
      $ sudo nmap -sn 192.168.xx.0/24
4) if no hostname called "Raspberry Pi" detected, do this (change xx accordingly)
      $ sudo nmap -sP 192.168.xx.0/24 | awk '/^Nmap/{ip=$NF}/B4:CB:FB/{print ip}'

# Arduino IDE upload: permission denied (change accordingly)
      $ sudo chmod a+rw /dev/tty*


# Fix VNC Desktop low resolution (Raspberry Pi)
1) $ sudo nano /boot/config.txt
2) find the following lines and UNCOMMENT them (or change/add accordingly):
      framebuffer_width=1920
      framebuffer_height=1080
      hdmi_force_hotplug=1
      hdmi_group=2
      hdmi_mode=16
3) find the following lines and COMMENT them:
      #dtoverlay=vc4-kms-v3d
      #max_framebuffers=2
4) ctrl + x, y, enter
5) sudo reboot


# Set-up LAMP Server
1) $ sudo apt-get update
2) $ sudo apt-get install apache2 -y
3) $ sudo apt-get install php -y
4) $ sudo apt-get install mariadb-server mariadb-client php-mysql -y
7) $ sudo apt-get install phpmyadmin -y
5) create 'root' password=[root_password]
      $ sudo mysql_secure_installation
6) configure 'root' privileges
      $ sudo mysql -u root -p
            > GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost';
            > FLUSH PRIVILEGES;
            > EXIT;
7) $ sudo nano /etc/apache2/apache2.conf
8) add the following line:
      Include /etc/phpmyadmin/apache.conf
9) ctrl + x, y, enter
10) $ sudo phpenmod mysqli
11) $ sudo service apache2 restart
12) check IP address for localhost [192.168.xx.yy]
      $ hostname -I
13) access PHPMyAdmin
      http://localhost/phpmyadmin
            or
      http://192.168.xx.yy/phpmyadmin
14) change the communication method to TCP/IP for remote server
      $ sudo mysql --host=localhost --protocol=TCP -u root -p
      $ status;


