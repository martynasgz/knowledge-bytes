# Operating Systems

Going through https://youtu.be/H4SDPLiUnv4?t=181.

An operating system acts as an intermediary between software & hardware.

## How 

Each processor has an instruction set - a list of instructions they're able to perform. To give one software more control, we need can define a subset of **privileged instructions**. To enforce this, the hardware must support different operational modes. The common approach to do that is to use a special one bit register that would store either a 1 or a 0. This register is called the **mode bit**. 1 is the privileged mode that allows executing all available instructions, while 0 is restricted mode - only the non-privileged instructions can be ran:
![mode bit|500](/assets/2025-06-27-22-39-49.png)  
The bit's value is ran into an AND logic gate with all of the privileged instructions.

## How Is the Mode Bit Toggled?

If any user process could switch to privileged mode, the feature would be pointless.

The switching mechanism is typically implemented through interrupts
