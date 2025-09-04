---
상위 개념: "[[Linux]]"
---
# Linux Source Tree

리눅스 소스코드의 루트 디렉터리는 다음과 같이 구성되어있다.

| Directory     | Description                                   |
| ------------- | --------------------------------------------- |
| arch          | Architecture-specific source                  |
| block         | Block I/O Layer                               |
| crypto        | Crypto API                                    |
| Documentation | Kernel source documentation                   |
| drivers       | Device drivers                                |
| firmware      | Device firmware needed to use certain drivers |
| fs            | The VFS and the individual filesystems        |
| include       | Kernel headers                                |
| init          | Kernel boot and initialization                |
| ipc           | Interprocess communication code               |
| kernel        | Core subsystems, such as the scheduler        |
| lib           | Helper routines                               |
| mm            | Memory management subsystem and the VM        |
| net           | Networking subsystem                          |
| samples       | Sample, demonstrative code                    |
| scripts       | Scripts used to build the kernel              |
| security      | Linux Security Module                         |
| sound         | Sound subsystem                               |
| usr           | Early user-space code (called initramfs)      |
| tools         | Tools helpful for developing Linux            |
| virt          | Virtualization infrastructure                 |
