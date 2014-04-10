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

2) After output to port 3, the program listing should have a space before next operation as indicated by the '0' in the data bus, which is then being transferred to the IR to be carried out. 

3)The controller retrieve this data d(hexadecimal)and place it on the data bus, so that at the next cycle , it is then loaded into the Instruction Register. 

For the waveform ranging from 150-200ns,

1)Since 'd' is the opcode for STA, the IR will carry the store operation.

2)The controller then retrieves the operand address , where the accumulator content is going to be stored into.

3)So that in the next cycle, the accumulator content is then stored into b0, when the enaccbuffer is active.

4)The controller retrieve the data 'b'(hexadecimal) and place it on the data bus, so that in the next cycle, it is then loaded into the Instruction Register.



For the waveform ranging from 200-250ns,
1)Since B(hexadecimal) is the opcode for JN, which meant jumping to the operand address if accumulator has a negative number.
2) Since the value of the accumulator is ' D' (hexadecimal) and its first bit is a one, it is negative.Thus jump operation will be carried out.

3)The controller then retrieves the data and place it on the data bus so that at the next cycle, it can then be loaded to the Instruction Register. The data is '3' which is the opcode for ROR.

4) Therefore that will meant that the Jump fuction is being assigned to jump back to the ROR step.



For the waveform ranging from 250-300ns,
1) As stated previously, the instruction register will carry out the ROR operation, the new value in the accumulator will be changed from 'D' (hexadecimal) to 'E' (hexadecimal).

2)However, cannot proceed immediately as now the enaccbuffer is low and thus does not allow value 'E' (hexadecimal)from the accumulator to the data bus.

3)The controller retrieve this data '4'(hexadecimal)and place it on the data bus, so that at the next cycle , it can then then loaded into the Instruction Register. 

4)The IR then run the output operation to display 'E' at output 3 instead and as now the enaccbuffer is active, the accumulator value 'E' has been send to the data bus too.



For the waveform ranging from 300-350 ns,

1)As stated earlier as 4(hexadecimal) is the opcode for OUT , the value 'E' is then output on port 3 and also concurrently as the enaccbuffer is active, the accumulator value 'E' is also send to the data bus.


2)The controller retrieve this data d(hexadecimal)and place it on the data bus, so that at the next cycle , it is then loaded into the Instruction Register. 


For the waveform from 350-400 ns,

1)In IR, the value 'D' hexadecimal is seen, it is the opcode for STA. It means storing the contents of the accumulator into the operand address.


2)The controller then retrieve the operand address where the new value is to be store and places it on the data bus.

3)The value 'E' hexadecimal is then stored in memory location B0 and the data bus when the enaccbuffer is active.

4))The controller retrieve this data b(hexadecimal)and place it on the data bus, so that at the next cycle , it is then loaded into the Instruction Register. 



For the waveform from 400-450ns,

1) 'B' hexadecimal is the opcode for jmp if the accumulator value is negative. Since 'E' (hexadecimal)first bit is 1 which means that the sign bit is negative, thus the jmp operation will be carried out

2)The controller retrieve this data '3'(hexadecimal)and place it on the data bus, so that at the next cycle , it is then loaded into the Instruction Register.  



For the waveform from 450-500ns,

1) '3' (hexadecimal) is the opcode for ROR, it is also within the Instruction Register thus the ROR operation will be carried out and the value in the accumulator can then be changed in the next cycle from 'E' to '7'(hexadecimal).

2)The controller retrieve this data '4'(hexadecimal)and place it on the data bus, so that at the next cycle , it is then loaded into the Instruction Register. 

3)Since '4'(hexadecimal) is the opcode for output, the controller then retrieve the selected output port number from the memory and place it on the data bus.

4) At the next cycle the accumulator value '7' get output on output port 3 and to the data bus as the enaccbuffer is now active too.



For the waveform from 500-550 ns,

1)The controller retrieve this data 'd'(hexadecimal)and place it on the data bus, so that at the next cycle , it is then loaded into the Instruction Register. 

2) Since 'd' is the opcode for STA, the controller then retrieve the operand address 'B0' and places it to the data bus.


For the waveform from 550-600ns,

1)The controller retrieve this data 'b'(hexadecimal)and place it on the data bus, so that at the next cycle , it is then loaded into the Instruction Register. 

2) Since 'b' (hexadecimal) is the JN fucntion, and the accumulator value is 7 now, it is not negative as it sign bit is not '1', therefore the JN function will not run anymore.


For the waveform from 600-650ns,

1)The controller retrieve this data '9'(hexadecimal)and place it on the data bus, so that at the next cycle , it is then loaded into the Instruction Register

2) Since '9' (hexadecimal) is the JMP fucntion, and the controller then retrieve the operand address '0C' and places it to the data bus.


For the waveform from 650-700ns,

Since there was no more instructions, there was no more changes on the data bus.
