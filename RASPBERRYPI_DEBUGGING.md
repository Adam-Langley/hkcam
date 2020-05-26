# Building and Debugging on Raspberry Pi

CANNOT USE UBUNTU > 18 because it doesn't have the VideoCore binaries available in online repo.
DONT USE CORE - requires online account management - yuck.
use this one
https://ubuntu.com/download/raspberry-pi/thank-you?version=18.04&versionPatch=.4&architecture=arm64+raspi3


setup wifi
https://raspberrypi.stackexchange.com/questions/98598/how-to-setup-the-raspberry-pi-3-onboard-wifi-for-ubuntu-server-18-04-with-netpla

Debugging Go requires running Delve on the Raspberry pi, which currently only supports ARM64 - that rules out Raspbian.
Use Ununtu as per these instructions:
https://blog.jetbrains.com/go/2020/02/18/running-goland-on-a-raspberry-pi-4/#installing-the-operating-system

DONT USE A NEWER UBUNTU THAN 19.10 - otherwise you'll get a failure when install the RPI dev binaries



BUILDING PI BINARIES
https://raspberrypi.stackexchange.com/questions/37359/how-to-use-raspistill-on-ubuntu/37387#37387

#NOTE: You will get an error when building the "userland binaries". You need to install the following dependency.
sudo apt-get update && sudo apt-get install build-essential


sudo apt-get install ffmpeg

# install go - make sure you have at least 1.13 or your will get error
error: go/src/github.com/brutella/hc/crypto/ed25519.go:5:2: cannot find package "crypto/ed25519" in any of:
(https://github.com/golang/go/wiki/Ubuntu)

sudo add-apt-repository ppa:longsleep/golang-backports
sudo apt update
sudo apt install golang-1.13-go
export PATH=$PATH:/usr/lib/go-1.13/bin/


# CURRENT BEST SETUP GUID, INCLUDES HOW TO ADD GO TO PATH
https://www.linode.com/docs/development/go/install-go-on-ubuntu/

# add this to ~/.profile
if [ -d "$HOME/.local/bin" ] ; then
    PATH="$PATH:/usr/lib/go-1.13/bin/"
fi

# then initialize it
source ~/.profile

# and init it for sudo
sudo visudo

# install gstreamer
https://gstreamer.freedesktop.org/documentation/installing/on-linux.html?gi-language=c

# install delve for debugging
# MAKE SURE YOURE NOT INSIDE A MODULE FOLDER OR YOU"LL GET WEIRED ERRORS WHEN INSTALLING
go get github.com/go-delve/delve/cmd/dlv


# getting starteding with debugging
https://github.com/Microsoft/vscode-go/wiki/Debugging-Go-code-using-VS-Code

# launching go program with debugging server
~/go/src/hkcam/cmd/hkcam$ sudo dlv debug --headless --listen=:2345 --log --api-version=2

# vscode debugging configuration
        {
            "name": "Remote",
            "type": "go",
            "request": "attach",
            "mode": "remote",
            "port": 2345,
            "host": "10.0.1.186",
            // "trace": "verbose",
            "remotePath": "/home/ubuntu/go/src/hkcam/",
        }


# make sure ssh is set up passwordless for smooth debugging
https://superuser.com/questions/555799/how-to-setup-rsync-without-password-with-ssh-on-unix-linux

# set up pre-launch task
        {
            "name": "Remote",
            "type": "go",
            "request": "attach",
            "mode": "remote",
            "port": 2345,
            "host": "10.0.1.186",
            // "trace": "verbose",
            "remotePath": "/home/ubuntu/go/src/hkcam/",
            "preLaunchTask": "deployAndLaunch",
        }