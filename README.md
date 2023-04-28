# Home Test

Please pick one existed Debian package from the Ubuntu archive and implement the following changes:

1. Download the source code of that Debian package.
2. Add an executable bash script called “testing.sh”
   • When executing the testing.sh will echo “this is a test from Ing. Alberto Petrucci ” on the standard error (STDERR).
4. Please echo the string “this is a test from Ing. Alberto Petrucci” during the Debian package installation time.
5. Push all of your change to a public git repository (e.g. launchpad git, Github or GitLab)
6. dput the source change to your ppa on the launchpad.
7. Install your package from your ppa.
8. Paste the message printed during the installation via standard out (STDOUT)
   • The string “this is a test from Ing. Alberto Petrucci” is expected.
8. Paste the executing result of  `dpkg -S testing.sh; testing.sh`
   • The string “this is a test from Ing. Alberto Petrucci” is expected.
9. Capture the issues and tasks that you experienced for this exercise. Please submit the following result in a PDF file
   a. url of the git repo
   b. url of the launchpad ppa
   c. Please write down the steps that can be a reference guide for the next engineer who is new to this task.
   d. Include the output of the installation and testing.sh This is an example of  executing result for your reference: pastebin.ubuntu.com/p/hZ4sH647Jt/

## Downloading the source code
I choosed the latest **htop** as example package

Downloading its source code
```
wget "http://archive.ubuntu.com/ubuntu/pool/main/h/htop/htop_3.2.2.orig.tar.gz"
```

Extracting it
```
tar xf htop_3.2.2.orig.tar.gz
cd htop-3.2.2
```