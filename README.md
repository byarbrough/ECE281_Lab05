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
toggleCount.bit | Bit file for creative segment
toggle_counter _PRISM.psm | PRISM file for creative segment



##Part 1

###Testbench - The Initial Program

Here is the initial testbench simulation:

![alt text](https://github.com/byarbrough/ECE281_Lab05/blob/master/sim_0_to_165ns.PNG?raw=true "PRISM Testbench")

As you can see, there are a lot of signals in there.

####The Big Picture
So what does this program do?

###Controller State Walkthrough


#### When I wired everything up to the topshell, Part 1 worked as expected. HUZZAH!

##Part 2

####This was a little more of a hassel

##Questions
I know this is a boring, uncreative way of doing this, but it's project season:

1.	When the controller’s current state is “FETCH,” what is the status of the following control lines:

    a. PCLd - 
    
    b. IRLd - 
    
    c. ACCLd - 

2.	The current state is Decode LoAddr and the IR contains “OUT.”  What are the control signals are asserted, and what will the next state be?







3.	What are the three status signals sent from the PRISM datapath to the PRISM controller?







4.	Why is it important that ACCLd signal be active during the execute state for the ADDI instruction?







5.	What changes are necessary to the PRISM datapath to add another instruction (SUBI, which would subtract an immediate value from the accumulator) to the instruction set?



