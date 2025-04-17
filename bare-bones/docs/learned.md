# Architectures

- x86: A family of 32-bit CPU architectures originating from Intel’s 8086 processor. It includes older generations like i386 (80386) and newer ones like i686 (Pentium Pro/II+).

<!-- - i686: A specific subset of the x86 family, representing 6th-generation CPUs (Pentium Pro/II+) with enhanced 32-bit features (e.g., CMOV, SSE). It is backward-compatible with older x86 CPUs like i386. -->

- i686-elf: A compiler target that generates standalone 32-bit ELF binaries for bare-metal systems (no host OS dependencies). It uses the i686 instruction set but avoids assumptions about a host OS like Linux.
- The -elf suffix indicates the compiler produces freestanding binaries (no host OS libraries like Glibc). This is critical for kernels, which must run without an underlying OS

# Cross Compiler Target Architectures

i686:
- targets: bare metal systems (no host OS deps)
- outputs: pure 32-bit ELF compatible binaries for standalone kernels
- use case: OS development
- cross compiler: required to avoid host OS assumptions

x86:
- targets: host OS environments
- outputs: 32-bit binaries linked to the host OS's C library and runtime
- use case: Compiling 32-bit userspace programs on a 64-bit OS
- cross compiler: native compiler relies on host libraries



# Assembly

.set ALIGN,    1<<0             /* align loaded modules on page boundaries */
.set MEMINFO,  1<<1             /* provide memory map */

- << operator shifts bits of the left number by the amount of the right number
- | bitwise OR -> can be used to combine bits into a byte

.long MAGIC
- .long is used to embed a value into the assembly code (in comparison to defining a constant)

- .align will advance the location counter until it is a multiple of the specified number of bytes

- the kernel doesn't return an error code

- in X86 it is up to the kernel to provide an ESP (Extended Stack Pointer)
- stack grows downwards on x86 and is 16-byte aligned
  - Required for SSE instructions or system calls (e.g., sysenter/sysexit).
  - Misalignment can cause crashes on modern x86 hardware.
- the stack doesn't have to be pre-allocated (before program execution) so kernel size is smaller


- .type directive sets metadata about a symbol’s purpose.
@function marks _start as a function entry point.
@object would indicate data (e.g., variables).


- $stack_top: The $ prefix denotes an immediate value (the address of the stack_top symbol). stack_top is a label defined earlier in your assembly code (e.g., in .bss), marking the end of the stack memory region.

- %esp: The % prefix indicates a register. %esp is the 32-bit stack pointer register on x86.
