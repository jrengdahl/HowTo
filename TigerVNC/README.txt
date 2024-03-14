This describes the trick to replacing TightVNC with TigerVNC on a simple
home network setup.

I had been using TightVNC for a long time. When I got a new laptop with
a different sized screen I wondered if a different version of VNC might
handle screen sizes automatically, and discovered that TigerVNC can.

However, one major difference between the two is that TigerVNC, by
default, disables direct access from the network -- you can only connect
from localhost, evidently assuming that you are using a SSH tunnel. The
documentation and other web info for TigerVNC did not make that clear
while I was trying to figure out why TightVNC worked and TigerVNC did
not. Eventually it was ChatGPT-4 that told me what the problem was.

Linux Mint runs on a ThinkCentre tiny PC, and the laptop runs Windows.
If the Linux PC has a firewall, then ports 22, 5900, and 5901 (at
minimum) need to be open for incoming TCP traffic. Install
tigervnc-standalone-server on the Linux PC. Install the Tiger VNC viewer
on the Windows PC.

Use PuTTY to ssh from the Windows PC to the Linux PC. On the Linux
PC run this script: ~/bin/vnc

vncserver -kill :1
tigervncserver -localhost no -geometry 1366x768 -xstartup /usr/bin/startxfce4

The key is that in order to accept a connection from the network you
have to put "-localhost no" on the command line.





