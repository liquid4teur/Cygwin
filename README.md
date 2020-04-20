# Quick Presentation

Cygwin is a collection of open sources tools, developped by Cygnus Solutions. It allows the possibility to emulate a Unix system running on Windows machines.

In our case, we wanted to use the OpenSSH tool (and other complementary tools) in order to communicate between two Windows machines through the SSH protocol and quickly automate the deployment of the AIT tool.

In order to answer this purpose, we decided to use Cygwin by installing it with the needed packages (OpenSSH, Tar, Unzip and more). Yet, the goal was to automate as far as possible the installation of Cygwin and the activation of the SSH service without any "human" intervention.

Consequently, we implemented a batch script that :
- Download the cygwin executable (for 64-bit),
- Install Cygwin with its core packages (C:\cygwin),
- Install additional packages:
	- openssh,
	- openssl,
	- rebase,
	- p11-kit,
	- p11-kit-trust,
	- run,
	- sed,
	- tar,
	- terminfo,
	- terminfo-extra,
	- tzcode,
	- tzdata,
	- unzip,
	- util-linux,
	- vim-minimal,
	- which,
	- xz,
	- zip,
	- zlib0.
- Create desktop shortcut,
- Install & activate the cygsshd service for SSH communication


# How it works

 1. You have to run the command prompt as administrator.
 2. Run the cygwin_deployment.bat script: the installation will ask you to press any key and then it will run in silent mode, an interactive window will show up but you have nothing to do.
 3. A final message of installation tells you that the installation is complete. Cygwin is installed and the SSH (cygsshd) service is running.

# How to connect 2 windows machines through SSH

 - First, ensure that on both windows machine, Cygwin is installed & "CYGWIN cygsshd" process is running .
 - Second, open a Cygwin terminal as administrator (on any windows machine, not necessarily on both).
 - Third, to establish a SSH connection, follow the commands:
>ssh HOSTNAME+username@HOSTNAME (for example "ssh BTIN01DSY+adminlocal@BTIN01DSY")

When typing this command, the command prompt will ask you the password of the username on the target machine.
Once the password entered, the SSH connection should be working.

## If you want to go further 

Optionally, if you want to simplify the way you log on the targeted machine through SSH, you have to modify the "passwd" file.
On the targeted machine (in our case BTIN01DSY), open the passwd file (located in the /etc folder):

>vi /etc/passwd  

In this case, we want to log as "adminlocal", so you have to go to the line where it's written "BTIN01DSY+adminlocal" and modify it in order to let it be "adminlocal".
Now instead of logging with the command:

>ssh HOSTNAME+username@HOSTNAME (for example "ssh BTIN01DSY+adminlocal@BTIN01DSY")

You can log on with a more simplified command:

>ssh username@HOSTNAME (for example "ssh adminlocal@BTIN01DSY")

# If you need to modify installation

For further developments or modifications, you can modify the cygwin_deployment.bat script. If you need for example:
- To modify the installation path: at line 5, you can modify the "CYGWIN_BASE" variable and set it depending your need.
- To add more packages: at line 59 and add or delete the packages needed.

# If you need to uninstall

1. You have to run the Cygwin terminal as administrator,
2. Verify the process that are running by doing:
>cygrunsrv -L

3. Once you identified the process that are still running, you have stop each process by doing:
>cygrunsrv --stop *nameoftheprocess*

4. Then, you have to remove each process by doing:
>cygrunsrv --remove *nameoftheprocess*

5. Once, all the process stopped and removed, you have to restart your machine. Then, you can remove the desktop shortcut and remove the cygwin folder (C:\cygwin by default in this script).
