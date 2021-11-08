## Assignment 2
- Your assignment is to modify the CPUID emulation code in KVM to report back additional information when special CPUID leaf nodes are requested.
- For CPUID leaf node %eax=0x4FFFFFFF:
	- Return the total number of exits (all types) in %eax
- For CPUID leaf node %eax=0x4FFFFFFE:
	- Return the high 32 bits of the total time spent processing all exits in %ebx
	- Return the low 32 bits of the total time spent processing all exits in %ecx
	- %ebx and %ecx return values are measured in processor cycles, across all VCPUs
## Question 1: Individual
## Question 2 : STEPS
-  Run following command on machine running as hypervisor
    ```
	    - sudo apt-get update
	    - sudo apt-get -y install openssh-server git build-essential kernel-package fakeroot libncurses5-dev libssl-dev ccache libelf-dev bison flex
	    - git clone https://github.com/neetra/linux.git
	    - cd linux/
	    - cp /boot/config-`uname -r` .config	    
	    - make oldconfig
	    - make prepare
	    - make -j 2 modules
	    - make -j 2
	    - sudo make INSTALL_MOD_STRIP=1 modules_install
	    - sudo make install
	    - sudo rmmod -f kvm_intel
	    - sudo rmmod -f kvm
   	    - sudo modprobe kvm
        - sudo modprobe kvm_intel	

    ```
- Steps to create nested virtual machine
	- Enviornment : ubuntu 20.04.3
	- Follow below steps - [Reference](https://www.tecmint.com/install-kvm-on-ubuntu/)
		```
			- sudo apt install -y qemu qemu-kvm libvirt-daemon libvirt-clients bridge-utils virt-manager
			
		```
		- Check the status of libvirt is running
		```
			sudo systemctl status libvirtd
		```
		- Check whether modules are loaded
		```
			lsmod | grep -i kvm
		```
		- Create ubuntu using virtual machine manager


## OUTPUT
   - Exit output is [here](cmpe283_a2_output/exit.png) 	
   - Processor cycle output is [here](cmpe283_a2_output/time.png) 	
## TROUBLESHOOTING
- Enable nested virtualization
    || |
    |-|-|
    |Host Machine | Windows 10 home|
    |Hypervisor|  VmWare Worker 16 Player |
    |Guest Machine| Ubuntu|
    - Open cmd as administrator and run following command
    ```
        bcdedit /set hypervisorlaunchtype off

    ```
    - Enable virtualization in  guest VM. 
        - Right Click on VM, Click settings
        - ![Enable virtualization](en_v.PNG)

-   No rule to make target 'debian/certs/debian-uefi-certs.pem'     
    - Required only if error occurred - [Reference](https://stackoverflow.com/questions/67670169/compiling-kernel-gives-error-no-rule-to-make-target-debian-certs-debian-uefi-ce)
    - Edit .config file and set empty string for keys
        ![Enable virtualization](emp_keys.PNG)
    - make clean
    - sudo reboot
    - make oldconfig 
-   rmmod: ERROR: ../libkmod/libkmod-module.c:793 kmod_module_remove_module() could not remove 'kvm': Resource temporarily unavailable
    rmmod: ERROR: could not remove module kvm: Resource temporarily unavailable
	- Stop nested vm.
	- remove first kvm_intel and then kvm
-  expected ‘const atomic_t * {aka const struct <anonymous> *}’ but argument is of type ‘atomic_long_t * {aka struct <anonymous> *}’
    - Use atomic64_read
 		       
    
    



     

