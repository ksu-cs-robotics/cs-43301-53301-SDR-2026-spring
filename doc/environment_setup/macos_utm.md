## ROS2 Humble on Ubuntu 22.04 via UTM (macOS)

This guide sets up Ubuntu 22.04 in UTM on macOS, then installs ROS2 Humble
inside the VM. These steps are adapted from `doc/Environment_Setup.md`.

### Prerequisites
- Apple Silicon Mac (M1/M2/M3/M4/M5)
- UTM: https://mac.getutm.app/ (also see https://github.com/utmapp/UTM)
- Ubuntu 22.04 LTS ARM64 server ISO:
  https://cdimage.ubuntu.com/releases/jammy/release/

### Recommended VM Resources
- CPU: 6+ cores
- RAM: 8+ GB
- Disk: 64+ GB

### Step 1: Install UTM
1. Download UTM and drag it into your Applications folder.
2. Launch UTM and finish the initial setup prompts.

### Step 2 (Recommended): Use the Pre-Made VM Image
1. Download the pre-made VM image:
   https://drive.google.com/file/d/1UuV1H5NJpqLvvxisugsIbIEiUMPUkZcf/view?usp=sharing
2. Unzip it in the download location.
3. Open UTM, click **Create a New Virtual Machine**, then **Open**.
4. Select and open the `.utm` file.
5. Password for the VM: `discovery`

### Step 2 (Manual): Download Ubuntu 22.04 LTS ARM64
1. From the Jammy release page, download
   `ubuntu-22.04.5-live-server-arm64.iso`.
2. Keep the ISO in a known location (e.g., `~/Downloads`).

### Step 3: Create the VM
1. Open UTM and select **Create a New Virtual Machine**.
2. Choose **Virtualize** (not Emulate), then **Linux**, and **ARM64**.
3. Select the Ubuntu ARM64 ISO as the boot image.
4. Set CPU/RAM/disk using the recommended resources.
5. Finish the wizard and start the VM.

### Step 4: Install Ubuntu
1. Follow the Ubuntu installer to complete the server install.
2. Reboot into the installed system when prompted.

### Step 5: Install GUI + Guest Tools (inside Ubuntu)
Create a script and run it to install the desktop environment and guest tools.
```
$ cat <<'EOF' > install-ubuntu-desktop.sh
#!/usr/bin/env bash
set -e

sudo apt update
sudo apt install -y tasksel ubuntu-desktop
sudo apt install -y spice-vdagent spice-webdavd

echo "Rebooting to apply desktop/guest tools..."
sudo reboot now
EOF

$ chmod +x install-ubuntu-desktop.sh
$ ./install-ubuntu-desktop.sh
```

### Step 6: Install ROS2 Humble (inside Ubuntu)
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
- If the VM boots to a black screen, switch to a different display driver in UTM.
- If clipboard sharing does not work, ensure `spice-vdagent` is installed.
