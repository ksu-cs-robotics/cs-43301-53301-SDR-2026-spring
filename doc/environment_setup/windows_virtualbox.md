## ROS2 Humble on Ubuntu 22.04 via VirtualBox (Windows 11)

This guide sets up Ubuntu 22.04 in VirtualBox on Windows 11, then installs
ROS2 Humble inside the VM.

### Prerequisites
- Windows 11 with hardware virtualization enabled in BIOS/UEFI
- Oracle VirtualBox installed: https://www.virtualbox.org/wiki/Downloads
- Ubuntu 22.04 LTS Desktop ISO (amd64):
  https://releases.ubuntu.com/22.04/

### Recommended VM Resources
- CPU: 4+ cores
- RAM: 8+ GB
- Disk: 64+ GB (VDI, dynamically allocated)

### Step 1: Create Ubuntu 22.04 VM in VirtualBox
1. Open VirtualBox and click **New**.
2. Name it `Ubuntu 22.04`, select **Linux** and **Ubuntu (64-bit)**.
3. Set memory and CPU using the recommended resources.
4. Create a new virtual hard disk (VDI, dynamically allocated).
5. In **Settings -> Storage**, attach the Ubuntu ISO to the optical drive.
6. Start the VM and complete the Ubuntu Desktop installer.

### Step 2: Install VirtualBox Guest Additions (inside Ubuntu)
1. In the VM window, go to **Devices -> Insert Guest Additions CD Image**.
2. In Ubuntu, open a terminal and run:
   ```
   sudo apt update
   sudo apt install -y build-essential dkms linux-headers-$(uname -r)
   sudo /media/$USER/VBox_GAs_*/VBoxLinuxAdditions.run
   ```
3. Reboot the VM.

### Step 3: Install ROS2 Humble (inside Ubuntu)
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
- If you cannot select 64-bit Ubuntu, enable VT-x/AMD-V in BIOS/UEFI.
- If the display is laggy, enable 3D acceleration in VM settings.