AMD has decided to abandon several older Xilinx products, including the
XC9500XL CPLDs. Why not use the newer products, you might ask?
-- the FPGAs they recommend as replacements only come in BGA packages.
-- the older CPLDs are 5-volt tolerant, the newer devices are not.
-- the older CPLDs come in PLCC-44 packages which can be socketed.
-- several years ago I bought a couple tubes of XC9572XL for $3 something
   each, the newer devices are considerably more expensive.
-- I don't feel like switching to the Actel or Lattice ecosystem right
   now, though I'm seriously considering switching to Efinix.

The ISE programming software has not been updated in 10 years and no
longer runs under Windows 10 or 11. The older devices are not supported
by Vivado. AMD/Xilinx's solution is to run the Linux version of ISE from
a VirtualBox VM, which takes up a huge amount of disk space. And there
is still the problem of getting the programming probe to work.

The solution I found to work is to run the Linux version of ISE on a
real Linux machine. I had bought a couple Lenovo ThinkCentre micro-PCs
on eBay for $35 each, and got them working using some smaller RAMs and
M.2 SSDs I had laying around. The Linux PC is a M710q with an i5 CPU, 8
gigs of RAM, and 500 gig of SSD. It runs Linux Mint. It shares my main
keyboard and mouse via a USB switch. The monitor has three input jacks,
and the Linux PC is connected to one of those.

I'm going to try to capture everything I had to do to install and setup
the tools by looking at a dump of my command line history. This was a
fairly long "three steps forward and two-steps back" process that
eventually succeeded, but I may record some steps that were not actually
necessary.

You may have to install libncurses first.
"sudo apt install libncurses5"

To install ISE go to
https://www.xilinx.com/downloadNav/vivado-design-tools/archive-ise.html
Download ISE 14.7 (not "ISE 14.7 Windows 10"). It comes in four large
files. Put the four files into a scratch directory. Untar part 1, run
"sudo ./xsetup", and the installer will walk you through the procedure.
Select the WebPack installation. Install the tools into the default
/opt/Xilinx. Do not install the cable drivers.

You need a USB driver for the Xilinx programming probe. I have a Xilinx
Platform cable USB II, and the following procedure worked for this
cable.

There is an open-source cable driver. The original can be found here:
https://rmdir.de/~michael/xilinx, and the source can be downloaded via
"git clone http://git.zerfleddert.de/git/usb-driver". There are also
several verbatim mirrors of this repository on GitHub, for example
"git clone https://github.com/dennisfen/xilinx-usb-driver.git".

You will need libusb-dev installed: "sudo apt install libusb-dev".
Depending on where you cloned it from, the working directory is called
usb-driver (original), or xilinx-usb-driver (mirrors). Go into that
directory and type "make". There may be a couple innocuous warnings.
When it is finished copy libusb-driver.so to your ~/bin directory.

I use the following scripts to start ise and impact. The first is called
"ise" and is located in my bin directory:

source /opt/Xilinx/14.7/ISE_DS/settings64.sh
export LD_PRELOAD=$HOME/bin/libusb-driver.so
ise &

The second is called impact:

source /opt/Xilinx/14.7/ISE_DS/settings64.sh
export LD_PRELOAD=$HOME/bin/libusb-driver.so
impact &

ISE will also run under WSL2 Debian, however, the screen rendering is a
little shaky, and I had problems with it. I was not able to get Impact
to talk to a USB cable under WSL2 Debian.




