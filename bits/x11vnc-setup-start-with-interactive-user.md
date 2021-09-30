---
title: "X11vnc Setup Start With Interactive User"
created: 2021-08-13 18:40:23
tags:
keywords: x11, vnc
---

# [X11vnc Setup Start With Interactive User](https://gist.githubusercontent.com/mgeeky/8e068b978095053b100ad49a6e9559d8/raw/161797a6eca86bc72338889e040be9a8a959e77b/ubuntu-x11vnc.md)

## Ubuntu x11vnc setup with start upon interactive user login to the without-monitor box

### Quick intro

After much time wasted with struggling to get VNC working on Ubuntu that stands for **without monitor box** - I've compiled the below draft notes on how I finally managed to configure the damn VNC.
This should be stated, that I've come a long journey from systemd/init.d configuration scripts, through various VNC daemons (tightvnc, tigervnc, xvnc) and experienced lot of issues (VNC starting before X, VNC not having Xauthority, VNC not being able to open display :0, and so on) .

Finally, this below setup got me working with VNC.

### Setup

Initial installation and VNC password creation:

```bash
sudo apt-get install x11vnc
sudo x11vnc -storepasswd "vnc-password" /etc/x11vnc.pass
sudo chmod a+r /etc/x11vnc.pass
```

The last command may be arguably **very unsafe** but it's necessary for launching x11vnc as an ordinary user in _rfbauth_ access mode.

First of all, we have to install _xorg dummy video driver_ like so:
`sudo apt-get install xserver-xorg-video-dummy`

Then inside of `/usr/share/X11/xorg.conf.d/` one has to create a file named **xorg.conf** with the following contents:

```conf
Section "Device"
    Identifier  "Configured Video Device"
    Driver      "dummy"
EndSection

Section "Monitor"
    Identifier  "Configured Monitor"
    HorizSync 31.5-48.5
    VertRefresh 50-70
EndSection

Section "Screen"
    Identifier  "Default Screen"
    Monitor     "Configured Monitor"
    Device      "Configured Video Device"
    DefaultDepth 24
    SubSection "Display"
    Depth 24
    Modes "1920x1080"
    EndSubSection
EndSection
```

Where the **Modes** directive stands for work resolution.

Then, using a following script `$HOME/start-vnc.sh`:

```bash
#!/bin/bash

pidfile=$HOME/x11vnc.pid
vncpath=/usr/bin/x11vnc
vnclog=/var/log/x11vnc
vncoptions="-display :0 -xrandr -forever -loop -noxdamage -repeat -rfbauth /etc/x11vnc.pass -rfbport 5904 -shared"

action=$1

function start_vpn {
  $vncpath $vncoptions > $vnclog 2>&1 &
  echo $! > $pidfile
  cat $pidfile
  sleep 1

  if netstat -plutan 2>/dev/null | grep LISTEN | grep x11vnc | grep -q 5904 ; then
    echo "x11vnc started listening on port 5904."
  else
    echo "[!] x11vnc is not listening!"
  fi
}

if [ -f "$pidfile" ]; then
  if [ "$action" == "start" ] || [ "$action" == "" ]; then
    if [ -s "$pidfile" ] && [[ `cat $pidfile` > 0 ]]; then
      if pgrep $vncpath ; then
        echo "x11vnc: running (pid file)."
      else
        killall $vncpath 2> /dev/null
        echo 0 > $pidfile
        start_vpn
      fi
    elif pgrep $vncpath ; then
      echo "x11vnc: running (pgrep)."
    else
      start_vpn
    fi
  elif [ "$action" == "stop" ]; then
    killall $vncpath 2> /dev/null
    echo 0 > $pidfile
    cat $pidfile
  fi

else
  echo "[!] PIDFILE: $pidfile does not exists, cannot proceed."
fi
```

(**WARNING**: In order to limit access to the **x11vnc** it is strongly advised to add **-localhost** switch to the `vncoptions` variable within above script)

Of course we will have to grant execute permission to this script file:

`$ chmod +x $HOME/start-vnc.sh`

We will be able to launch VNC on demand easily. Before we use it, this will be necessary to touch a pid file:

`$  touch ~/x11vnc.pid`

Then launching it from `~/.profile` can be arranged like so:

```bash
[...]
$HOME/start-vnc.sh start
```

In order to invoke this script one has to at least ssh into the machine.:

```bash
ssh user@192.168.0.17
Welcome to Ubuntu 16.04.1 LTS (GNU/Linux 4.4.0-59-generic x86_64)
[...]

You have new mail.
Last login: Thu Jan 19 16:36:32 2017
3241
x11vnc started listening on port 5904.
user@hostname:~$
```

We can observe from the second to the last line that the VNC server has been observed to listening.
