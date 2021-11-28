## Assignment 3
- Your assignment is to modify the CPUID emulation code in KVM to report back additional information when special CPUID leaf nodes are requested.
- For CPUID leaf node %eax=0x4FFFFFFD:
	- Return the number of exits for the exit number provided (on input) in %ecx
	- This value should be returned in %eax 
- For CPUID leaf node %eax=0x4FFFFFFC:
	- Return the time spent processing the exit number provided (on input) in %ecx
	- Return the high 32 bits of the total time spent for that exit in %ebx
	- Return the low 32 bits of the total time spent for that exit in %ecx
- For leaf nodes 0x4FFFFFFD and 0x4FFFFFFC, if %ecx (on input) contains a value not defined by the 
  SDM, return 0 in all %eax, %ebx, %ecx registers and return 0xFFFFFFFF in %edx. For exit types not 
  enabled in KVM, return 0s in all four registers.
## Question 1: Individual
## Question 2 : STEPS
-  Run following command on machine running as hypervisor
    ```
	    - Ensure that kernel builds (You can refer the steps from assignment 1 from [here](https://github.com/neetra/linux/tree/master/))
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

## HOW TO TEST
- Install cpuid package in the nested VM(Guest VM / VM level 1)
	```
		sudo apt-get install cpuid
	```
- Run following commands
	-  Get total exits in the nested VM
	```
		cpuid -l 0x4FFFFFFF	
	```
	-  Get total time processing exits(processor cycles) in the nested VM
	```
		cpuid -l 0x4FFFFFFE

## Question 3 : Comment on the frequency of exits – does the number of exits increase at a stable rate? Or are there more exits performed during certain VM operations? Approximately how many exits does a full VM boot entail?
- The number of exits do not increase at stable rate.
- VM reboot performs approximately ... exits
- 
## Question 4:  Of the exit types defined in the SDM, which are the most frequent? Least?
- Most frequent exits:
	|Exit No| Exit description| Output
	|--|--|--|
	|1|External Interrupt||
	|12|HLT||
	|30| I/O||	
	|32|WRMSR||
	||48| EPT Violation||
	|49| EPT misconfiguration|
- Least Frequent exits:
	
	|Exit No| Exit description| Output
	|--|--|--|
	|29|Mov DR||
	|54|WBINVD||
	|55|XSETBW||
## OUTPUT
   - Test Case : Valid exit but not enabled in KVM and Invalid exit
	![image](ass3/TestCaseInvalidAndNotenable.png)
 

   - Test Case: Number of exits for VM reboot
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
-  Refer [this](https://stackoverflow.com/questions/13772567/how-to-get-the-cpu-cycle-count-in-x86-64-from-c) to get total processor cycles.
 		       
    
    



     

