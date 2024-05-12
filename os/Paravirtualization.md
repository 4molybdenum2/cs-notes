
- Modify Guest OS to not issue sensitive instructions
- Xen runs in ring 0
- Guest OS can make a system call like hypercall
- any hypercall is trapped to xen in ring 0


x86 paravirt exceptions:
1. guest OS registers a descriptor table for exception handlers with Xen
2. aside from page faults, handlers remain the same

x86 paravirt system calls:
1. fast system calls introduced so everytime Xen hypervisor is not involved in syscalls
2. allows direct calls from application into guest OS

x86 paravirt interrupts:
1. hardware interrupts, replaced by lightweight event notification system
2. event notification is a means of an upcall instead of interrupts


x86 paravirt time:
1. modify kernel to understand virtualized environment
2. wall clock vs virtualized time
3. guest OS has a timer interface and knows about both the real and the virtual time




Memory paravirt segmentation:
1. Xen lives in top 64mb linear address space
2. we want to isolate hypervisor address from the guest address
3. Guest OS has direct access to hardware page tables
4. Updates are **batched** and **validated** by Xen
5. Update page tables via **hypercalls**
6. a domain may be allocated, discontiguos machine pages


Xen doesn't swap out memory allocated to domains
- provides consistent performance to domains

instead:

Balloon drivers are used to grow/shrink guest memory
- memory target set as value in XenStore
- if guest above target swap out to Xen
- if guest below target, increase usage

Hypercalls allows guests to see / change state of memory
- physical-real mappings



Split Device Drivers:
1. Xen doesn't emulate hardware device
2. Uses shared memory buffer async IO rings
3. Domain 0 - root domain -> device driver of domain 0 controls hardware -> is a part of Xen, but acts as a regular guest OS
4. Domain X -> guest OS 
5. Front end device driver -> suppose networking -> use front end device driver -> to put request to shared ring -> accessed by domain 0 backend driver -> uses domain 0 actual device driver to send request
6. Backend device driver -> 







From wikipedia:
In [software engineering](https://en.wikipedia.org/wiki/Software_engineering "Software engineering"), **containerization** is [operating system-level virtualization](https://en.wikipedia.org/wiki/Operating_system-level_virtualization "Operating system-level virtualization") or [application-level virtualization](https://en.wikipedia.org/wiki/Application-level_virtualization "Application-level virtualization") over multiple network resources so that software applications can run in isolated user spaces called _containers_ in any [cloud](https://en.wikipedia.org/wiki/Cloud_computing "Cloud computing") or non-cloud environment, regardless of type or [vendor](https://en.wikipedia.org/wiki/Vendor "Vendor").[[1]](https://en.wikipedia.org/wiki/Containerization_(computing)#cite_note-1)

The _containers_ are basically a fully functional and portable cloud or non-cloud computing environment surrounding the application and keeping it independent of other environments running in parallel.[[2]](https://en.wikipedia.org/wiki/Containerization_(computing)#cite_note-2) Individually, each container simulates a different software application and runs isolated processes[[3]](https://en.wikipedia.org/wiki/Containerization_(computing)#cite_note-3) by bundling related configuration files, libraries and dependencies.[[4]](https://en.wikipedia.org/wiki/Containerization_(computing)#cite_note-4) But, collectively, multiple containers share a common [operating system kernel](https://en.wikipedia.org/wiki/Kernel_(operating_system) "Kernel (operating system)") (OS).[[5]](https://en.wikipedia.org/wiki/Containerization_(computing)#cite_note-5)




Function as a Service (FaaS):

making virtualization smaller and smaller, only virtualization functions nowadays




