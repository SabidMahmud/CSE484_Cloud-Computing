> Written with [StackEdit](https://stackedit.io/).
### Enabling Hardware Virtualization Support
Our virtual machine manager KVM requires a CPU with virtualization extension. These extensions are called intel VT or AMD-V. To check the virtualization support, we need to run the following command.
```
$ egrep '^flags.*(vmx|svm)' /proc/cpuinfo
```
if this command results nothing then, the system does not support the relevant virtualization extension. But QUEMU or KVM could be executable in this machine.
![Screenshot from 2023-10-14 15-00-39.png](https://github.com/SabidMahmud/CSE484_Cloud-Computing/blob/main/Everything%20is%20virtual%3F/Screenshot%20from%202023-10-14%2015-00-39.png?raw=true)
# Installing virtualization software
We can install the virtualization tools via command line using the Virtualization Package Group. To view the package, we can run the folloing command:
```
$ dnf groupinfo virtualization
```
This will show the mandatory, defauld and the optional packages required for the virtualization.
![dnf groupinfo virtualization.png](https://github.com/SabidMahmud/CSE484_Cloud-Computing/blob/main/Everything%20is%20virtual%3F/dnf%20groupinfo%20virtualization.png?raw=true)
1. We need to run un the following command to install the mandatory and default packages in the virtualization group:
	```
	$ sudo dnf install @virtualization
	```
	Alternatively, to install the mandatory, default, and optional package, run:
	```
	$ sudo dnf group install --with-optional virtualization
	```
2. After installing the packages, we need to start the `libvirtd` servince:
	```
	$ sudo systemctl start libvirtd
	```
	To start the service on boot, we can run the following command:
	```
	$ sudo systemctl enable libvirtd
	```
3. To verify that the KVM kernel modules are properly loaded:
		```
		lsmod | grep kvm
		```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE2OTc2ODg0NTgsLTIxMTg4NDYwMjgsLT
E0NjI1NjYyNjddfQ==
-->