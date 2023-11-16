# Everything is virtual?

### Enabling Hardware Virtualization Support

Our virtual machine manager KVM requires a CPU with virtualization extension. These extensions are called intel VT or AMD-V. To check the virtualization support, we need to run the following command.

```bash
$ egrep '^flags.*(vmx|svm)' /proc/cpuinfo
```

if this command results nothing then, the system does not support the relevant virtualization extension. But QUEMU or KVM could be executable in this machine.

![Untitled](Everything%20is%20virtual%2007bff1bdac0047a58c1db9af4dd443eb/Untitled.png)

# Installing virtualization software

We can install the virtualization tools via command line using the Virtualization Package Group. To view the package, we can run the folloing command:

```bash
$ dnf groupinfo virtualization
```

This will show the mandatory, defauld and the optional packages required for the virtualization.

![Untitled](Everything%20is%20virtual%2007bff1bdac0047a58c1db9af4dd443eb/Untitled%201.png)

1. We need to run un the following command to install the mandatory and default packages in the virtualization group:
    
    ```bash
    $ sudo dnf install @virtualization
    ```
    
    Alternatively, to install the mandatory, default, and optional package, run:
    
    ```bash
    $ sudo dnf group install --with-optional virtualization
    ```
    
2. After installing the packages, we need to start the `libvirtd` servince:
    
    ```bash
    $ sudo systemctl start libvirtd
    ```
    
    To start the service on boot, we can run the following command:
    
    ```bash
    $ sudo systemctl enable libvirtd
    ```
    
    ![Untitled](Everything%20is%20virtual%2007bff1bdac0047a58c1db9af4dd443eb/Untitled%202.png)
    
3. To verify that the KVM kernel modules are properly loaded:
    
    ```bash
    $ lsmod | grep kvm
    ```
    
    ![Untitled](Everything%20is%20virtual%2007bff1bdac0047a58c1db9af4dd443eb/Untitled%203.png)
    

# Creating a Virtual Machine Using the GUI

To install the virtual machine, we need to download the target os iso image first. For this task, I am selecting the Ubuntu22.04.3 LTS server iso.

To download the iso, we need to run this command.

```bash
cd ~/Downloads
sudo wget https://releases.ubuntu.com/22.04.3/ubuntu-22.04.1-live-server-amd64.iso
```

This command will download the iso image in the `/home/sabid/Downloads/` directory.

### 1. Launching the Virtual Machine Manager

In GUI installation, we need to run the Virtual Machine Manager application, which will be easily found by searching in the applications menu or a simple `virt-manager` command from the terminal. After running the application we will see something like this:

![Untitled](Everything%20is%20virtual%2007bff1bdac0047a58c1db9af4dd443eb/Untitled%204.png)

### Connecting the Hypervisor

- First we need to click on the file option, and select the option Add Connection.
- Then, select "QEMU/KVM" as the hypervisor, and Click "Connect.”

![Untitled](Everything%20is%20virtual%2007bff1bdac0047a58c1db9af4dd443eb/Untitled%205.png)

### Create a New VM

After connecting to the hypervisor, in the file menu click the “New Virtual Machine”. Then the “New VM Wizard” will open.

![Untitled](Everything%20is%20virtual%2007bff1bdac0047a58c1db9af4dd443eb/Untitled%206.png)

Choose the installation media, and the iso image of the desired Operating system, and press forward.

![Untitled](Everything%20is%20virtual%2007bff1bdac0047a58c1db9af4dd443eb/Untitled%207.png)

### Allocate memory, VCPU, Disk Space

After selecting the operating system image file successfully allocate the memory, vcpu and disk space, then press forward.

![Untitled](Everything%20is%20virtual%2007bff1bdac0047a58c1db9af4dd443eb/Untitled%208.png)

### Customize the Virtual Machine settings

In this step we need to customize the settings of the virtual machine. i.e. The name of the virtual machine.

![Untitled](Everything%20is%20virtual%2007bff1bdac0047a58c1db9af4dd443eb/Untitled%209.png)

After this step, the installation of the virtual machine will be started.

![Untitled](Everything%20is%20virtual%2007bff1bdac0047a58c1db9af4dd443eb/Untitled%2010.png)

After finishing the installation, click on the reboot option. This will reboot the virtual machine/guest os.

![Untitled](Everything%20is%20virtual%2007bff1bdac0047a58c1db9af4dd443eb/Untitled%2011.png)

After rebooting, lets log in the virtual machine by entering the username and the password. Then I installed and run the neofetch command to showcase the configuration.

![Untitled](Everything%20is%20virtual%2007bff1bdac0047a58c1db9af4dd443eb/Untitled%2012.png)

# Creating Virtual machines using `virt-install`

Though kvm has a GUI to install virtual machine, the installation can be started on the command line using the `virt-install` command on terminal.

### Creating a guest instance with virt-install command

`virt-install` is a command line based tool for creating virtualized guest instance.
To use the virt-install command, we should first download an iso of the targeted  operating sytem. This time we are selecting the ubuntu server 22.04.

We will need two options for installing a vm using `virt-install` , which are, `kernel` and `initrd` . To find them we need to mount the downloaded iso in a directory. I am mounting the iso file in the `~/vm_with_cli/` directory.

/mnt/casper/initrd
/mnt/casper/vmlinuz

```bash
find ./ -type f \( -name "initrd" -o -name "vmlinuz" \)
```

### Planning VM Resources

We need to adjust the ram, vcpus, and disk size parameters according to the resources we have available. Finally, lets run the virt-install command using the following format (adjusting parameters as needed):

```bash
$ virt-install \
--name mahmud-22301172 \
--memory 1048 \
--vcpus 1 \
--disk size=8 \
--cdrom /home/sabid/Downloads/ubuntu-22.04.3-live-server-amd64.iso \
--os-variant ubuntu22.04
```

After hitting enter, We will see something like this:

![Untitled](Everything%20is%20virtual%2007bff1bdac0047a58c1db9af4dd443eb/Untitled%2013.png)

The new console is the installer console of the virtual machine.

After installing, let us reboot the VM.

This is how it looks like after rebooting.

![Untitled](Everything%20is%20virtual%2007bff1bdac0047a58c1db9af4dd443eb/Untitled%2014.png)

# Creating a shared folder between the guest os and the Host OS

We can do this using both the cli and the gui. In cli method, we need to first create a folder that we want to share with the guest pc  in the host pc.

```bash
$ mkdir shared_gues
```

This command will create a directory in the ~/ of host os.
Now we should edit the xml configuration of the vm to link the folders.

To do that, we have to use the `virsh edit` command. Before using that, we need to make sure that the vm is shutoff.
so,

```bash
$ virsh list --all
Id   Name              State
----------------------------------
-    mahmud-22301172   shut off
```

Then,

```jsx
$ sudo virsh edit mahmud-22301172
```

This command will open the xml file in nano terminal text editor.
We must add these two codeblocks in the xml file.

```xml
<currentMemory unit='KiB'>4194304</currentMemory>
<memoryBacking>
  <source type='memfd'/>
  <access mode='shared'/>
</memoryBacking>
<vcpu placement='static'>1</vcpu>
```

```xml
<devices>
...
...
  <filesystem type='mount' accessmode='passthrough'>
    <driver type='virtiofs'/>
    <source dir='/home/madhu'/>
    <target dir='host_home'/>
  </filesystem>
</devices>
```

In the above XML component,

- source type="memfd" -> This defines memory backing source type. memfd is a specialized anonymous memory-backed file.
- access mode="shared" -> This enables memory to be shared rather than kept private.
type="mount" -> A host directory to mount in the guest. This is the default type if one is not specified.
- accessmode="passthrough" -> The source is accessed with the permissions of the user inside the guest.
- driver type="virtiofs" -> The hypervisor driver used to provide the filesystem, in this case, virtiofs.
- source dir="/home/sabid/shared_guest" -> The directory in the host you would like to share with the Linux guest. In this case, it is the home/shared_guest directory on the KVM host computer.
- target dir="sahred_guest" -> The arbitrary string used to identify the shared directory to be mounted within the guest. I named it “shared_guest“.

Save the XML configuration file and exit.

Now, start the guest virtual machine.

```bash
$ virsh start mahmud-22301172 --console
```

Then, in the guest os, lets create a folder, which we will mount with the host os folder.

```bash
$ mkdir shared_host
```

Then mount the host os file using the sudo mount command to the destination folder.

```bash
$ sudo mount -v -t virtiofs shared_guest ~/shared_host
mount: sahred_guest mounted on /home/sabid/sahred_host.
```

Then I have created a txt file and changed it both from the host and the guest os. It works perfectly.
here is the screenshot.

![Screenshot from 2023-10-26 19-01-12.png](Everything%20is%20virtual%2007bff1bdac0047a58c1db9af4dd443eb/Screenshot_from_2023-10-26_19-01-12.png)

# Cloning a Virtual Machine

## Using KVM-based command:

We can clone a virtual machine easily using the virt clone command.

```bash
virt-clone --original mahmud-22301172 --auto-clone
```

![Untitled](Everything%20is%20virtual%2007bff1bdac0047a58c1db9af4dd443eb/Untitled%2015.png)

Now we can check the added new hard disk from the guest os using the following command:

```bash
$ sudo fdisk -l | grep "^Disk /dev/vd[a-z]"
```

![Here we can see the vda, and the newly added vdb and vdc drives.](Everything%20is%20virtual%2007bff1bdac0047a58c1db9af4dd443eb/Untitled%2016.png)

Here we can see the vda, and the newly added vdb and vdc drives.

## Using Virt-manager GUI:

At first we need to shutdown the guest os. Then right click on the VM. There will be an option to clone the virtual machine. We need to click that option.

![Untitled](Everything%20is%20virtual%2007bff1bdac0047a58c1db9af4dd443eb/Untitled%2017.png)

After clicking the clone button, there will be a window where we can select the customizations of the cloned vm.

![Screenshot from 2023-10-21 19-45-23.png](Everything%20is%20virtual%2007bff1bdac0047a58c1db9af4dd443eb/Screenshot_from_2023-10-21_19-45-23.png)

![Untitled](Everything%20is%20virtual%2007bff1bdac0047a58c1db9af4dd443eb/Untitled%2018.png)

Here we can see that there is a new virtual machine named Mahmud-22301172-clone.

Thus we can clone a virtual machine using the graphical user interface.

## Adding two virtual storage using the cli command:

At first we need to create two virtual hard drives for the vm. We need to qemu-image create command to do it.

```bash
qemu-img create -f qcow2 /var/lib/libvirt/images/new_disk1.qcow2 4G
qemu-img create -f qcow2 /var/lib/libvirt/images/new_disk2.qcow2 3G
```

![Untitled](Everything%20is%20virtual%2007bff1bdac0047a58c1db9af4dd443eb/Untitled%2019.png)

Then we need to execute this command:

```bash
$ virsh attach-disk mahmud-22301172-clone /var/lib/libvirt/images/new_disk1.qcow2 vdb
Disk attached successfully
```

The disk1 has been added to the virtual machine. The name of the disk is vdb.

Now doing this again for the second virtual disk.

![Untitled](Everything%20is%20virtual%2007bff1bdac0047a58c1db9af4dd443eb/Untitled%2020.png)

This is the way to adding additional hard drive to the virtual machine.

# Adding a virtual storage (Hard drive) using the GUI

First We need to connect with the vm using virtual machine manager window.
Then, click on virtual machine hardware details.
Here we will see all the hardware configurations for the machine.

There will be an option Add Hardware. Click on this. There will be a window, where we will see the configuration file for the hard disk. Here I am adding 4 gb additional hard drive storage in the vm.

After clicking on finish, the storage drive will be added.

![Screenshot from 2023-10-21 21-09-07.png](Everything%20is%20virtual%2007bff1bdac0047a58c1db9af4dd443eb/Screenshot_from_2023-10-21_21-09-07.png)

![Here is the second virtual disk.](Everything%20is%20virtual%2007bff1bdac0047a58c1db9af4dd443eb/Screenshot_from_2023-10-21_21-10-15.png)

Here is the second virtual disk.

# Migrating the Virtual Machine:

This process requires multiple steps:

- **********************************Exporting the VM:********************************** Firstly we need to export the configuration of the existing VM. This involves creating an archive that includes all the necessary configuration files, disk images, and metadata. I used **`virsh`** for this. Here's what I did with **`virsh`**:
    
    Firstly we need to make sure that the vm we are exporting is completely shutoff. To do that, we lcan run the command:l
    
    ```bash
    $ virsh list -all
    ```
    
    This will show all the virtual machines and their status. If the status of the machine is running, then we should run this command,
    
    ```bash
    $ virsh shutdown vm_name #in this case it is mahmud-22301172
    ```
    
    This will shutdown the virtual machine.
    
    Now, to export the configuration of the vm:
    
    ```bash
    $ virsh dumpxml mahmud-22301172 > sabid_mahmud_vm.xml
    $ virsh dumpxml mahmud-22301172 --security-info > sabid_mahmud_vm-security.xml
    ```
    
    These commands generated XML configuration files for my VM.
    
    Now I need to copy the virtual disk image file of the VM.
    
    ```bash
    $ sudo cp /var/lib/libvirt/images/mahmud-22301172.qcow2 ./
    ```
    
    Now I need to move them in a folder and then upload the folder in Google Drive or email.
    
    I sent the file to my friend’s pc, there he was able to run the vm.
    

## Accessing mobile phone’s Data:

Unfortunately my mobile phone is not connecting with the Computer. It is not connecting either in windows or linux. It is my mobile phone’s issue. I couldnot manage another phone to do this.