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
	![systemctl libvirtd.png](https://github.com/SabidMahmud/CSE484_Cloud-Computing/blob/main/Everything%20is%20virtual%3F/systemctl%20libvirtd.png?raw=true)

3. To verify that the KVM kernel modules are properly loaded:
	```
	$ lsmod | grep kvm
	```
	![grepkvm.png](https://github.com/SabidMahmud/CSE484_Cloud-Computing/blob/main/Everything%20is%20virtual%3F/grepkvm.png?raw=true)

# Creating Virtual machines using `virt-install`
Though kvm has a GUI to install virtual machine, the installation can be started on the command line using the `virt-install` command on terminal.
### Creating a guest instance with virt install command
`virt-install` is a command line based tool for creating virtualized guest instance. 
To use the virt-install command, we should first download an iso of the targeted  operating sytem. This time we are selecting the ubuntu server 22.04.
#### Planning VM Resources
We need to adjust the ram, vcpus, and disk size parameters according to the resources we have available. Finally, lets run the virt-install command using the following format (adjusting parameters as needed):
```
$ sudo virt-install \ 
--name mahmud-22301172_ubuntu_server \
--os-variant ubuntu20.04 \ 
--vcpus 2 \
--ram 2048 \
--location ./Downloads/ubuntu-22.04.3-live-server-amd64.iso \
--network bridge=virbr0,model=virtio \
--graphics none \ 
--extra-args='console=ttyS0,115200n8 serial'
```



<!--stackedit_data:
eyJoaXN0b3J5IjpbMTEzNDI4MDU5NCwxODI4NDQ5ODU3LDE0ND
c2NzE2MjEsMjA2Mzk2NDg4OSwtMTY3NDI3MTU2OCwtMjExODg0
NjAyOCwtMTQ2MjU2NjI2N119
-->