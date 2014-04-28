ECE281_Lab05
============

More PRISM!

| File Name | Description |
------------|------------
Nexys2_top _shell.vhd | Top level file, interacts with board
PRISM.vhd | Interaction between PRISM code and Nexys2
Part2_PRISM.psm | PRISM code for the second part of the project: the counter
RAM_bug.PNG | Screenshot of VHDL error
ROM_176x4.vhd | Original ROM file
ROM_176x4 _Count.vhd | ROM for part2
ROM_176x4 _Toggle.vhd | ROM for the creative segment
part2_counter.bit | Bit file for Part 2
part1_bit | Bit file for part 1
sim_0 _to _ 165ns.PNG | Screenshot of Part 1 running testbench
state_diagram.PNG | State Diagram for PRISM. From Manual.
toggleCount.bit | Bit file for creative segment
toggle_counter _PRISM.psm | PRISM file for creative segment



##Part 1

###Testbench - The Initial Program

Here is the initial testbench simulation:

![alt text](https://github.com/byarbrough/ECE281_Lab05/blob/master/sim_0_to_165ns.PNG?raw=true "PRISM Testbench")

As you can see, there are a lot of signals in there. Each segment is labled in accordance with this state diagram:

![alt text](https://github.com/byarbrough/ECE281_Lab05/blob/master/state_diagram.PNG?raw=true "PRISM State Diagram")

Figuring out what this prgram does really wasn't incredibly difficult. It starts with _irld_ high, which signals a fetch (the program has to begin with a fetch). Once PRISM has fetched the incoming instruction it needs to decode whatever was just on the data bus, and is now on the instruction register. Depending on which instruction it is (ROT, ADDI, LDAI, ect.) the state diagram progresses accordingly. The important thing to note is that it always returns to _Fetch_ and then _Decode_ after every execution.

####The Big Picture
So what does this program do? Big picture, it starts at '9' and then coutns up by '1' over and over, continuously outputting the sum to port 3.

Step by step:
1. LDAI: Load '8' into the accumulator
2. ADDI: Add '1' to the accumulator
3. OUT: Display the result on Port 3
4. JN: Jump to "02"
5. At this point the program repeats from Step 2

###Controller State Walkthrough


#### When I wired everything up to the topshell, Part 1 worked as expected. HUZZAH!

##Part 2

####This was a little more of a hassel

##Questions

1.	When the controller’s current state is “FETCH,” what is the status of the following control lines:

    a. PCLd - HIGH
    
    b. IRLd - HIGH
    
    c. ACCLd - 

2.	The current state is Decode LoAddr and the IR contains “OUT.”  What are the control signals are asserted, and what will the next state be?







3.	What are the three status signals sent from the PRISM datapath to the PRISM controller?







4.	Why is it important that ACCLd signal be active during the execute state for the ADDI instruction?







5.	What changes are necessary to the PRISM datapath to add another instruction (SUBI, which would subtract an immediate value from the accumulator) to the instruction set?



