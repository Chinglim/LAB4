LAB4
====
#Design 
Main purpose of this lab is to build/ create an ALU and construct its data path.
##1)Discussion of ALU Modification
The ALU is required to perform 8 different functions: AND,NEG,NOT,ROR,OR,IN,ADD,LD.
The execution of these functions are based on a 3 bits OpSel input and the data path. The necessary design output will then be generated, through the given input and selected functions.

##2)ALU Test and Debugging
One issue that i faced was on the ROR command function, it was not functioning, till I decided to just write out the step by step , bit by bit execution method of the ROR command function. I decided to design the ALU using the conditional statements, as I had previously used it before in Lab3 and find it more comfortable to work around with.

As attached below is the waveform generated after simulating the ALU_shell.vhd in the ALU_testbench.vhd file

Image:
![ALU_Testbench Waveform](ALU _testbench_Waveform.jpg)
