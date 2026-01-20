## ROS2 Humble on Ubuntu 22.04 via Parallels Desktop (macOS)

This guide sets up Ubuntu 22.04 in Parallels Desktop on macOS, then installs
ROS2 Humble inside the VM.

### Prerequisites
- Apple Silicon Mac (M1/M2/M3/M4/M5) recommended
- Parallels Desktop installed
- Ubuntu 22.04 LTS ARM64 Desktop ISO:
  https://cdimage.ubuntu.com/releases/jammy/release/

### Recommended VM Resources
- CPU: 6+ cores
- RAM: 8+ GB
- Disk: 64+ GB

### Step 1: Create Ubuntu 22.04 VM in Parallels
1. Open Parallels Desktop and click **Create New**.
2. Choose **Install Windows or another OS from a DVD or image file**.
3. Select the Ubuntu 22.04 ARM64 ISO.
4. Choose **Linux** and set the VM name and location.
5. Customize resources (see recommended values) and finish the wizard.
6. Start the VM and complete the Ubuntu installer.

### Step 2: Install Parallels Tools (inside Ubuntu)
1. In the Parallels menu, select **Actions -> Install Parallels Tools**.
2. If prompted, enter your Ubuntu password to mount and run the installer.
3. Reboot the VM after installation completes.

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
sudo add-apt-repository ppa:openrobotics/gazebo11-non-amd64
sudo apt update
sudo apt install -y gazebo*
```

### Verify Installation
```
source /opt/ros/humble/setup.bash
echo $ROS_DISTRO
ros2 --help
```

### Troubleshooting
- If the VM is slow, increase CPU/RAM and ensure macOS is not under heavy load.
- If Parallels Tools fails, rerun the installer after `sudo apt update`.
