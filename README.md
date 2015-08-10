qeo-vm
==========

Automated provisioning and configuration of an Ubuntu VM containing the Android development environment, including Android ADT Bundle with SDK, Eclipse &amp; and QEO SDK using the Vagrant DevOps tool with Chef and shell-scripts.

This automated VM installation and configuration uses the excellent DevOps tool, [Vagrant](http://downloads.vagrantup.com/) which works with both VirtualBox (free) and VMware Fusion &amp; Workstation (paid plug-in) in addition to several [Community Chef Cookbooks](http://community.opscode.com/cookbooks).

Please feel free to contribute improvements and enhancements to the provisioning code & reporting issues or questions.  The goal is to improve this Android VM project with community support.

Currently, it will provision an Android VM for development with the following specifications,

- Ubuntu Trusty 64 VM
	- Memory size: 2048 MB
	- 2 vCPU
- Ubuntu Unity Desktop as the UI launched at startup (see the provision.sh section "Install a desktop for the Android graphical tooling" for other options)
- [Android ADT Bundle 20140702 (including Eclipse)](https://dl.google.com/android/adt/adt-bundle-linux-x86_64-20140702.zip)
- [Qeo SDK 1.1.0](https://dl.bintray.com/elendrim/generic/qeo-sdk-1.1.0-20150529.113620-83.zip)

## Clone the Android VM Code Repository

1. Create a working directory to use for the Android VM project in, e.g. ~/qeo-vm 

	
2. Download or clone the project repository into the newly created directory on your local machine from one of the following sources,

	Visit the Qeo-VM repository on GitHub,
		
	[https://github.com/elendrim/qeo-vm](https://github.com/elendrim/qeo-vm)
		
	Clone the Qeo-VM repository directly from GitHub,
	
	[https://github.com/elendrim/qeo-vm.git](https://github.com/elendrim/qeo-vm.git)
	
	Download the Qeo-VM repository as a zip file,
	
	[https://github.com/elendrim/qeo-vm/archive/master.zip](https://github.com/elendrim/qeo-vm/archive/master.zip)


## Install VirtualBox

 [VirtualBox](https://www.virtualbox.org/wiki/Downloads) (Free)
 You need to install the "extension pack" depending on your version of VirtualBox, to be able to use USB devices.

## Install Vagrant

1. Download and install the latest version of Vagrant for your OS from  [https://www.vagrantup.com/downloads.html](vagrantup.com/)
2. Install vagrant plugins : 
		$ vagrant plugin install vagrant-berkshelf
		$ vagrant plugin install vagrant-omnibus

## Install the Qeo VM

_Note: All the software needed is automatically downloaded as it is needed.  Several of the downloads are somewhat large.  Patience is a virtue while the automated installation is running._

1. From the newly created working directory, e.g.

		$ cd ~/qeo-vm 

2. Run the following to start Vagrant and kick-off the process to build an Android VM,
	
	For VirtualBox,
	
		$ vagrant up

	_Note: As the Android VM build runs you will see various types of screen output from Vagrant, Chef and Shell scripts -- some of the dependency downloads and compilations require a bit of time.  Again, Patience is a virtue._
3. Once the Android VM build provisioning process is complete, run the following to login via SSH, and configure your keyboard.

		$ vagrant ssh
		$ sudo dpkg-reconfigure keyboard-configuration

4. The Ubuntu Unity desktop UI is set to automatically launch on `vagrant up`, login using the credentials,
	- Username: vagrant
	- Password: vagrant
5. The Android development environment directories with eclipse, sdk are located in the directory `/usr/local/android/`.
6. The QEO development environment directories is located in the directory `/usr/local/QeoSDK/`.
7. The VM has an internal `/vagrant` directory which maps to the directory created previously (i.e. the one from which you are running the Android VM on your local machine), e.g. `~/qeo-vm` maps to the internal VM directory `/vagrant`.

	_The net effect is that anything you drop in your local working directory, e.g. ~/qeo-vm, can be accessed from within the VM by opening the directory "/vagrant" and vice-versa_


## Manually Configure the Android VM in the Virtualization Provider
	
To connect an Android device you must manually setup a USB connection mapping for your Android device to the new VM	
	
For example, if using VirtualBox perform the following steps,

1. Plug your android device hardware into the computers USB port
2. Open the 'Oracle VM VirtualBox'
3. Run the QeoSdkVM
4. Click on the USB devices (on the bottom of the window) and select your device.
5. Plug the device into the USB port and verify that it appears when you run `lsusb` from the command line
6. Your device should appear when running `lsusb` enabling you to use Android `adb`, e.g.

		$ adb devices
			...
			List of devices attached
			007jbmi6          device

		$ adb shell
			i.e. to log into the device (be sure to enable USB debugging on the device)

_Note: Additionally you may want to change various settings in the Virtualization Provider to size memory and vCPUs allocated to the Android VM_
_Note: To open the terminal from desktop, use ctrl-alt-T for PC or control-option-T for Mac

### Vagrant Basics &amp; Workflow

		$ vagrant up # start the QeoVM
		$ vagrant provision # Download and reinstall all ( Eclipse + Java + Android + Qeo .. )
		$ vagrant ssh # connect to the QeoVM in command line
		$ vagrant status # status of QeoVM
		$ vagrant halt  # To shutdown the VM
		$ vagrant reload # stop and restart the QeoVM
    	$ vagrant --help


### References

1. [Vagrant v2 documentation](http://docs.vagrantup.com/v2/getting-started/)
2. [http://www.vagrantbox.es/](http://www.vagrantbox.es/)
3. [Chef Cookbooks](http://community.opscode.com/cookbooks)


