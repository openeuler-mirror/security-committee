##  Requirements for OpenEuler Security Compilation Options (C&C++)

It is recommended to use security compilation options in the following way to enhance the security quality of versions.

### Contents

- [Enable Address Space Randomization](#Enable Address Space Randomization)
- [Enable Stack Protection](#Enable Stack Protection)
- [Enable GOT Table Relocation Read-Only Option](#Enable GOT Table Relocation Read-Only Option)
- [Enable Immediate Binding Option](#Enable Immediate Binding Option)
- [Enable Non-Executable Stack Option](#Enable Non-Executable Stack Option)
- [Enable -s Option or Use Strip Tool](#Enable -s Option or Use Strip Tool)
- [Disable the Use of rpath Option](#Disable the Use of rpath Option)
- [Enable FS Option](#Enable FS Option)
- [Enable fvisibility Option](#Enable fvisibility Option)
- [Enable ftrapv Option](#Enable ftrapv Option)
- [Enable Stack Check Option](#Enable Stack Check Option)
- [Enable Stack Clash Protection Option](#Enable Stack Clash Protection Option)

### Enable Address Space Randomization

**Level:** Mandatory

**Description:**

**Linux User Space**

a. Use the command `echo 2 >/proc/sys/kernel/randomize_va_space` to enable system randomization configuration.

**Effect Phase:** Run-time system configuration

**Scope:** Heap, stack, memory mapping area (mmap base address, shared libraries, vdso page)

**Usage:** `echo 2 >/proc/sys/kernel/randomize_va_space`

**Explanation:**

Address Space Layout Randomization (ASLR) is a security protection technique against buffer overflow attacks. It randomizes the layout of the heap, stack, and memory-mapped areas such as the base addresses of mmap, shared libraries, and vdso pages. This makes it harder for attackers to predict target addresses, preventing them from directly locating attack code positions and thwarting overflow attacks. When `randomize_va_space` is set to 1, the stack, data segment, and VDSO are randomized. When set to 2, the heap address is also randomized.

ASLR should be set to the highest level, i.e., `randomize_va_space` equal to 2.

b. Enable Position Independent Code (PIC) to achieve dynamic library random loading.

**Effect Phase:** Compilation option

**Scope:** Dynamic library

**Usage:** `-fPIC` (`-fpic`)

**Explanation:**

Address-independent options relocate the code segment to the data segment to achieve dynamic library loading without modifying the code segment when the shared object (so) file is loaded.

-fPIC and -fpic both instruct GCC to generate position-independent code. The only difference is that -fPIC generates slightly larger code, while -fpic generates relatively smaller code.

c. Enable Position Independent Executable (PIE) to achieve random loading of executable files.

**Effect Phase:** Compilation and linking options

**Scope:** Executable programs

**Usage:** `-fPIE` (`-fpie`) `-pie`

**Explanation:**

Executable files with PIE can be randomly loaded during execution, similar to shared libraries. Research suggests that PIE can effectively reduce the success probability of attacks such as fixed-address and buffer overflow attacks.

(1) Check whether the corresponding hot patch version supports the PIE option. It is not recommended to use this option in scenarios where it is not supported.

(2) -fPIE compilation option, -pie linking option.

(3) -fPIE generates slightly larger code, while -fpie generates relatively smaller code.

### Enable Stack Protection

**Level:** Mandatory

**Description:**

**Linux Platform User Space**

**Effect Phase:** Compilation option

**Scope:** Object files (.o), dynamic libraries, executable programs

**Usage:** `-fstack-protector-all` or `-fstack-protector-strong`

**Explanation:**

In the presence of buffer overflow vulnerabilities, attackers can overwrite the return address on the stack to hijack program control flow. Enabling stack protection inserts a canary word between the buffer and control information. When overwriting the return address, attackers often overwrite the canary word as well. By checking whether the canary word has been modified, it can be determined whether a buffer overflow attack has occurred.

**Linux Platform Kernel**

**Effect Phase:** Compilation option

**Scope:** Linux platform kernel space

**Usage:** Enable configuration `CONFIG_CC_STACKPROTECTOR` or `CONFIG_CC_STACKPROTECTOR_STRONG` before kernel compilation.

**Explanation:**

`CONFIG_STACKPROTECTOR` corresponds to `-fstack-protector`.

`CONFIG_CC_STACKPROTECTOR_STRONG` corresponds to `-fstack-protector-strong`.

### Enable GOT Table Relocation Read-Only Option

**Level:** Mandatory

**Description:**

**Linux Platform User Space**

a. Partial redirection read-only option

**Effect Phase:** Linking option

**Scope:** Dynamic libraries, executable programs

**Usage:** `-Wl,-z,relro`

**Explanation:**

Dynamic linking ELF binary programs use a Global Offset Table (GOT) lookup table to dynamically resolve functions in shared libraries. Attackers can exploit buffer overflows to modify function address values in the GOT table. By adding the RELRO option, the GOT table can be protected from malicious overwrites.

b. Full redirection read-only option

**Effect Phase:** Linking option

**Scope:** Dynamic libraries, executable programs

**Usage:** `-Wl,-z,relro,-z,now`

**Explanation:**

Enabling partial redirection read-only protection and then enabling immediate binding can achieve full redirection read-only protection, i.e., all redirections are read-only (GOT table fully protected): `-Wl,-z,relro,-z,now`. This provides better protection against attacks such as ret2plt, but may not prevent buffer overflow attacks.

### Enable Immediate Binding Option

**Level:** Mandatory

**Description:**

**Linux Platform User Space**

**Effect Phase:** Linking option

**Scope:** Dynamic libraries, executable programs

**Usage:** `-Wl,-z,now`

**Explanation:**

1. If there are issues with dynamic library circular dependencies, resolve the circular dependency before implementing this option.
2. This option may slow down the startup of programs containing dynamic libraries but speeds up the first call to functions.

### Enable Non-Executable Stack Option

**Level:** Mandatory

**Description:**

**Linux Platform User Space**

**Effect Phase:** Linking option

**Scope:** Dynamic libraries, executable programs

**Usage:** `-Wl,-z,noexecstack`

**Explanation:**

1. If there are inline functions, it may cause functional errors, and you need to check with -Wtrampolines first.

### Enable -s Option or Use Strip Tool

**Level:** Mandatory

**Description:**

**Linux Platform User Space**

**Effect Phase:** Linking option

**Scope:** Dynamic libraries, executable programs

**Usage:** `-s` or strip tool

**Explanation:**

Symbols play a crucial role during the linking process, acting as the glue that binds multiple different object files together. The symbol table has no effect on the executable file's runtime and can become a tool for attackers to construct attacks. Therefore, deleting the symbol table can defend against hacker attacks. Deleting the symbol table is not only a defense against attacks but also reduces the size of the file, reducing the file size.

1. For static libraries, relocatable files (.o) cannot be stripped; otherwise, compilation errors will occur. This only applies to ELF executable files and dynamic libraries.
2. You can use the "-s" compilation option or the strip tool to delete the symbol table from dynamic libraries and executable files.
3. It is recommended to use the strip tool directly before publishing, with the default strip level, such as `strip bin.out`.

### Disable the Use of rpath Option

**Level:** Mandatory

**Description:**

**Linux Platform User Space**

**Effect Phase:** Linking option

**Scope:** Dynamic libraries, executable programs

**Usage:**

- `-Wl,--disable-new-dtags,--rpath,/libpath1:/libpath2`
- `-Wl,--enable-new-dtags,--rpath,/libpath1:/libpath2`
- `-Wl,--enable-new-dtags,--rpath,/libpath1:/libpath2`

**Explanation:**

This is mainly used to protect against LD_LIBRARY_PATH replacing attacks on the same named dynamic library. By adding this option, you can specify a runtime dynamic library search path with a higher priority than the path specified by LD_LIBRARY_PATH. When an executable file searches for dynamic libraries during runtime, it will first look in the path specified by --rpath and then search in the path specified by LD_LIBRARY_PATH. Therefore, it can effectively defend against LD_LIBRARY_PATH=[attackpath] attacks to replace the same named dynamic library.

However, this option has many limitations. For example, if the specified path is not secure, ordinary users can use malicious programs to replace normal programs in these directories, causing privilege escalation and triggering insecure path vulnerabilities. Therefore, it is recommended to disallow the use of rpath.

### Enable FS Option

**Level:** Recommended

**Description:**

**Linux Platform User Space**

**Effect Phase:** Compilation option

**Scope:** Relocatable files (.o), dynamic libraries, format executable programs

**Usage:** `-D_FORTIFY_SOURCE=2 -O2`

**Explanation:**

When the program uses static fixed-size buffers, adding this option will cause the compiler or runtime library to check calls to related functions at compile time or runtime. This has an impact on performance, and it is recommended to decide whether to use it based on test results.

In principle, the recommended level is -O2 (performance optimization is better than O1).

### Enable fvisibility Option

**Level:** Recommended

**Description:**

**Linux Platform User Space**

**Effect Phase:** Compilation option

**Scope:** Dynamic libraries

**Usage:** `-fvisibility=hidden`

**Explanation:**

After using the -fvisibility=hidden option, control symbols are globally invisible.

### Enable ftrapv Option

**Level:** Recommended

**Description:**

**Linux Platform User Space**

**Effect Phase:** Compilation option

**Scope:** Relocatable files (.o), dynamic libraries, format executable programs

**Usage:** `-ftrapv`

**Explanation:**

When the -ftrapv option is used, signed integer addition, subtraction, and multiplication between signed integers are not implemented through CPU instructions but through functions contained in the GCC auxiliary library libgcc.c. This option has a significant impact on performance.

### Enable Stack Check Option

**Level:** Recommended

**Description:**

**Linux Platform User Space**

**Effect Phase:** Compilation option

**Scope:** Relocatable files (.o), dynamic libraries, executable programs

**Usage:** `-fstack-check`

**Explanation:**

Stack-check checks the stack space used by the program during compilation. If it exceeds the compilation warning threshold, a warning is generated. Then, additional instructions are generated in the program to check that the runtime stack does not overflow. The stack-check option sets a safe buffer at the lowest bottom of each stack space in a function. If the allocated stack space in a function enters the safe buffer, a Storage_Error exception is triggered. However, the code generated by this option does not actually handle exceptions. If an exception is detected, a message is issued to notify the operating system to handle it. It only ensures that the operating system can detect stack overflow. This option has a significant impact on performance.

### Enable Stack Clash Protection Option

**Level:** Recommended

**Description:**

**Linux Platform User Space**

**Effect Phase:** Compilation option

**Scope:** Relocatable files (.o), dynamic libraries, executable programs

**Usage:** `-fstack-clash-protection`

**Explanation:**

After enabling this option, the compiler allocates only one page of stack space at a time and immediately accesses each page after allocation. Therefore, it can prevent allocation from skipping any stack pages protected by the operating system.