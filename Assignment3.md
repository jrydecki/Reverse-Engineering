# Assignment 3

## Assembly Review Exercises
1. The main difference between machine code and assembly is that assembly is a symbolic and readable version of machine code, which is all bytes that represent different actions and different instructions for the CPU. An Assembler is used to convert the assembly into machine code, and therefore assembly is a higher level representation of the lowest level instructions for the CPU.

2. If the Stack Pointer is pointing to memory address `0x00000000001270A4` and a `pushq rax` instruction is executed, the Stack Pointer will now be decremented (because the stack grows downward) by 8 bytes (the `q` in `pushq` indicates a 64-bit environment). So, `0x00000000001270A4 - 0x8 = 0x000000000012709C`. Therefore `rsp` is now pointing to `0x000000000012709C`.

3. A stack frame is essentially a chunck of memory defined on the stack for a called function during runtime. It most importantly holds the local variables for a function and a return address for the function.

4. The .data section oftentimes contains global variables for a program and other values that are initilized at the very beginning of the program's runtime. This also oftentimes includes static variables, constants, and strings.

5. The heap is generally used for dynamic memory allocation. This means heap memory is allocated for objects and data structures during the runtime of the program as, contratry to stack memory allocation, it is unknown how much memory will be needed for those instructions during compilation.

6. Within the code section of a program's virtual memory space, the actual instructions to be run for the program are found. The executable instructions for the process to perform are found in the code section.

7. The `inc` instruction increments the register or memory location, which is specified as it's only operand. And increments means to "add 1 to".

8. When you perform a `div` instruction, you would find the remainder in the `%edx` register.

9. The instruction `jz` decides whether to jump or not based on the zero flag in the status register.

10. The instruction `jne` decides whether to jump or not also based on the zero flag in the status register.

11. A `mov` instruction generally moves data from one place to another. The two operands are the source then the destination. The source can be an immediate value, a register, or a memory location and the destination can be a register or another memory location.

12. The `TF` flag is also called the `Trip Flag` and this flag is useful in debugging because when this flag is set, a debug interrupt is called after each instruction is completed on the CPU. This means that after each instruction which this flag is set for, there is a context switch and the state of the process is saved at that moment -- allowing inspection and debugging to occur.

13. An attacker would want to control the RIP (the Instruction Pointer) inside a program because then they can change the value of this pointer to the location of their own, malicious, instructions. Because the computer simply goes to and executes the instruction at the RIP, the attacker would be able to execute their own code.

14. The `ax` register is the lowest byte of the `rax` register. `rax` denotes and references the entire 8 bytes of that register, but if you reference just `ax` this references only to the lowest, or least significant, byte of that register.

15. The result of the instruction `xor %rax, %rax` is `0` being stored in `%rax`, so the entire register is zeroed out.

16. The `leave` instruction sets the stack pointer, `rsp`, to the value of the base pointer, `rbp`, and then pops that base pointer. This essentially takes the stack pointer, moves it back to the base pointer (thereby 'deallocating' that stack frame's memory) and the sets the base pointer to the previous stack frame (the pop).

17. The instruction `retn` in equivalent to the `pop` instruction `pop %rip`.

18. A stack overflow is when the call stack exceeds the allocated memory space for it. Assembly-wise, this occurs when the stack pointer is incremented to the point that the stack pointer in one stack frame moves to/accesses a different stack frame, thereby touching memory it shouldn't have access to.

19. A segmentation fault is error that occurs when the program attempts to access a memory location it does not have the ability to do so in. For example, attempting to write a memory value in a segment that is read-only memory would cause a segmentation fault.

20. `RSI` means `Register Source Index` and `RDI` means `Register Destination Index`. Based on these meanings, `RSI` typically contains the memory address for the input, or source, data; while `RDI` indicates the memory location where the output data can be found (the destination of the output data).

## Crackme Solution

![IDA Image](./src/3-ida)



