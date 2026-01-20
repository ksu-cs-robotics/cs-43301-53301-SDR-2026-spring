## ROS2 Humble on Ubuntu 22.04 via Dual Boot (Windows 11)

This guide installs Ubuntu 22.04 alongside Windows 11 (dual boot), then
installs ROS2 Humble in Ubuntu.

### Prerequisites
- Windows 11 with admin access
- Backup of important files
- 40+ GB free disk space for Ubuntu
- Ubuntu 22.04 LTS Desktop ISO (amd64):
  https://releases.ubuntu.com/22.04/
- USB flash drive (8 GB or larger)
- Rufus:
  https://rufus.ie/en/

### Step 1: Prepare Windows
1. Back up important files.
2. If BitLocker is enabled, suspend it temporarily.
3. Disable Windows Fast Startup.
4. Use **Disk Management** to shrink the Windows partition and leave
   unallocated space for Ubuntu (40+ GB).

### Step 2: Create Ubuntu Installer USB
1. Download the Ubuntu 22.04 ISO.
2. Use Rufus or balenaEtcher to create a bootable USB.

### Step 3: Install Ubuntu 22.04
1. Boot from the USB drive (use the boot menu key for your system).
2. Start the Ubuntu installer.
3. Choose **Install Ubuntu alongside Windows Boot Manager** if available.
   If not, choose **Something else** and select the unallocated space.
4. Complete the installer and reboot.
5. Choose Ubuntu from the boot menu (GRUB).

### Step 4: Install ROS2 Humble (inside Ubuntu)
Create a script and run it inside Ubuntu 22.04:
```
$ cat <<'EOF' > install-ros2-humble.sh
#!/usr/bin/env bash
set -e

sudo apt update
sudo apt install -y locales
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8

sudo apt install -y software-properties-common
sudo add-apt-repository universe

sudo apt update
sudo apt install -y curl
ROS_APT_SOURCE_VERSION="$(curl -s https://api.github.com/repos/ros-infrastructure/ros-apt-source/releases/latest | grep -F "tag_name" | awk -F\" '{print $4}')"
curl -L -o /tmp/ros2-apt-source.deb "https://github.com/ros-infrastructure/ros-apt-source/releases/download/${ROS_APT_SOURCE_VERSION}/ros2-apt-source_${ROS_APT_SOURCE_VERSION}.$(. /etc/os-release && echo ${UBUNTU_CODENAME:-${VERSION_CODENAME}})_all.deb"
sudo dpkg -i /tmp/ros2-apt-source.deb

sudo apt update
sudo apt upgrade -y

sudo apt install -y ros-humble-desktop ros-dev-tools

echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc
source ~/.bashrc

echo "ROS 2 Humble installed. Open a new terminal or source ~/.bashrc"
EOF

$ chmod +x install-ros2-humble.sh
$ ./install-ros2-humble.sh
$ source /opt/ros/humble/setup.bash
```

### Gazebo
```
sudo apt install -y ros-humble-gazebo*
```

### Verify Installation
```
source /opt/ros/humble/setup.bash
echo $ROS_DISTRO
ros2 --help
```

### Troubleshooting
- If Ubuntu does not appear in the boot menu, check BIOS/UEFI boot order.
- If Wi-Fi drivers are missing, connect via Ethernet and install updates.
