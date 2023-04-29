# Home Test

## Git url
[https://github.com/capriele/hometest](https://github.com/capriele/hometest)

## Launchpad ppa url
[https://launchpad.net/~capriele/+archive/ubuntu/test](https://launchpad.net/~capriele/+archive/ubuntu/test)


## The task
Please pick one existed Debian package from the Ubuntu archive and implement the following changes:

1. Download the source code of that Debian package.
2. Add an executable bash script called “testing.sh”

   • When executing the testing.sh will echo “this is a test from Ing. Alberto Petrucci ” on the standard error (STDERR).
3. Please echo the string “this is a test from Ing. Alberto Petrucci” during the Debian package installation time.
4. Push all of your change to a public git repository (e.g. launchpad git, Github or GitLab)
5. dput the source change to your ppa on the launchpad.
6. Install your package from your ppa.
7. Paste the message printed during the installation via standard out (STDOUT)

   • The string “this is a test from Ing. Alberto Petrucci” is expected.
8. Paste the executing result of  `dpkg -S testing.sh; testing.sh`

   • The string “this is a test from Ing. Alberto Petrucci” is expected.
9. Capture the issues and tasks that you experienced for this exercise. Please submit the following result in a PDF file

   a. url of the git repo
   
   b. url of the launchpad ppa
   
   c. Please write down the steps that can be a reference guide for the next engineer who is new to this task.
   
   d. Include the output of the installation and testing.sh This is an example of  executing result for your reference: pastebin.ubuntu.com/p/hZ4sH647Jt/

### Step 1: Downloading the source code
I chose the latest **htop** as an example package

Downloading its source code
```bash
wget "http://archive.ubuntu.com/ubuntu/pool/main/h/htop/htop_3.2.2.orig.tar.gz"
```

Extracting it
```bash
tar xf htop_3.2.2.orig.tar.gz
cd htop-3.2.2
```

#### Build the package
Before creating the deb distribution package I built the htop sources to check all the needed dependencies.
I need to install the following dependencies.

#### Install missing dependencies
```bash
sudo apt-get install gcc make wget tar libncursesw5 libcunit1-ncurses libncursesw5-dev python automake
```

and finally, I can:

#### Generate configure script
```bash
bash autogen.sh
```

#### Configuring the project
```bash
./configure
```

#### Compiling the project
```bash
make
```

### Step 2: create the testing.sh script
I've created the custom **testing.sh** script to complete the assigned task (print a simple string to the stderr). And now I start to build the environment to create a custom package.

```bash
#!/bin/bash

echoerr() { 
    echo "$@" 1>&2;
}
echoerr "this is a test from Ing. Alberto Petrucci "
```

#### Generating the signature
To create the .deb package I first generate a *gpg signature* to sign the final package.
```bash
gpg --full-generate-key
```

after that I followed all the needed steps to add the generated key in launchpad as described [here](https://launchpad.net/+help-registry/import-pgp-key.html)
```bash
gpg --armor --export 0FE5852BDDC32A5C
```

#### Sending the gpg key to ubuntu server
```bash
gpg --send-keys 0FE5852BDDC32A5C
```

#### Copy the key to launchpad
```bash
capriele@xubuntu:~/Desktop/canonical$ gpg --fingerprint
/home/capriele/.gnupg/pubring.kbx
---------------------------------
pub   rsa3072 2023-04-28 [SC]
      68E6 7785 21E3 9F86 8F25  3FAC 0FE5 852B DDC3 2A5C
uid           [ultimate] Alberto Petrucci (Test Key) <petrucci.alberto@gmail.com>
sub   rsa3072 2023-04-28 [E]
```

#### Decript email message
```
-----BEGIN PGP MESSAGE-----

hQGMA7+d/adDbyaGAQv+OPg59tOqbydlh/dtEg6vMR90yGF5OVXY5YJoUNuMm+ze
nLHl7yGyL992aMcBgqk33RNwVUpN+QhJotJd5nuNw/H8ilRn4u/vGstr8zrVuWCj
rkoUsuElJUbuN+DBDU4whJUlamBLyvdyGdNCsyGoYaVusf7i6ToqeQq/zj42jVk8
+NvnYxSkmuU5zHsdcC1u+JO7UvYLW/IEaR+qUP86tnY3dD464AOfT8NEm4bfkHUO
Y8Sng87EotSq4t8pu1aalVi4ssw0LYx4g/+A1fNtXXzgdVaU3llh1R3kVOhWLE7m
yLk6KslHUXv4kSjTzGsrc5VvPlROuQ5Tdw1H55muYaMkZdLWIuLQHxVyHvX4Uh4/
gILsEkEg/IHQuS1MSOrYzLURD2uf2lB+Twzj54TqePSQFnqMS0DjcWKFf7a+5haa
QSa02iXvGyWFMjZQV1ZNjrJTWU6yRW8yN5g+4waDBCZn5t5rl/3TknXPLx5i83o5
mX5FQOQs6MKEs1Q3/Dke0sCxAYRm1mfj+ywDCHPlWMbafCZcWIE3CPfTORGi4K9L
i6RBA8bLOiIc5p4L2J9B+ryiwHWc7wPsMr0zg00ZCP6LIEhDNLJ3BD09QQMS+oeP
gmqW9kkLHvW6ED9UF9XCMYoJunfhQDpED6C9F1cLiCA8Y/DWWbdH/LU90yVkig5q
Ro9rlLlQjCS4GC3WdrG8ERd8x31Tn1Z1pPcedxdYDzF+HjusjtiGgVOfY9CoeITF
kDWElWPdNxPoXj1MIWbAZc6oRCXw8FzjQpzBDa7MbAldEUdqD7kXC2siCuHmZeC1
WQQkwCrUzn3bKe1zojl/DRs3U8DJIdeAn3CPJ9CaWuhv3W8zBTphg0bSJtdfwOyE
9KCl7bUa6Av7B8evEcrH/UQvTVZ5IpVIuSB7P5agYB2W6VjM3fOjuWDX+Oo8ZLzD
Wq5ezD9ENoL6YGwnQtZE1zf9rlmOIpaq7uOAl1ZaVzVYA015buAXBUjDFP683CnT
AoOO
=OZ6A
-----END PGP MESSAGE-----
```

it simply asks to click on the following link:
https://launchpad.net/token/C6NZRjsJ6xDWPNq6HkGb


#### Creating the .deb package
To create the .deb package I first installed the following dependencies
```bash
sudo apt-get install dh-make bzr-builddeb
```

after that, I set up my data
```bash
brz whoami "Alberto Petrucci (Test Key) <petrucci.alberto@gmail.com>"

cat >>~/.bashrc <<EOF
DEBEMAIL="petrucci.alberto@gmail.com"
DEBFULLNAME="Alberto Petrucci"
DEBSIGN_KEYID="0FE5852BDDC32A5C"
export DEBEMAIL DEBFULLNAME DEBSIGN_KEYID
EOF
```

and finally, I reloaded the .bashrc file.
```bash
. ~/.bashrc
```

From here I started creating a custom .deb package for htop sources (I'm supposing to be in *deb_generation* folder):
```bash showLineNumbers
bzr dh-make htop 3.2.2 ../htop_3.2.2.orig.tar.gz
rm -r ./htop/debian
cp -a ../debian/. ./htop/debian/
cd htop
bzr add .
bzr commit -m "Initial commit of Debian packaging."
bzr builddeb -- -S -us -uc
debsign -k 0FE5852BDDC32A5C htop_3.2.2-2ubuntu1_source.changes
gpg --verify htop_3.2.2-2ubuntu1_source.changes
```

### Step 3: execute testing.sh during the installation
To complete this step I simply configured the **debian/postinst** script to run the **testing.sh** file just after the .deb installation

#### Testing the .deb locally
Just a little note: to generate the .deb package (only for local testing) I changed the build command with the following one:
```bash
bzr builddeb -- -us -uc
```
the only thing to note is that now we do not have the **-S** (source) parameter.

```bash
cd ..
sudo dpkg --install htop_3.2.2-2ubuntu1_source.deb
```
Output:
```bash
Selecting previously unselected package htop.
(Reading database ... 157539 files and directories currently installed.)
Preparing to unpack htop_3.2.2-2ubuntu1_source.deb ...
Unpacking htop (3.2.2-1ubuntu1) ...
Setting up htop (3.2.2-1ubuntu1) ...
this is a test from Ing. Alberto Petrucci 
Processing triggers for man-db (2.9.1-1) ...
```

And 
```bash
dpkg -S testing.sh; testing.sh
```
Output:
```bash
htop: /usr/local/bin/testing.sh
this is a test from Ing. Alberto Petrucci 
```

And finally, I removed the packages to proceed with the next step
```bash
sudo apt --purge remove htop
```

### Step 4: upload all on a git repository
I've uploaded all the sources and changes at the following url: [https://github.com/capriele/hometest.git](https://github.com/capriele/hometest.git)

### Step 5: dput the source change to launchpad
This step was a little bit tricky because to successfully complete the upload of the generated Debian package you have two respect at least the following points:
1. Having a registered OpenPGP in launchpad (done in the previous step)
2. Having a package that does not have any errors/warnings according to Debian policies.

To complete the second step I executed the following command to get the errors/warning reports
#### Check deb error/warnings
```bash
dput -ol ppa:capriele/ppa htop_3.2.2-2ubuntu1_amd64.changes
```

this command returns to me different kinds of errors:
- error: installation path (/usr/local/bin): to solve I changed the configure step in *debian/rules* file
- error: the dependencies version: to solve I updated the *debian/control* file with all the needed versions
- warning: naming convention
the most important thing is that any case is really well described and so it was not so difficult to solve the errors and the warnings.

#### Upload the package
Finally, I can now successfully upload the package on Launchpad (on my [personal ppa](https://launchpad.net/~capriele/+archive/ubuntu/test)) simply using:
```bash
dput ppa:capriele/test htop_3.2.2-2ubuntu1_sources.changes 
```
it returns the following output
```bash
Successfully uploaded htop_3.2.2-2ubuntu1.dsc to ppa.launchpad.net for ppa.
Successfully uploaded htop_3.2.2.orig.tar.gz to ppa.launchpad.net for ppa.
Successfully uploaded htop_3.2.2-2ubuntu1.debian.tar.xz to ppa.launchpad.net for ppa.
Successfully uploaded htop_3.2.2-2ubuntu1_source.buildinfo to ppa.launchpad.net for ppa.
Successfully uploaded htop_3.2.2-2ubuntu1_source.changes to ppa.launchpad.net for ppa.
```
after a while I received an email confirming that the packet was accepted.

### Step 6: Add my ppa to local apt repositories
I simply executed
```bash
sudo apt-key adv --recv-keys 0FE5852BDDC32A5C
sudo add-apt-repository ppa:capriele/test
sudo apt update
sudo apt install htop
```

### Step 7: the installation output
```bash
capriele@xubuntu:~/Desktop/canonical$ sudo add-apt-repository ppa:capriele/test
 
 More info: https://launchpad.net/~capriele/+archive/ubuntu/test
Press [ENTER] to continue or Ctrl-c to cancel adding it.

Hit:1 http://it.archive.ubuntu.com/ubuntu focal InRelease
Hit:2 http://it.archive.ubuntu.com/ubuntu focal-updates InRelease                                                 
Hit:3 http://it.archive.ubuntu.com/ubuntu focal-backports InRelease                                               
Hit:4 http://ppa.launchpad.net/capriele/test/ubuntu focal InRelease                                               
Hit:5 https://packages.microsoft.com/repos/code stable InRelease    
Hit:6 http://security.ubuntu.com/ubuntu focal-security InRelease    
Reading package lists... Done

capriele@xubuntu:~/Desktop/canonical$ sudo apt update
Hit:1 http://it.archive.ubuntu.com/ubuntu focal InRelease
Hit:2 http://security.ubuntu.com/ubuntu focal-security InRelease
Hit:3 http://ppa.launchpad.net/capriele/test/ubuntu focal InRelease
Hit:4 http://it.archive.ubuntu.com/ubuntu focal-updates InRelease               
Hit:5 http://it.archive.ubuntu.com/ubuntu focal-backports InRelease             
Hit:6 https://packages.microsoft.com/repos/code stable InRelease
Reading package lists... Done
Building dependency tree       
Reading state information... Done
23 packages can be upgraded. Run 'apt list --upgradable' to see them.

capriele@xubuntu:~/Desktop/canonical$ sudo apt install htop
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following NEW packages will be installed:
  htop
0 upgraded, 1 newly installed, 0 to remove and 23 not upgraded.
Need to get 124 kB of archives.
After this operation, 360 kB of additional disk space will be used.
Get:1 http://ppa.launchpad.net/capriele/test/ubuntu focal/main amd64 htop amd64 3.2.2-2ubuntu1 [124 kB]
Fetched 124 kB in 0s (404 kB/s)
Selecting previously unselected package htop.
(Reading database ... 193088 files and directories currently installed.)
Preparing to unpack .../htop_3.2.2-2ubuntu1_amd64.deb ...
Unpacking htop (3.2.2-2ubuntu1) ...
Setting up htop (3.2.2-2ubuntu1) ...
this is a test from Ing. Alberto Petrucci 
Processing triggers for desktop-file-utils (0.24-1ubuntu3) ...
Processing triggers for mime-support (3.64ubuntu1) ...
Processing triggers for hicolor-icon-theme (0.17-2) ...
Processing triggers for gnome-menus (3.36.0-1ubuntu1) ...
Processing triggers for man-db (2.9.1-1) ...
```

### Step 8: `dpkg -S testing.sh; testing.sh` output
```bash
capriele@xubuntu:~/Desktop/canonical$ dpkg -S testing.sh; testing.sh
htop: /usr/bin/testing.sh
this is a test from Ing. Alberto Petrucci 
capriele@xubuntu:~/Desktop/canonical$ 
```
