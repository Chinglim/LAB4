LAB4
====
#Design 
Main purpose of this lab is to build/ create an ALU and construct its data path.
##1)Discussion of ALU Modification

This ALU is just simply a multiplexer,that is required to perform 8 different functions: AND,NEG,NOT,ROR,OR,IN,ADD,LD.
The execution of these functions are based on a 3 bits OpSel input and the data path. The necessary design output will then be generated, through the given input and selected functions.

##2)ALU Test and Debugging
One issue that i faced was on the ROR command function, it was not functioning, till I decided to just write out the step by step , bit by bit execution method of the ROR command function. I decided to design the ALU using the conditional statements,given that the ALU seemed to be a multiplexer and I had previously used conditional statements codes before in Lab3 and find it more comfortable to work around with.

As attached in this git repository is the waveform generated after simulating the ALU_shell.vhd in the ALU_testbench.vhd file, it will be labelled as ALU_testbench_Waveform.jpeg.

Simulated Waveform: 

![ALU testbench Waveform](ALU_testbench_Waveform.jpg)


Cross-checking with the waveform and the Conditional Statements coding within the ALU_shell.vhd, the values within the waveform picture protrayed the correct desired result value of the respective assigned functions.

##3)Discussion of Datapath Modifications
Programme Controller was done by the kindness of Cpt Sliva.

For the Instruction Register(IR), as stated in the comment, its reset low is asychronous.If the reset is asynchronous, the reset statement have to be run before the clock statement, as shown below.
 
 process(Clock,Reset_L)
  	begin
 if(Reset_L= '0') then       (Reset statement)
			IR<="0000";
 elsif(Clock'event and Clock ='1')then    (Clock Statement)
	  
Furthermore with the knowledge from the class, IR will only be active when IRLd is '1', thus allow the 4 bits Data Bus input to be output on the IR output.

MARHi and MARLo is somehow similar to the operation of the IR as in that the Data bus inputs will only passes through them when the MARHi/ MARLo Ld is '1', and output via the respective MARHi/MARLo output. 

As for Address Selector (multiplexer), as based on the knowledge obtained through the lessons in class,Address Selector (multiplexer) is not sensitive to clock input. Furthermore, due to its 8 bits output, when the addrsel is '1' , it allows the 2 4 bits MARHi and MARLo values to be passed through. Whereas, when addrsel is '0', it will then select the 8 bits PC value as output.

Furthermore, as based on the lesson taught in class, the Accumulator only allow the 4 bits ALU output to pass through if the ACCLD is active ,'1'.and only when the ENAccB, which is the input of a tristate buffer, is '1' then can the value of the accumulator be transferred to the data bus.

##4)Datapath Test and Debug
During the whole process, mainly syntax error like use of wrong " " or ' ' , forgetting to end of certain function properly and semicolons too.

Nonetheless, managed to find those along the way and got it fixed. The first 50ns waveform diagram is similar to the one as requested on the labsheet and the value indeed changes @ 225ns.




##5)Simulation Analysis
With the ALU and datapath, they create the basics of the PRISM program. I believe a controller is the device needed to the Datapath program to create the microprocessor in VHDL.

For the waveform ranging from 0-50ns,

![Simulation waveform 1](0till50ns.jpg)

1)From 0-10 ns,Reset_l is set low to initialize everything to zeros.Therefore the first instruction is found in the read-only memory at address 00(hexadecimal).

2)The controller will retrieve the data from 10-26 ns and places it on the data bus

3)At the next cycle, the data from the data bus is then uploaded into the Instruction Register.

4)As 7 is the opcode for LDAI, the contorller then retrieves the operand from memory(and places it on the data bus)

5)So that at the next cycle, it can be loaded into the Accumulator.

6) This meant that the first instruction is a LDAI 'B'(hexadecimal).



For the waveform ranging from 50-100ns,

1)The controller retrieves the new data and places it on the data bus.

2)So that then at next cycle it can be upload into the Instruction Register.

3)As 3 is the opcode for ROR,the ROR is carried out and as 4 is the opcode for out, the ROR value of 'B' (hexadecimal): D(hexadecimal) is then displayed on the the accumulator.

4)However, cannot proceed immediately as now the enaccbuffer is low and thus does not allow value 'D' (hexadecimal)from the accumulator to the data bus.

5) This meant that the second instruction is is ROR '3', followed by OUT '4' .

For the waveform ranging from 100-150ns,

1)The value 'D' is sent to the data bus as the enaccbuffer is now active and allow the accumulator to send value to the data bus. The value will then be outputed to port 3.

2)

3)

4)

