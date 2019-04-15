# cs2400-arm-classwork-1

CS2400 ARM assembly programming classwork problems.

## C and assembly code

Follow the instructions for each of the following code samples in [Compliler Explorer](https://godbolt.org).

1. [printf](https://godbolt.org/z/y2YKew)
   1. What is the library function that is called?
   <stdio.h>
   2. Research the implementation (source code) of this function.
   3. Find out if the program directly executes the output operation or it makes a *system call* to the operating system.
   The program doesn't directly execute output- it calls write(), which is a system call.
   
2. [malloc](https://godbolt.org/z/kAZX7x)
   1. How are the arguments passed to `malloc` and `free`?
   When you malloc, the arguments are passed based on their size. The assembler moves a constant that's the size of the variable into a register, then copies that register into another one, then allocates that much space in memory, incrementing the stack pointer. 
   When you free, the assembler moves a zero into that last register (the same one whose contents were malloced), and then stores the zero to memory, making that contents at that address "null".
   2. Research the implementation (source code) of `malloc` and `free`.
   
3. [malloc array](https://godbolt.org/z/bBl0zx)
   1. How does this case differ from the previous one?
   2. [**hard**] Write your own tiny `malloc` by declaring a large `FILL` area and writing a `malloc` and a `free` subroutine. *Assume you are not getting any `free` calls until you are done with the `malloc` calls. In other words, assume you are not going to have holes in the `FILL` memory you manage for allocations by `malloc`.*
   
4. [arrays](https://godbolt.org/z/lcH006)
   1. Port this code to VisUAL.
   2. Observe/show that this code writes the local array in reverse order to the `static` global array.
   
5. [2d array](https://godbolt.org/z/Kr-Sn8)
   1. Port this code to VisUAL.
   2. How are nested `for` loops handled in assembly? Are they *"nested"* in assembly?
   They're not nested, but they are connected with branch instructions. 
6. [2d array with mul](https://godbolt.org/z/cHwSTR)
   1. Port this code to VisUAL. (It's the same as the previous but with multiplicatoin).
   2. Add your 32-bit unsigned integer multiplication algorithm as a subroutine and run the code. Verify its correctness.
main
	   adr 		r10, iarr
        sub     sp, sp, #8
        movs    r3, #0
        str     r3, [sp, #4]
        b       L2
L5
        movs    r3, #0
        str     r3, [sp]
        b       L3
L4
        ldr     r3, [sp, #4]
        ldr     r2, [sp]
        
        ;mul     r3, r2, r3
        ADR		r0, numbers

		LDR		r3, [r0]
		LDR		r2, [r0, #4]
		MOV		r7, #0
		MOV		r8, #0
		MOV		r9, #0
loop
		TST		r2, #1
		BEQ		shift
		ADDS		r8, r8, r3
		ADC		r9, r9, r7
shift
		LSR		r2, r2, #1
		LSLS		r3, r3, #1
		ADC		r7, r7, #0
		CMP		r2, #0
		BGT		loop
		ADR		r0, result
		STR		r8, [r0]
		ADR		r0, carry
		STR		r9, [r0]
		MOV		r3, r9
		
        lsls    r1, r3, #1
        ldr     r0, r10
        ldr     r2, [sp, #4]
        mov     r3, r2
        lsls    r3, r3, #2
        add     r3, r3, r2
        ldr     r2, [sp]
        add     r3, r3, r2
        str     r1, [r0, r3, lsl #2]
        ldr     r3, [sp]
        adds    r3, r3, #1
        str     r3, [sp]
L3
        ldr     r3, [sp]
        cmp     r3, #4
        ble     L4
        ldr     r3, [sp, #4]
        adds    r3, r3, #1
        str     r3, [sp, #4]
L2
        ldr     r3, [sp, #4]
        cmp     r3, #9
        ble     L5
        movs    r3, #0
        mov     r0, r3
        add     sp, sp, #8
        b     lr
L7
iarr       FILL 52
## Submission
1. Fork this repository
2. Clone and implement.
3. Commit any modifications.
4. Push your commits to your remote.
5. For research-type questions, answer inline in the [README](README.md).

