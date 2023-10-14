


> Written with [StackEdit](https://stackedit.io/).
### Enabling Hardware Virtualization Support
Our virtual machine manager KVM requires a CPU with virtualization extension. These extensions are called intel VT or AMD-V. To check the virtualization support, we need to run the following command.
```
$ egrep '^flags.*(vmx|svm)' /proc/cpuinfo
```
if this command results nothing then, the system does not support the relevant virtualization extension. But QUEMU or KVM could be executable in this machine.

 - List item

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTA5NzM0NzgxNl19
-->