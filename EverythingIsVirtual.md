> Written with [StackEdit](https://stackedit.io/).
### Enabling Hardware Virtualization Support
Our virtual machine manager KVM requires a CPU with virtualization extension. These extensions are called intel VT or AMD-V. To check the virtualization support, we need to run the following command.
```
$ egrep '^flags.*(vmx|svm)' /proc/cpuinfo
```
if this command results nothing then, the system does not support the relevant virtualization extension. But QUEMU or KVM could be executable in this machine.
![Screenshot from 2023-10-14 15-00-39.png](https://github.com/SabidMahmud/CSE484_Cloud-Computing/blob/main/Everything%20is%20virtual%3F/Screenshot%20from%202023-10-14%2015-00-39.png?raw=true)
# Installing virtualization software
We can install the virtualization tools via command line using the Virtualization Package Group. To view the packag

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTUyNDEwNzA0MSwtMTQ2MjU2NjI2N119
-->