# Volatility Notes: Digital Forensics
The Volatility framework is a free and open-source memory forensics tool. It is to monitor incident response and malware analysis. Volatility memory dump analysis tool was created by Aaron Walters in academic research while analyzing memory forensics. Volatility is a completely open collection of tools, written in Python language and released under the GNU General Public License. It is used for extraction of digital artifacts from volatile memory (RAM) samples and supports Linux, Windows and Mac OS.

Volatility memory forensics framework is intended to introduce extraction techniques and complexities associated with digital artifacts from volatile memory samples at runtime. Volatility memory extraction utility framework runs on any platform that supports Python. Volatility forensics open source software has 5.1K GitHub stars and 1.1k GitHub forks.


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

## How to identify the image profile
```bash
vol.py -f memdumpWin7.mem imageinfo
```

## Who was using the device?
```bash
vol.py -f memdumpWin7.mem --profile=Win7SP1x86_23418 printkey -K "Volatile Environment"
```

## Who is associated with the device?
```bash
vol.py -f memdumpWin7.mem --profile=Win7SP1x86_23418 printkey -K "Microsoft\Windows NT\CurrentVersion\ProfileList"
```

## Who has a particiular SID?
```bash
vol.py -f memdumpWin7.mem --profile=Win7SP1x86_23418 printkey -K "Microsoft\Windows NT\CurrentVersion\ProfileList\S-1-5-21-1716914095-909560446-1177810406-1002"
```

## How to find suspect's CPU
```bash
vol.py -f memdumpWin7.mem --profile=Win7SP1x86_23418 printkey -K "DESCRIPTION\System\CentralProcessor\0"
```
## How to find additional PC sysinfo:
```bash
vol.py -f memdumpWin7.mem --profile=Win7SP1x86_23418 printkey -K "DESCRIPTION\System"
```
