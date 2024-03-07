# hello
This package is for homework demostation.

##1.1 Download source

edit /etc/apt/sources.list with sudo append below context

```
deb-src http://archive.ubuntu.com/ubuntu/ focal main restricted
```

Then you can download package (We chose hello as an example)
```
apt-get source hello

apt-get install devscripts 

apt-get install debhelper
```

##1.2 Modify file for target output
Create patches folder on debian
```
mkdir hello-2.10/debian/patches
cd hello-2.10/debian
```

Create patch file and add related files for making patch
```
quilt new homework-adler.patch
quilt add test.sh
quilt add rules
```
Append below context in test.sh
```shel
echo "this is a test from Adler Yang"
```

Append below context in rules
```makefile
override_dh_auto_install:
       		dh_auto_install
       		install -m 755 debian/test.sh debian/hello/usr/bin/test.sh
```
Save patch
```
quilt refresh
```

Update changelog with dch
```
dch -i
```
##1.3 Build and install package
Build package generat hello_2.10-2ubuntu2_amd64.deb 
```
cd ../hello-2.10
dpkg-buildpackage -rfakeroot -uc -b
```

Install dependent packages
```
sudo apt-get build-dep hello
```

Install hello
```
cd ..
sudo dpkg -i hello_2.10-2ubuntu2_amd64.deb
```

And then check test.sh belongs to which package and execute it.
```
dpkg -S testing.sh; testing.sh
```

##2.1 PPA preparation (Ubuntu client site operation)
Create SSH keys
```
ssh-keygen -t rsa
cat ~/.ssh/id_rsa.pub
```

Create OpenPGP keys
```
gpg --gen-key
```

List gpg keys find <KEY_ID>
```
gpg --list-keys
```

Send gpg keys to keyserver
```
gpg --keyserver keyserver.ubuntu.com --send-keys <KEY_ID>
```

##2.2 PPA preparation (lanchpad website operation)
Register an account
Add an SSH keys
Import an OpenPGP keys
Create PPA



##2.3 Upload source to PPA
Update dsc and changes file
```
dpkg-buildpackage -S
```

```
debsign -k<KEY_ID> hello_2.10-2ubuntu3_amd64.changes
dput ppa:adlery/ppa hello_2.10-2ubuntu3_amd64.changes
```

##2.4 Download source from PPA
Append PPA
```
sudo add-apt-repository ppa:adlery/ppa
sudo apt-get update
```

Install package
```
sudo apt-get install <package-name>
```

##3.1 Git
Create a git project, prepare ssh-key
```
git clone git@github.com:nillsonyang/hello.git
```

Upload source to git server
