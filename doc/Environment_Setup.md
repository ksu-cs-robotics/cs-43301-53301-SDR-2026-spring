<p align="center">
  <a href="" rel="noopener">
 <img src="../resources/images/ATR-logo.gif" alt="ATR"></a>
</p>

<h3 align="center">Software Development for Robotics</h3>

<div align="center">

  [![Status](https://img.shields.io/badge/status-active-success.svg)]() 
  [![build](https://img.shields.io/badge/build-melodic-green)]()
  [![License](https://img.shields.io/badge/license-MIT-blue.svg)](/LICENSE)

</div>

---

<p align="center"> ROS Installation
</p>


## 📝 Table of Contents
+ [0. Tools](#tools)
+ [1. PC Setup](#pc_setup)
+ [2. ROS Installation](#rosinstall)
+ [3. Software Setup](#software)
+ [4. Notes](#notes)

## 💬 0. Tools <a name = "tools"/>
#### We will use "Github" for homework submision and "Slack" for communication 

#### 0.1 Github: 
GitHub is a global company that provides hosting for software development version control using Git.

Instruction Link: [basic use](https://www.youtube.com/watch?v=SWYqp7iY_Tc)

#### 0.2 Slack
Slack is a cloud-based proprietary instant messaging platform developed by Slack Technologies.

Instruction Link: [basic use](https://www.youtube.com/watch?v=dJmdHowChWk)

### Chanels: 
- **Github:** Please Fork this Repo into your account. if you do not know how to do that, please watch [this video](https://www.youtube.com/watch?v=HbSjyU2vf6Y)

- **Slack:** Please send an emial to Instructor (xlin10@kent.edu) about your Slack handle and email 

## 💾 1. PC Setup <a name = "pc_setup"></a>
#### 1.1 Personal PC Setup
During this class, we are going to use [Ubuntu 18.04](https://ubuntu.com/download/desktop) as our main operating system. 

# BACK UP ALL YOUR IMPORTENT FILES BEFORE YOU CONTINUE

##### 1.1.1. You Already Have Ubuntu 16 or You Want to Wipe Entire PC:

- **CASE 0 - BLANK INSTALL:** 
  check out 1.1.2. CASE 0. do the same thing but skip the first part where you particition on Windows

- **CASE 1 - UPGRADE:** 
  If you already have **Ubuntu 16.04** insteaded, it is *preferable* to upgrade to 18.04.
  ```
  sudo apt-get update
  sudo apt-get upgrade
  sudo apt-get install update-manager-core
  sudo do-release-upgrade
  ``` 

##### 1.1.2. You Already Have Windows and You Dont Want To Wipe Anything:

- **CASE 0 - WINDOWS + UBUNTU (DUAL BOOT):** 
  Dual Booting installs both system. You can run one or the other full speed with full and full features, but only run one at a time.

  Instruction Link: [Dual boot Windows and Ubuntu](https://www.youtube.com/watch?v=u5QyjHIYwTQ) (Choose Option#1 - install ubuntu alongside windows boot manager) 

- **CASE 1 - WINDOWS + VIRTUAL MACHINE (VM):** 
  Running a VM allows running both system at the same time with full features, but slows down the VM (and potentially also the host).

  Instruction Link: [Virtual Box on Windows 10](https://www.youtube.com/watch?v=sB_5fqiysi4) (replace ubuntu *16.04* with *18.04*)

- **CASE 2 - WINDOWS + UBUNTU BASH (WSL):** 
  WSL is APIs and libraries to enable binary compatibility, but you aren't actually running Linux, and some programs are incompatible. Full speed, limited features.

  Instruction Link: [Windows 10 Bash & Linux Subsystem](https://www.youtube.com/watch?v=Cvrqmq9A3tA&t=243s). it does not provide a GUI interface.(you can use **X Servers** to bypass it, but it is bugyy to use)

##### 1.1.3. Install OS on Raspberry pi

- **CASE 0 Raspberry Pi 3:** Please follow [this video]() link to setup your raspberry pi

- **CASE 1 Raspberry Pi 4:** Since the offical Ubuntu support for raspberry pi 4 havent come out yet. You can install [Raspbian Buster with desktop](https://www.raspberrypi.org/downloads/raspbian/), the setup process would be the similar as above.

- **!!->> DVI type monitor Setup: <<-!! (NOTES)** 
  ```
  sudo nano /boot/config.txt
  ```
  ```
  ### edit these lines ###
  hdmi_force_hotplug=1
  hdmi_drive=1
  hdmi_group=2
  hdmi_mode=82
  ```

#### 1.2 Mac (Apple Silicon) - UTM Virtual Machine (Ubuntu 22.04 LTS ARM64) <a name = "mac_utm"></a>
If you are on an Apple Silicon Mac (M1/M2/M3/M4/M5), use UTM to run Ubuntu 22.04 LTS (Jammy) in a VM.

**Downloads**
- UTM: https://mac.getutm.app/ (also see https://github.com/utmapp/UTM)
- (If creating manually) Ubuntu 22.04 LTS ARM64 server ISO (Jammy): https://cdimage.ubuntu.com/releases/jammy/release/

**Step 1: Install UTM**
1. Download UTM from the site above and drag it into your Applications folder.
2. Launch UTM and finish the initial setup prompts.

**Recommended**
- Download the pre-made VM image here: https://drive.google.com/file/d/1UuV1H5NJpqLvvxisugsIbIEiUMPUkZcf/view?usp=sharing
- Unzip it in the download location.
- Open UTM, "Create a New Virtual Machine", "Open".
- Select and open the .utm file.
- Password for the VM: discovery

**(If installing manually) Step 2: Download Ubuntu 22.04 LTS ARM64**
1. From the Jammy release page, download `ubuntu-22.04.5-live-server-arm64.iso`.
2. Keep the ISO in a known location (e.g., `~/Downloads`).

**Step 3: Create the VM**
1. Open UTM and select **Create a New Virtual Machine**.
2. Choose **Virtualize** (not Emulate), then **Linux**, and **ARM64**.
3. Select the Ubuntu ARM64 ISO as the boot image.
4. Suggested resources: 6+ CPU cores, 8+ GB RAM, 64+ GB disk.
5. Finish the wizard and start the VM.

**Step 4: Install Ubuntu**
1. Follow the Ubuntu installer to complete the server install.
2. Reboot into the installed system when prompted.

**Step 5: Install GUI + Guest Tools (run inside Ubuntu)**
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

## 🏁 2. ROS Installation <a name = "rosinstall"></a>

ROS(Robot Operating System) is a popular framework for building the software for robot now adays, which we will also use it to build our Robot.

**Simple But Important Linux Conept and Command Line Tutorial Video (7min)**
https://www.youtube.com/watch?v=YHFzr-akOas

---

do follow steps to update your system first regardless which version of Ubuntu you are using
```
sudo apt update
sudo apt upgrade
```
### 2.1 Ubuntu 16 & ROS Kinetic
here is the more detail explanation of how to [install ROS on ubuntu 16](http://wiki.ros.org/kinetic/Installation/Ubuntu)

here is simplify version if you are having trouble following the tutorial link:
```
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'

sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654

sudo apt-get update

sudo apt-get install ros-kinetic-desktop-full

sudo rosdep init

rosdep update

echo "source /opt/ros/kinetic/setup.bash" >> ~/.bashrc

source ~/.bashrc

sudo apt install python-rosinstall python-rosinstall-generator python-wstool build-essential
```
### 2.2 Ubuntu 18 & ROS Melodic
here is the more detail explanation of how to [install ROS on ubuntu 18](http://wiki.ros.org/melodic/Installation/Ubuntu)

here is simplify version if you are having trouble following the tutorial link:
```
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'

sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654

sudo apt update

sudo apt install ros-melodic-desktop-full

sudo rosdep init

rosdep update

echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc

source ~/.bashrc

sudo apt install python-rosinstall python-rosinstall-generator python-wstool build-essential
```
### 2.3 Ubuntu 20 & ROS Noetic
here is the more detail explanation of how to [install ROS on ubuntu 20](http://wiki.ros.org/noetic/Installation/Ubuntu)

here is simplify version if you are having trouble following the tutorial link:
```
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'

sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654

sudo apt update

sudo apt install ros-noetic-desktop-full 

for Rpi 3B+, "sudo apt install ros-noetic-desktop"

sudo rosdep init

rosdep update

echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc

source ~/.bashrc

sudo apt install python-rosinstall python-rosinstall-generator python-wstool build-essential
```

### 2.4 Ubuntu 22.04 & ROS 2 Humble (Recommended for Apple Silicon VM)
Official guide: https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debs.html

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
$ sudo add-apt-repository ppa:openrobotics/gazebo11-non-amd64
$ sudo apt update
$ sudo apt install -y gazebo*
```

## 👨🏻‍💻 3. Software Setup <a name = "software"></a>

### 3.1. IDE
- **Raspberry pi:**

  Bluefish: ``` sudo apt install bluefish ```

- **Personal PC:**

  Visual Studio Code: check [this link](https://code.visualstudio.com/) and download **.deb**
    1. Option 1: left click on the file to install 
    1. Option 2: open terminal and enter 
    ``` 
    cd ~/Downloads/
    sudo dpkg -i code_1.41.1-1576681836_amd64.deb 
    ## check the name of the file you just download, it might be different
    ```
### 3.2 Terminal Tool:
  - on both Raspberry pi and Personal PC
    ```
    sudo apt install terminator
    ```
    you can **right click** on Terminator to split windows, so that you can have multiple terminal in one windows

### 3.3 SSH
- **Raspberry pi:**

  enable ssh:
  ```
  sudo apt install ssh
  sudo systemctl enable ssh
  sudo systemctl start ssh
  sudo rm /etc/ssh/ssh_host_*
  sudo dpkg-reconfigure openssh-server
  ```

- **Personal PC:**

  1. **Ubuntu:** open terminal and install ssh ```sudo apt install ssh```

      **Windows:** [install putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html), under "MSI (‘Windows Installer’)" -> "64-bit" -> "putty-64bit-0.73-installer.msi"

  1. Generate ssh config file: open terminal (Ubuntu) or CMD (Windows), enter ```ssh remote_PC_username@remote_PC_IP_address``` for example, username for our raspberry pi is atr@cs-ksu, and ip is 192.168.1.31. so you need to use ```ssh atr@192.168.1.31```

  1. Open **Visual Studio Code** and click on **Extensions** on the left side, install "Remote - SSH **(Nightly)**" (this extension work best with "VS Code Insiders", but it is not required to have that, "VS Code" normal version would also work)

  1. check out [this tutorial](https://www.hanselman.com/blog/VisualStudioCodeRemoteDevelopmentOverSSHToARaspberryPiIsButter.aspx)
