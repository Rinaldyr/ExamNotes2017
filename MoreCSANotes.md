# CSA Notes

See CSA-CourseNotes.pdf for content not covered in these notes


----

# Miroarchitecture

## MIPS Datapath

The Datapath is a collection of functional unit which implement the instruction set.

Each unit has a specific purpose.



Unit | Description
--- | ---
Registers | Storing data
Program Counter | Book marks code
Instruction Register | Stores current instruction
ALU | Executes arithmetic and logic operations
Control Unit | Configure the datapath such that desired instruction is correctly implemented.
Load/Store Unit | Accessing the memory to be used by the CPU



##### Summary of the instruction execution cycle:

1. Get next address from PC

2. Fetch next instruction from memory

3. Update PC

4. Decode instruction

5. Fetch required data (if any)

6. Execute instruction

7. Return to 1



### Instruction fetch & PC increment

First step is to get the address of next instruction.

Program counter is sent to memory as an address via address bus.

Contents of that address is loaded onto the IR via data bus.

We use PC, IR and Main Memory (not a part of the datapath).

Then increment PC (adding "4"). This is done frequently so simple adder is included.



### ALU (R-type) instructions

Performs arithmetic and logic operations (e.g. comparisons).

Used to compare address.

Must be able to accept any 2 32-bit words from the registers, perform the instruction, and output another 32-bit word (or true/false).

Once the correct registers have been selected and the ALU correctly configured, the contents of the source registers can be read by the ALU, which will perform the selected operation and put the result back in the destination register.

### Load/stores

Surprisingly more complex than ALU.

Need R/W access to registers, R/W access to memory and be able to use ALU to calculate addresses.

Address may be offset but offset is 16-bits whereas base address is 32-bits. 

Therefore, sign extension unit used to perform manipulation to translate offset (not so trivial due to negative offsets using two's complement representation).

### Branches and Jumps



Opcode | Base Address | Src/Dest | Address Offset
--- | --- | --- | --- 
6 | 5 | 5 | 10 



Computation for `beq`: Compare the contents of the 2 registers specified. If they are equal, add the specified offset to the PC. Otherwise, the PC is incremented by 4 as normal.

16-bit offset is used but we need to extend to 32-bits. Therefore ALU is used for comparison and dedicated adder used for calculating new address. 

### Integration & Multiplexing

We don't want seperate ALU's for branches/loads and instruction. So we will put these datapaths together to form one single datapath.

This is called **multiplexing**.

Allows us to switch between different signals to allow multiple pieces of data to go to the same functional units.

In other words, transmit multiple signals along the same channel and select which one is allowed to pass.

A multiplexer with N control wires allows for 2^N inputs.

### Jumps

In jump instruction, a 26-bit memory addres is encoded.

This is the word address (not a byte address) so it is converted to a byte address by shifting it by 2 bits.

It is then concatenated with the 4 most significant bits from the PC to form a full 32-bit address.

The jump instruction can span up to 256MB relative to the current position.

Jump instruction then multiplexed with the PC update value from the branch multiplexer and selected using the "jump" signal.



## CPU Control

### ALU Control (Not sure if assessed so incomplete)

ALU opcode is walsy 000000, and precise instruction is specified by `func` code.

But R-Type ALU are not the only instruction that use ALU so no as simple as reading the `func` code.

Therefore another control signal `alu_op` gives extra information about the kind of instruction.

Consider various types that use ALU:

**Loads and Stores:** use ALU to add address offset to base register. Must be configured for addition.

**Branches:** use ALU to perform comparison. 

### Single-cycle: combinatorial control

Can be implemented easily using stateless (combinational) logic. Input is 6-bit opcode and output are control signals which are a simple function of the opcode and combinational logic.

Appropriate if each instruction can be completed in one clock cycle.

Some instruction (memory related for example) take long. Instead of extending clock cycle to allow all operations in 1 cycle, we allow some instruction to take multiple. Serious complication.

### Multi-cycle: state machine or microcode

Performance is gained but control is much harder.

One way to implement is to use a **state machine**. The outputs of which depend on current state and its inputs.

It allows various stages to be stract, and can stall the machine while waiting for instruction to execute. IOW, next instruction doesn't start until current state indicates that current instruction is finished.

Straight forward for simple designs but almost impossible for complex designs. Therefore, a better approach is to use a microcode.

##### Microcode

Instructs are expressed as a sequence of microinstructions. Each microinstruction configures the datapath in a particular way.



Example `lw`:

First microinstruction issues requrest to memory.

Few null microinstructions whilst awaiting data.

When data ready, microinstruction that configures registeres for writing is issued.



Structure if microcode same as program code. But instead of being stores in RAM, it is stored in ROM (Read only memory). Not accessible or modifiable by the programmer. 

----

# Number Representation

### Unsigned integers

Any natural number can be represented in binary but there are practical limitations. Computers break data into 32-bit or 64-bit chunks (words) and this limits the maximum size of numbers. With n bits, we can store from 0 to 2^n - 1. 



**Converting from base 10 to base 2:**

Keep dividing by 2 to deduce the least significant bit.


24/2 = 12 remainder 0


12/2 = 6 remainder 0


6/2 = 3 remainder 0


3/2 = 1 remainder 1


1/2 = 1 remainder 1


24 base 10 = 11000 base 2

### Unsigned fixed point

**Converting from base 10 to base 2:**

Mutliplying by 2 and take integer part of the result.



0.142 * 2 = 0.284



0.284 * 2 = 0.568



0.568 * 2 = 1.136



1.136 * 2 = 0.272



0.272 * 2 = 0.544



0.544 * 2 = 1.088



1.088 * 2 = 0.176



0.176 * 2 = 0.352



0.142 base 10 = 0.00100100 base 2





### Signed integers/fixed point: two's complements

Naivie solution to negative numbers is to add extra bit for sign. This is suboptimal and inefficient - it would also require different hardware for subtraction.

Adding 11111111111... to any number is equivalent to adding -1.

01011010 and 10100110 add to 0 so they are the negative of each other. They are called each other's two's complement.



**To form a number's two's complement:**

1. Invert all bits.

2. Add 1 to the least significant bit.



Subtraction example 7-15:



(00111 - 01111)



Convert the 7 into it's two's complement and add



(00111 + 10001) = 11000.



Leading digit is 1 so it is negative. Convert to two's complement gives us 8.

# Digital Logic

### Logic gates

### Combinational Logic

Combinational logic is blocks of digital logic which contain no memory: output is solely dependent on input.

A block of cominational logic can be described by its truth table.

For a logic block of $n$ inputs, there are $2^n$ possible output states.

Examples: Multiplexers, adders, ALU.

Not examples: Registers (since they are a form of memory)

### Building CPU components
Here are Boolean algebra laws:
![](https://i.gyazo.com/0c79b9bad478913d53c457ef091f5f06.png)

#### Decoder
Decoder designed so that each of four possible input combinations uniquely selects only 1 of the possible output.

Here is the truth table for a decoder:

i0 | i1 || Q1 | Q2 | Q3 | Q4
--- | ---
0 | 0 || 1 | 0 | 0 | 0
0 | 1 || 0 | 1 | 0 | 0
1 | 0 || 0 | 0 | 1 | 0
1 | 1 || 0 | 0 | 0 | 1

Now let's formulate this as logic equations:

$Q_1 = ¬A $
 
# Clocked logic

### Memory and Flip-flops

### State machines

# Interrupts & Peripherals

### I/O Requirements

### Memory-mapped vs port-mapped I/O

### Polling

### Interrupts

### DMA

# Improving Performance

### Caches

### Virtual Memory

### Calculating cache benefits

### Pipelining: MIPS pipeline, hazards

### Branch Prediction: static, dynamic, speculative execution, delayed branching
