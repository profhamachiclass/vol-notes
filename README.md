# Volatility Notes: Digital Forensics
The Volatility framework is a free and open-source memory forensics tool. It is to monitor incident response and malware analysis. Volatility memory dump analysis tool was created by Aaron Walters in academic research while analyzing memory forensics. Volatility is a completely open collection of tools, written in Python language and released under the GNU General Public License. It is used for extraction of digital artifacts from volatile memory (RAM) samples and supports Linux, Windows and Mac OS.

Volatility memory forensics framework is intended to introduce extraction techniques and complexities associated with digital artifacts from volatile memory samples at runtime. Volatility memory extraction utility framework runs on any platform that supports Python. Volatility forensics open source software has 5.1K GitHub stars and 1.1k GitHub forks.

To look up a Enumerated PCI Vendor/Device ID, click [here](https://devicehunt.com/)

## Volatility
If you are running the custom Kali VM, volatility has been **pre-installed**. If you need to manually install, you can run:
```bash
git clone https://github.com/volatilityfoundation/volatility.git
```

## Where to download a sample memory image for investigation?
```bash
wget https://www.dropbox.com/s/x41f2lyhlrixts6/memdumpWin7.mem
```

Other samples are [here](https://github.com/volatilityfoundation/volatility/wiki/Memory-Samples)

## Understand the Suspect and Accounts
### How to identify the image profile
```bash
vol.py -f memdumpWin7.mem imageinfo
```

### Who was using the device?
```bash
vol.py -f memdumpWin7.mem --profile=Win7SP1x86_23418 printkey -K "Volatile Environment"
```

### Who is associated with the device?
```bash
vol.py -f memdumpWin7.mem --profile=Win7SP1x86_23418 printkey -K "Microsoft\Windows NT\CurrentVersion\ProfileList"
```

### Who has a particiular SID?
```bash
vol.py -f memdumpWin7.mem --profile=Win7SP1x86_23418 printkey -K "Microsoft\Windows NT\CurrentVersion\ProfileList\S-1-5-21-1716914095-909560446-1177810406-1002"
```
### Who is the default logon user?
```bash
vol.py -f memdumpWin7.mem --profile=Win7SP1x86_23418 printkey -K "Microsoft\Windows NT\CurrentVersion\Winlogon"
```
## Understand the Suspect's PC
### How to find suspect's CPU
```bash
vol.py -f memdumpWin7.mem --profile=Win7SP1x86_23418 printkey -K "DESCRIPTION\System\CentralProcessor\0"
```
### How to find additional PC sysinfo:
```bash
vol.py -f memdumpWin7.mem --profile=Win7SP1x86_23418 printkey -K "DESCRIPTION\System"
```
### Enumerated devices
```bash
vol.py -f memdumpWin7.mem --profile=Win7SP1x86_23418 printkey -K "ControlSet001\Enum"
```
### How to find devices connected to PCI
```bash
vol.py -f memdumpWin7.mem --profile=Win7SP1x86_23418 printkey -K "ControlSet001\Enum\PCI"
```
### How to find Windows computer name
```bash
vol.py -f memdumpWin7.mem --profile=Win7SP1x86_23418 printkey -K "ControlSet001\Control\ComputerName\ComputerName"
```
## Network Forensics
### Look at computer's netstat
```bash
vol.py -f memdumpWin7.mem --profile=Win7SP1x86_23418 netscan | grep TCPv4
```
## Look at computer's processes
```bash
vol.py -f memdumpWin7.mem --profile=Win7SP1x86_23418 pslist
```
## Look at computer's processes (filter just for PID 4)
```bash
vol.py -f memdumpWin7.mem --profile=Win7SP1x86_23418 pslist -p 4
```
## Investigate Command History
### Did the suspect use commands to copy files?
```bash
vol.py -f memdumpWin7.mem --profile=Win7SP1x86_23418 cmdscan
```
### Alternate Command History
```bash
vol.py -f memdumpWin7.mem --profile=Win7SP1x86_23418 consoles
```
## Investigate Suspect's USB
### List all device interfaces
```bash
vol.py -f memdumpWin7.mem --profile=Win7SP1x86_23418 printkey -K "ControlSet001\Control\DeviceClasses"
```
### List all device interfaces
```bash
vol.py -f memdumpWin7.mem --profile=Win7SP1x86_23418 printkey -K "ControlSet001\Control\DeviceClasses"
```
### List all disk interfaces
```bash
vol.py -f memdumpWin7.mem --profile=Win7SP1x86_23418 printkey -K "ControlSet001\Control\DeviceClasses\{53f56307-b6bf-11d0-94f2-00a0c91efb8b}"
```
### Access DeviceClass\Disk Interface\USB Interface
```bash
vol.py -f memdumpWin7.mem --profile=Win7SP1x86_23418 printkey -K "ControlSet001\Control\DeviceClasses\{53f56307-b6bf-11d0-94f2-00a0c91efb8b}\##?#USBSTOR#Disk&Ven_General&Prod_UDisk&Rev_5.00#6&1bec0f48&0&_&0#{53f56307-b6bf-11d0-94f2-00a0c91efb8b}"
```
### Verify USBSTOR subkey and timestamp
```bash
vol.py -f memdumpWin7.mem --profile=Win7SP1x86_23418 printkey -K "ControlSet001\Enum\USBSTOR"
```
### Which drive letter was the USB assigned to?
```bash
vol.py -f memdumpWin7.mem --profile=Win7SP1x86_23418 printkey -K "MountedDevices"
```
### Who mounted the volume F?
```bash
vol.py -f memdumpWin7.mem --profile=Win7SP1x86_23418 printkey -K "Software\Microsoft\Windows NT\CurrentVersion\Explorer\MountPoints2"
```
### When was the USB last attached to the PC?
```bash
vol.py -f memdumpWin7.mem --profile=Win7SP1x86_23418 printkey -K "Software\Microsoft\Windows\CurrentVersion\Explorer\MountPoints2\F"
```
## Investigate Internet History
### Did the suspect use Internet Explorer?
```bash
vol.py -f memdumpWin7.mem --profile=Win7SP1x86_23418 iehistory
```
## Investigate File Explorer History
### Are there any folder access activites on 2019-01-06?
```bash
vol.py -f memdumpWin7.mem --profile=Win7SP1x86_23418 shellbags | grep  "Last updated: 2019-01-06" | sort
```
### What are folder access activities on 2019-01-06?
```bash
vol.py -f memdumpWin7.mem --profile=Win7SP1x86_23418 shellbags --output=html --output-file=shellbags.html
```
## Assemble the timeline
