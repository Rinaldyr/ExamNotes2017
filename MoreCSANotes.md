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

ALU opcode is always 000000, and precise instruction is specified by `func` code.

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

For a logic block of n inputs, there are 2^n possible output states.

Examples: Multiplexers, adders, ALU.

Not examples: Registers (since they are a form of memory)

### Building CPU components
Here are Boolean algebra laws:

![](https://i.gyazo.com/0c79b9bad478913d53c457ef091f5f06.png)

#### Decoder
Decoder designed so that each of four possible input combinations uniquely selects only 1 of the possible output.

Here is the truth table for a decoder:

i0 | i1 || Q1 | Q2 | Q3 | Q4
--- | --- | --- | --- | --- | --- | --- 	
0 | 0 || 1 | 0 | 0 | 0
0 | 1 || 0 | 1 | 0 | 0
1 | 0 || 0 | 0 | 1 | 0
1 | 1 || 0 | 0 | 0 | 1

Now let's formulate this as logic equations:

Q1 = ¬i0 ^ ¬i1

Q2 = ¬i0 ^ i1

Q3 = i0 ^ ¬i1

Q4 = i0 ^ i1

From this, it's trivial to translate into a circuit diagram.

![](https://i.gyazo.com/b27b5d99bad73bdef00efd0b08ca4e7e.png)

#### Multiplexer
Truth table: 

 i0 | i1 || S | Q
--- | --- | --- | --- | --- 	
0 | 0 || 0 | 0 
0 | 0 || 1 | 0 
0 | 1 || 0 | 0 
0 | 1 || 1 | 1 
1 | 0 || 0 | 1 
1 | 0 || 1 | 0 
1 | 1 || 0 | 1 
1 | 1 || 1 | 1 

Logic Expression:

Q = (i1 ^ S) V (i0 ^ ¬S)

Diagram:

![](https://i.gyazo.com/6d780654ef0d023bb1ebfaf86f4c5706.png)

#### Simple Adder

The circuit we will design will be able to add unsigned integers, two-complement integers, and both unsigned and two’s complement ﬁxed-point numbers.

We know that:

0 + 0 = 0

0 + 1 = 1 + 0 = 1

For 1 + 1, we need a second output, the carry.

Truth table for single bit addition:

A | B || Q | C
--- | --- | --- | --- | --- 	
0 | 0 || 0 | 0 
0 | 1 || 1 | 0 
1 | 0 || 1 | 0 
1 | 1 || 0 | 1 

It's obvious that C = A ^ B from truth table.

Q is not so obvious. We must introduce XOR (exclusive or):

![](https://i.gyazo.com/bfae4f3c52e2e6e48cb912181fdd394b.png)

Q = A ⊻ B
C = A ^ B

Here is the circuit diagram: 

![](https://i.gyazo.com/75bdf089d71d9550f27396f293acf123.png)

This is referred to as half adder. Works for adding single-bit numbers, but we want to add multiple-bit numbers.

Half adder can be used for LSB but for higher bits we need to take the previous carry as an input. This is a full adder.

Truth table for full adder: 

 i0 | i1 || Cin | Q | Cout
--- | --- | --- | --- | --- | ---
0 | 0 || 0 | 0 | 0  
0 | 0 || 1 | 1 | 0
0 | 1 || 0 | 1 | 0
0 | 1 || 1 | 0 | 1
1 | 0 || 0 | 1 | 0
1 | 0 || 1 | 0 | 1
1 | 1 || 0 | 0 | 1
1 | 1 || 1 | 1 | 1

Circuit diagram:

![](https://i.gyazo.com/03ac719997dbaffe24e48766ec6bf656.png)

32-bit full adder for unsigned ints:

![](https://i.gyazo.com/9e61f221359eb58240c54ef45630beb3.png)

#### Simple ALU

We can put together elementary ALU that can add, AND and OR.

In order to do this, we take out 32-bit adder and mod-ify it by adding one AND gate per bit, and one OR gate per bit, plus some multiplexers to select which operation we wish to per-form:

![](https://i.gyazo.com/eff9d4636a7aada44234e6c6b2d2fa14.png)


# Clocked logic

All processors use an externally provided clock signal to synchronise the timing of the machine so we can always guarantee that the output of a functional unit is stable at a particular well-deﬁne moment in time. 

The frequency of the clock signal is chosen to ensure that all signal paths are stable, even in the worst case (poor quality materials, high temperature).

### Memory and Flip-flops

In order to stabilise before they are used, we need to store them for the duration of the clock cycle.

The start is taken to be the rising edge of the clock signal.

On this event, outputs of blocks of combinational logic are captured by flip-flops.

Output of flip-flops only change on rising clock edge, regardless of change in  inputs, making it stable for the whole cycle. This allows outputs of subsequent blocks of cominational logic to stabilise before inputs change again.

Visualisation:

![](https://i.gyazo.com/86e7ef462e4c38b431f9a0107db4ecd0.png)

Flip-Flops are widely used throughout the CPU whenever we have something to store or protect. 

In MIPS, each register is simply a bank of 32 ﬂip-ﬂops.  

#### Master-Slave flip-flop (D-Type)

D-Type has 2 inputs (detain and clock) and 1 output (dataout).

D-types are edge-triggered so output data only gets the input data when clock signal changes from 0 to 1.

Not immediately obvious how to make a circuit that is edge-triggered. The key is to make it in two 

Stage 1: Allow input data to pass only when clock is low

Stage 2: Only allow data to pass when clock it high machines

##### Set-reset latch

The basic building block for ﬂip-ﬂops is the set-reset latch, which implements a simple memory element.

We will take advantage of the properties of the NOR gate, which can be programmed to behave like an inverter by setting one of it’s inputs to zero. 

The output is then the inverse of the other input. Let us examine the behaviour of the cross-coupled NOR gates referred to as a bistable circuit

![](https://i.gyazo.com/f7b581b087245782a3c90efa1aae84d7.png)


To aid us, here is truth table for NOR:

A | B | Q
--- | --- | --- |
0 | 0 | 1
0 | 1 | 0
1 | 0 | 0
1 | 1 | 0


* When R=0 and S=1, then Q=1 and Q'=0. (Q' must be 0 because one input is 1). **Setting the latch**

* When R=1 and S=0, then Q=0 and Q'=1. (Q must be 0 because one input is 1) **Resetting the latch**

* If R=1 and S=1, then Q=0 and Q'=0. (We need to avoid this)

* If R=0 and S=0, outputs hold their current values.

We need to add further logic to prevent the 3rd case when both inputs are 1.

A modified bistable circuit forms a D-latch:

![](https://i.gyazo.com/bc44b4f53695aa32264e9ee0d4a6e962.png)

* If C=1, then R=0 and S=0 holding current state
* If C=0, then R=¬D and S=D
	* If D=0, then S=0, R=1 (reset)
	* If D=1, then S=1, R=0 (set)

(S=1 and R=1 is not possible)

 Now we connect 2 D-latches together to create an edge-triggered D-type flip flop

![](https://i.gyazo.com/4d7240ca2774271345e25e669bb6d0dc.png)

* When clock is low, first latch is open and output follows its input.
* When clock is high, second latch opens and first one closes.

On the rising edge, the second latch captures the ouput of the ﬁrst at that time, and prevents it from being changed until the next rising edge. 

Output of ﬂip-ﬂop can be read at any time, and is stable for the whole clock cycle, except for a short period just after the rising clock edge, when it may change as the input is captured.

A common modiﬁcation made to D-type ﬂip-ﬂops is to provide a mechanism by which a ﬂip-ﬂop can be selectively written so we can use it in, for example, a register, where we don’t want the stored value to be overwritten on every cycle. 

An obvious way to do this is to AND the clock with a write signal, but this is generally avoided as it introduces a extra delay to the clock signal which can cause problems.

A more usual method is to feed the output back to the input and add extra logic such then when write=1, the input of the master latch gets D and when write=0, the input of the master latch gets Q. Here is the circuit diagram:

![](https://i.gyazo.com/b1788524a39445e2f45556df17c3dcad.png)

### Registers

Edge-triggered ﬂip-ﬂops can be used as the basis for a register.

A single 32-bit register has 32 D-type flip flops such that input to each flip-flop corresponds to 1 bit of input data, and output of each flip-flop corresponds to 1 bit of output data.

The clock and write signals are shared by all ﬂip-ﬂops, although often we allow individual bytes to be written, in which case a register has separate write signals for each byte. This would be necessary for MIPS, which has a store-byte (sb) instruction.

![](https://i.gyazo.com/17ecb3147b9bd8fd34a848e0c6ece389.png)

#### Building a register file

Having constructed a single 32-bit registers, we can put together a full 32-register register ﬁle such as the one at the core of the MIPS datapath. Revisiting the structure of the datapath we recall that the registers have:

* 1 x 32-bit write port
* 2 x 32-bit read ports
* 1 x 5-bit write address
* 2 x 5-bit read addresses
* 1 register_write signal

To implement, we need:

* 32 x 32-bit registers
* 2 x 32-1 multiplexers, to select which two of the 32 registers will be propagated to the output as specified by the read addresses.
* 1 x write decoder, to select which register should be written to during a write, as specified by the write address
* Some extra logic to ensure the write only occurs when register_write is asserted.

![](https://i.gyazo.com/55a35c2e61238d0a26642a50eb750200.png)

Other clocked elements, like the program counter and instruc-tion register can be built similarly easily

### State Machines

All state machines consist of:

* Inputs
* Outputs
* A state register
* Combinational logic

Two ways these can be combined.

1. **Mealy state machine**: outputs and the next state are a combinatorial function of both the state and the input
2. **Moore state machine**:  ouputs are a function of the state only, whilst the next state depends on the inputs and the current state

![](https://i.gyazo.com/2395c984f15d7c8e7ac576620eaeb4c9.png)

The output of the **Moore-type** state machine can only change on the clock edge.

The output of a **Mealy** machine can change asynchronously when the inputs change. 

Which of these is desirable depends on the properties of the application.

When designing a FSM, one must:

* Determine how many states are needed and how to enumerate them.
* Determine the sequence of state transitions.
* Write down the truth tables for the combinational logic.
* Implement the logic.

Let's consider simple Moore example - the outputs only change synchronously, i.e. when the clock ticks.

The machine has 1 input (stop) and 3 output (A, B, C).

In our desired machine, only one of the out-puts is ever “on” and with each clock tick, the outputs are cycled in the order A→B→C→A→ · · · , unless stop=1, in which case the output remains constant.

Since, in a Moore machine, the outputs are dependent only on the state, we can ascribe each distinct combination of outputs to a separate state of the state register. We will choose the following:

ABC | State
--- | ---
100 | 00 
010 | 01
001 | 10
000 | 11

Since we have only three desired output states, but four possible register states, there is a spare state (11) to which we have ascribed a “safe” output (000).  In practice though, when we consider how the machine will transit between states, we will make sure that this state is never reachable, but connects to one of the other states, as shown in the state diagram below:

![](https://i.gyazo.com/429bf1fd6ea8c2ae3c04d5a953ae91c3.png)

Since we know the order, we can formulate this in terms of the states: 00→01→10→00→...

To ensure 11 is never reached, we add transition 11→00.

![](https://i.gyazo.com/d6ff84ac6fa42915dae9b49f2c1b93fd.png)

![](https://i.gyazo.com/bffce79f726f091493231e9fe198c595.png)

![](https://i.gyazo.com/0a4dfffbf8b5c6e2a47e9200ad6b78bd.png)


# Interrupts & Peripherals

I/O devices are not an integrated part of the main processor.

It is necessary to provide mechanisms that allow external devices to communicate with the processor, memory, and programmes that they execute.

### I/O Requirements

We need:

1. A communications channel between the processor and the IO device;

2. A way of identifying which I/O device we are communicating with; and

3. A mechanism by which we can determine whether the I/O device requires the processor's attention


Events occur at a much lower frequency that the processor's clock rate; therefore they do not require constant attention.

The communications channel is realised by the bus.

The bus is a collection of wires that carry signals between devices.

The signals down these wires must be multiplexed between various devices, and the bus must also include a set of rules and protocols that determine which devices can send and receive signals.

The bus is time-shared between all the system's devices and the bus controller will arbitrate between the devices to determine which can have access at any given moment.

![](https://i.gyazo.com/ed5edbf26f624e808750a3de39b67bc9.png)


### Memory-mapped vs port-mapped I/O

The bus allows CPU to communicate with peripherals in two ways

#### 1 Memory-mapped

Sends them commands, and send/receive data. This is similar to the way the CPU interacts with memory. Peripherals can be treated similar to memory in what is known as **memory-mapped**.

Each IO device is allocated a region of the address space, i.e. a block of "memory addresses". Each address corresponds to some form of register of buffer on the device. 

It is employed in RISC designs as it allows IO to be implemented with the minimum of additional complexity - accessed by the same mechanisms as memory.


#### 2 Port-mapped

Devices have their own address space instead of sharing the memory's address space. Done by adding a single control wire that indicates whether an address is for memory or IO. Thisis more complex.

Adopted in x86 processors. This family
of devices employ a logically separate I/O bus with its own address space, and dedicated instructions for controlling this bus. 

Physically, the address and I/O buses share the same wires, but are controlled by different instructions and are thus effectively separate buses.

The x86 I/O bus is supported by dedicated I/O instructions.

Recall that to control the memory bus, there is the mov instruction that can access both memory and registers, but mov cannot access the I/O bus. For this, there are the in and out instructions, with in used to read a peripheral’s registers and out used to write to them

Typically a simple peripheral will have a command buffer and a data buffer. In a typical output operation, we will send some data to the data buffer, and then send a command to the command buffer that tells the peripheral what to do with it.

### Polling

It is not necessary to check frequently whether, for example, a key on the keyboard is pressed.

Instead, we can test, or poll the device regularly, but less frequently, allowing the CPU to perform other tasks between polling events.

Is is very simple to do with software but it can still waste a signiﬁcant amount of processor time even if the polling frequency is reduced.

One can of course reduce the frequency even further, but in this case there is a risk that multiple events may occur between polling events, and thus the earlier ones will be missed by the CPU. 

There are wasted clock cycles in any polling based scheme, even if the frequency is correctly chosen there can still be idle period when no input is received.

There may also be problems if a particular I/O device is very slow as the last I/O operation may not have ﬁnished before the next poll.

This approach can be summarised in a diagram:

![](https://i.gyazo.com/ab162ebbdb2361b52361fbbbebfb8abb.png)

Polling gives us a simple software solution to I/O control, but sub-optimal. The main alternative requires some extra hardware complexity, but is much more ﬂexible and efﬁcient.

### Interrupts

Instead of repeatedly probing a device to see when it is ready as we do in the polling scheme, one can conceive an alternative system that includes a mechanism to allow peripheral devices to tell the CPU when they are ready, effectively allowing them to interrupt the current process.

Each device is allocated a special hardware interrupt request channel (IRQ), which can be used to signal to the processor that it needs to pay some attention to the device.

Here is the diagram:

![](https://i.gyazo.com/78615b315cae165511d8cf6267108f2d.png)

Interrupts require some specialist action by the programmer, who must include a handler (subroutine) to deal with interrupt requests (IRQs), and manually initiate I/O by sending appropriate commands to the relevant I/O controller. These routines are usually included in the device driver associated with a particular peripheral.

Interrupt request channels are checked by the CPU each time a new instruction is fetched, to ensure that instructions are never left unﬁnished.

If there is a pending request, the CPU then acknowledges the request via the interrupt channel and disables further interrupts.

The CPU state is saved to the stack and the interrupt-handling subroutine is loaded, temporarily halting the execution of the program that was running. The state of the CPU is retrieved from the stack once the interrupt has been handled so that the main program’s execution can continue.

By this scheme, the CPU does not waste cycles polling the peripherals so in this sense interrupts are a great improvement over polling.

### DMA

Direct Memory Access (DMA) is a method for removing the load of large, monotonous data transfers from the CPU by providing a separate controller for processing certain types of request without using the CPU resource.

![](https://i.gyazo.com/ea84b1172bda6d945d16bcb1be50928f.png)

The DMA module is a mini-processor that specialises in transferring data between (virtual) memory locations which can include memory-mapped or isolated peripherals.

When the CPU needs to read or write a block of data, it issues commands to the DMA controller which consist of:

1. the address of the I/O device
2. the starting location of the data block
3. the number of words of data to be transferred
4. whether the transfer is read or write.

The CPU then returns to the interrupted program, leaving the DMA module to do the memory transfer with the system bus.

The DMA module can control the bus independently of the CPU so can use it when the processor doesn’t need it, or can steal cycles from from CPU by forcing it to suspend bus access. 

When the transfer is complete, the DMA module issues an IRQ to the CPU so that, if necessary, it can make use of any data that has been loaded into memory.

If the system has a cache then some care is required with DMA. In particular, if the cache is write-back, then we need to ensure that the correct version of the data is being copied. In this case, the cache would normally be ﬂushed (any changed entries written back to memory) before the DMA transfer is allowed to start.

# Improving Performance

The basic processor we have designed in MIPS is functional but not optimal. There are techniques we can use to improve performance.

### Caches

The cache is a medium sized, moderately fast memory which acts as a buffer between the main memory and registers.

Virtual memory is the generalisation of the main memory, that includes storage on secondary devices like hard disks.

The advent of multi-chip packages and multi-core devices has led to increasingly large quantities of cache being included in modern designs.

Used to store a copy of some part of the main memory. This can lead to performance improvements because of two basic properties of computer programs:

#### 1 **The principle of spacial locality**

If you have recently referenced an item, it is likely you will want to reference nearby items soon. True for instructions because their sequential execution sequence. True for data because program's data is stored in contiguous memory locations.

#### 2 **The principle of temporal locality**

If you have recently referenced an item, it is likely you will want to reference it again soon. The main constructs that leads to this are loops.

When memory access is made, the cache is first searched.

Over time, the contents of the cache stabilise and fewer requests to the main memory are made.

It is often the case that separate caches for data and instructions are used as they are dealt with separately by the processor. 

It is not sufﬁcient for the cache to simply mirror the contents of the memory: the cache must also know whereabouts in the memory an item came from. 

The cache must therefore be able to store both the item that is being cached, and the address of that item in memory. A cache therefore consist of two blocks of memory, one that stores the item itself, and one storing the address of that item. In the cache, this is normally called the tag.

When request is made to the cache, the memory address being accessed is compared to all of the addresses stored in the cache. If the address is in the cache, the corresponding item in the cache is read/written – a cache hit. Otherwise, main memory must be accessed – a cache miss. 

If the result of the access is a miss, then the access is passed on to main memory. It is not usual to access the cache and memory simultaneously due to the extra memory accesses this would need and the cost of doing so. The addition of a cache to the system is therefore not free: it actually increases the time taken to service a memory access in the case of a cache miss.

#### Associativity

Associativity is the number of different locations an item of data can occupy in the cache.

Most common is direct-mapping which is one-way assoicative. Each address in main memory can only go in 1 spot in the cache.

A subset of the address is hard-coded into the cache so that when an address 101x is presented, it is only necessary to look at one cache entry. The remaining address bits are checked against the tag for that entry.

Main problem with this is trashing: contents continually swapped due to having similar memory addresses. 

The obvious solution to minimise thrashing is to allow each piece of data to be stored in multiple locations in the cache. In an n-way set associative cache, each address in main memory maps to n cache entries.

A smaller portion of the address is hardcoded in to the cache, and the memory address has then to be compared with n entries in the tag. This can be slower and adds complexity due to the need to perform multiple comparisons simultaneously.

The ultimate realisation of an n-way set associative cache is one in which n is the same as the number of locations in the cache, in which case it is said to be fully associative. 

There is no hardwired address mapping, and any item in main memory can go anywhere in the cache. This adds very considerably to the complexity as all of the tag entries have to be compared with the desired address, but in practice this is done using a special type of memory that is content-addressable. 

Its use can greatly reduce the potential for thrashing, at the cost of an increased access time due to the added complexity. 

Performance is gained by having fewer accesses to external memory, but is lost by either increasing the cycle time to accommodate the cache, or by having to allow multiple clock cycles for cache access. 

There are no concrete rules as to which is best and it will usually be necessary to per-form extensive analysis of typical code in simulation in order to determine which design is likely to be best.

#### Refill

 In a direct-mapped cache, new entries can only go to one location and so, when a new item stored into the cache, there is no requirement for any sort of algorithm to determine where in the cache it should be placed. In an n-way associative cache, there are n possible lo-cations that the item could be stored, and it is necessary to decide which one should be replaced.

The simplest strategy is to use the principle of tempo-ral locality to replace the item that has been least-recently-used (LRU) on the grounds that it is less likely to be needed soon. 

How-ever, this can cause problems in loops, especially in cases where the cache is a little smaller than the size of the loop body.

The alternative is random replacement, which does not use any of the principles by which the cache operates, but is far easier to implement.

The designer will have to determine whether increased complexity (which affects the area required and potentially the cycle time) or the increased miss rate is most acceptable.

### Virtual Memory


### Calculating cache benefits

We will make the following assumptions:

1. All instructions can be fetched in a single cycle (removing the need for an instruction cache).
2. All instruction execute in a single cycle (except for those that access memory).
3. There are *N* instruction in a program.
4. A proportion *m* of instructions access memory.
5. The cycle time is given by *t_cyc*.
6. The memory access time (additive to the cycle time) is given by *t_mem*.
7. The cache access time is given by *t_cache*.
8. The cache hit rate is *h*.


### Pipelining: MIPS pipeline, hazards

### Branch Prediction: static, dynamic, speculative execution, delayed branching
