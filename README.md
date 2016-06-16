# WIMP BUILD VM  #  
*(Windows-IIS-MySQL-PHP Virtual Machine)*

## Purpose ##
The purpose of this build is to give users the ability to have a local environment replicating our development environment.

The user would do all development work on the virtual machine and then once stable move it to a staging environment for more 
rigorous testing.

Included in the build:
- Windows Server 2012
- IIS 8
- MySQL 5.5
- PHP 5.4
- PHPMyAdmin

## Requirements ##
- [Vagrant](https://www.vagrantup.com/downloads.html) 
  - For the environment
- [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
  - For the virtual machine needed for vagrant to work
  
## Contents and Descriptions ##
- after_build
	- These are configurations/credentials and files that will be needed for after the machine is up and running
- resources
	- These are programs that will be installed when the machine is initially booted up
- scripts
	- These are powershell / cmd modules that will help automate the installation process initially. ** \*DO NOT TOUCH THESE\* **
- README.md
	- This file
- README.txt
	- Step by Step instructions to building up the virtual environment
- Vagrantfile
	- The file that creates the virtual environment ** \*Do Not Modify\* **

\* After creating the machine for the first time, a new directory will appear: *.vagrant*. This directory houses the virtual machines infrastructure. 

## Usage ##
Download the package either through git or download as a zip.  
  
There are step by step instructions on how to replicate the environment inside the wimp_build directory in a file named **README.txt**.
Following these instructions are important. 

Do the actual creating of the environment at a location with decent bandwidth because when the environment starts up, it will 
download a virtual box, which will be a decent sized download. The updates and other installs that happen after the machine is up 
and running will require some bandwidth as well.

Included in the instructions are directions to modify the hosts file. IF you're unable to accomplish this, you can always use the IP address
included in the directions.

To start up the machine for the first time, open the command line interface (cmd, or gitbash) and cd to the base directory. Once there
type ```vagrant up```

Give it a few minutes while the base box is downloaded and installed. After the base box is installed, it will boot up and vagrant will run the 
provision scripts needed to automate the install process.

#### Notes ####
- The package can be stored anywhere, I recommend keeping it on the desktop just because it's an easy location to remember.
- If something should go terribly wrong while building up the environment, you can use the ```vagrant destroy``` command to 
wipe the box and start over. **If for some reason this happens, you will not have to re-download the base box.**