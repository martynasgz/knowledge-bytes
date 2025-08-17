# System Calls

System calls represent an interface to services provided by the operating system, allowing user programs to request privileged operations to be performed by the OS in kernel mode. Otherwise user code is ran in the restricted user mode. Thus, if you only have non-privileged code that doesn't need system calls, the code doesn't depend on the OS anymore.

## Relationship with the OS

The same OS system call API can be used to execute privileged tasks in different architectures - the different machine code implementation details are taken care of by the OS, so the user code only cares about the system call operations the OS exposes. Usually, languages have libraries (for example, libc in C) which define the OS-specific system calls as functions with similar names. The https://youtu.be/eP_P4KOjwhs video creates confusion when defining, for example, CreateProcess as a system call, even though it's just a function that invokes a system call with OS-specific machine-code.

Even if you're implementing system calls yourself (for example - with Assembly), the meaning of the syscall number and the expected arguments are defined by the OS, not the CPU. Different OS'es execute syscalls differently too.

To illustrate the difference OS creates, let's take an example - on Windows, `CreateProcess()` is used to create a new process. The Unix equivalent `fork()`, on the other hand, also creates a new process, but it actually clones the process it's called on, with the only difference being the process ID. Thus, on Unix, additional steps are required to run a program - the child process must also use the `exec` system call for a different executable. So, **on one platform a task might require a single system call, while on another it might need several**. This means that when the task is compiled to machine code, platforms will use different instructions to achieve the same result. Even though both programs are compiled for the same architecture, those extra instructions won't have meaning on the other OS.


https://chatgpt.com/c/685efe71-99f0-8009-b19b-31d3afe3de73



