# toolkit-information

## [Indri](https://www.lemurproject.org/indri/)


### Installation
1) Download Indri from [Sourceforge](https://sourceforge.net/projects/lemur/files/lemur/indri-5.11/indri-5.11.tar.gz/download)

2) In the directory of the download: `gunzip indri-5.11.tar.gz`, followed by `tar -xvf indri-5.11.tar`

4) Create a directory where Indri should be installed, e.g. `mkdir /myhome/Indri`

5) Enter Indri's source directory (the unzipped/untarred one): `cd /myhome/indri-5.11`

6) Run `./configure --prefix=/myhome/Indri/` in Indri's source directory to generate the Makefiles. The prefix tells Indri in which directory to install Indri in.

7) Run `make`, followed by `make install` followed by `make clean` in Indri's source directory

If an error such as `zlib.h not found` occurs, check if zlib is installed. If not, install it via `sudo apt-get install zlib1g-dev`.

8) 
