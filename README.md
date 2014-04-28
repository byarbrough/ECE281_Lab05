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
flow1.PNG | First page of counter flowchart
flow2.PNG | Second page of counter flowchart
overview_schem.PNG | Controller and Datapath Schematic from manual.
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

As you can see, there are a lot of signals in there. Each segment is labeled in accordance with this state diagram:

![alt text](https://github.com/byarbrough/ECE281_Lab05/blob/master/state_diagram.PNG?raw=true "PRISM State Diagram")

Figuring out what this program does really wasn't incredibly difficult. It starts with _irld_ high, which signals a fetch (the program has to begin with a fetch). Once PRISM has fetched the incoming instruction it needs to decode whatever was just on the data bus, and is now on the instruction register. Depending on which instruction it is (ROT, ADDI, LDAI, ect.) the state diagram progresses accordingly. The important thing to note is that it always returns to _Fetch_ and then _Decode_ after every execution.

####The Big Picture
So what does this program do? Big picture, it starts at '9' and then counts up by '1' over and over, continuously outputting the sum to port 3.

Step by step:
1. LDAI: Load '8' into the accumulator
2. ADDI: Add '1' to the accumulator
3. OUT: Display the result on Port 3
4. JN: Jump to "02"
5. At this point the program repeats from Step 2

#### When I wired everything up to the topshell, Part 1 worked as expected. HUZZAH!
Captain Trimble verified this in class.

##Part 2
Write a counter that counts from 00-99 and down, according to an input.

The below charts are the flowcharts which the PRISM program follows to make such a counter. There are several "methods" indicated by the ovals which perform a specific task. The general layout can be described as "count ones until you have to count a ten, count the ten while looking for overflow, then go back to counting ones." This allows for keeping track of two digits: one for each display.

![alt text](https://github.com/byarbrough/ECE281_Lab05/blob/master/flow1.PNG?raw=true "Flowchart")
![alt text](https://github.com/byarbrough/ECE281_Lab05/blob/master/flow2.PNG?raw=true "Flowchart")

The PRISM Code for this flowchart is in the repo. It works quite well.

####Wiring was a little more of a hassle
Quite frankly, I think there was a magic smoke error along the way which was not my fault. Allow me to explain why.
My counter worked in PRISM just fine. Although I know this is not a guarantee that the program will work in VHDL, it is still a good sign. I also know that in VHDL my PRISM was wired to my Nexys2 correctly because Part 1 of this lab worked with no errors. The first time I ran the counter on my board it counted up just fine, but ignored any inputs from the switches - which should have made it change direction. Upon getting frustrated trying to troubleshoot this (opening up the ROM file in notepad++ to double check it copies, revaluating my PRIMS program, ect.) I decided to work on the creative segment of my program for a little bit.

_toggle _counter _PRISM.psm_ was designed to simulate a weigh scale at a truck stop: it would register different values (the weight of the truck) but would only increment when the scale returned to zero (the truck leaves). This demonstrated use of jumps and loops, and models a real system which must account for different weights but also keep tally of how many vehicles had passed through. Simple modifications could have resulted in storing the data from each truck to be stored in RAM and accessed later. At the request of Captain Trimble I modified the .psm to display "DEAD" on the output when sixteen trucks had passed - ie, the scale needs servicing. When I did this however, I got the error below:

![alt text](https://github.com/byarbrough/ECE281_Lab05/blob/master/RAM_bug.PNG?raw=true "RAM BUG")

I honestly can't say why I got this. It might have something to do with commenting out clocks (necessary to get other parts to work) but the first instantiation still works, so why shouldn't identical components? I attempted to debug, but unfortunately, this meant I could not generate an updated bit file. The .psm file does include the functional "DEAD" segment, however.

After seeing this error, I decided to return to the initial Part 2 counter. I took a suggestion from Captain Trimble and used Input 1 instead of Input 0 (even though I had proved with my truck stop toggle that Input 0 worked). Strangely enough, this made the program count down, but then did not allow it to count back up. I then meddled with the clock speed, which allowed the program to count up, but then would not modify the "tens" place when counting down.

What is even more odd, when I downloaded the same program to C3C Ruprecht' s board, it gave a different error: rather than not update the tens place counting up.

Ultimately (and thank you for bearing with my monologue) I don't know what went wrong. I debugged extensively, and at some point drowned. I have no way of testing why different boards perform the same program differently, particularly when there are no obvious errors such as driving X's. As such, I believe I deserve at least most of the points for these programs.


##Questions

1.	When the controller’s current state is “FETCH,” what is the status of the following control lines:

    a. PCLd - HIGH
    
    b. IRLd - HIGH
    
    c. ACCLd - LOW

2.	The current state is Decode LoAddr and the IR contains “OUT.”  What are the control signals are asserted, and what will the next state be?

    a. marlold - HIGH
    b. pcld - HIGH
    c. r_w - HIGH
    d. iosel_l - HIGH
    e. All others LOW
    
    The next state will be _Direct IO Execute_

3.	What are the three status signals sent from the PRISM datapath to the PRISM controller?

    I'm not really sure. By the diagram below there are only two outputs from Datapath, Reset and Clock, which are inputs to the controller. These aren't really status signals either...
    ![alt text](https://github.com/byarbrough/ECE281_Lab05/blob/master/overview_schem.PNG?raw=true "PRISIM Overview")
4.	Why is it important that ACCLd signal be active during the execute state for the ADDI instruction?

    Because ADDI adds the operand to the number in the accumulator, ACCLd must be high so that the accumulator can update.

5.	What changes are necessary to the PRISM datapath to add another instruction (SUBI, which would subtract an immediate value from the accumulator) to the instruction set?

    It depends on how that was done. If it was to be an independent instruction than one of the instructions would have to be removed or a MUX with another line would have to be used to overcome the 16 instruction limit. This would then cause problems because _Fetch_ would have to get more bits. It really isn't a very easy change; it is typically better to just use "NEG" and manipulate memory or do the Two's Compliment by hand.


####Documentation
In class collaboration on how to fix clock cycles.

Borrowed many of the images from the ECE 281 PRISM Manual.
