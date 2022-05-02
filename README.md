# RTL-design-using-Verilog-with-SKY130-Technology
![Verilog-flyer](https://user-images.githubusercontent.com/104454253/166084640-128e6351-1739-4b38-a3ce-76459da921b5.png)
# Table of contents
 - [1. Introduction](#1-Introduction)
 - [2. Day-1- Introduction to Verilog RTL design and Synthesis](#2.-day-1---introduction-to-verilog-rtl-design-and-synthesis)
    - [2.1 Various aspects of frontend design](#21-Various-aspects-of-frontend-design)
    - [2.2 Introduction to open source simulator iverilog and gtkwave](#22-Introduction-to-open-source-simulator-iverilog-and-gtkwave)
     	- [2.2.1 Lab examples using iverilog and gtkwave](#221-Lab-examples-using-iverilog-and-gtkwave)
    - [2.3 Introduction to Yosys simulator](#23-Introduction-to-Yosys-simulator)
        - [2.3.1 Labs on Yosys introduction](#231-Labs-on-Yosys-introduction)
 - [3. DAY2-Timing libs, hierarchical, flat synthesis, efficient flop coding styles](#3.-DAY2-Timing-libs,-hierarchical,-flat-synthesis,-efficient-flop-coding-styles)
    - [3.1 Introduction to timing .libs](#31-Introduction-to-timing-.libs)
        - [3.1.1 LAB- Introduction to dot Lib](#311-LAB--Introduction-to-dot-Lib)
        - [3.1.2 LAB Hierarchical synthesis and flat synthesis](#312-LAB-Hierarchical-synthesis-and-flat-synthesis)
        - [3.1.3 Various Flop coding styles and optimization](#313-Various-Flop-coding-styles-and-optimization)
            - [3.1.3.1 Lab flop synthesis simulations](#3131-Lab-flop-synthesis-simulations)
            - [3.1.3.2 Interesting optimisations](#3132-Interesting-optimisations)
 - [4. Day3- Combinational and sequential optmizations](#4.-Day3--Combinational-and-sequential-optmizations)
    - [4.1 Combinational logic optimization with examples](#41-Combinational-logic-optimization-with-examples)
    - [4.2 Sequential logic optimization with examples](#41-Sequential-logic-optimization-with-examples)
        - [4.2.1 Basic](#421-Basic)
        - [4.2.2 Advanced](#422-Advanced)
# 1. Introduction
This report is a final submission of 5-day workshop from [VLSI Sytem Design-IAT](https://www.vlsisystemdesign.com/) on RTL design and synthesis using open source tools, in particular iVerilog, GTKWave, Yosy and Skywater 130nm Standard Cell Libraries  
# 2. Introduction to Verilog RTL design and Synthesis
## 2.1 Various aspects of frontend design
**RTL Design**: In simple terms RTL design or Register Transfer Level design is a method in which we can transfer data from one register to another. In RTL design we write code for Combinational and Sequential circuits in HDL(Hardware Description Language) like Verilog or VerilogHDL which can model logical and hardware operation. RTL design can be one code or set of verilog codes. **One key note is that we need to write RTL design with optimized and synthesizable (realizable as physical gates)**.

**Sample RTL design outline:**<br />
	module module_name (port list);<br />
		//declarations;<br />
		//initializations;<br />
		//continuos concurrent assigments;<br />
		//procedural blocks;<br />
	endmodule<br />

**Test Bench**: Using Verilog we can write a test bench to apply stimulus to the RTL design and verify the results of the design by instantiating design with in test bench. Up-front verification becomes very important as design size increases in size and complexity while any project progresses. This ensures simulation results matches with post synthesis results. A test bench can have two parts, the one generates input signals for the model to be tested while the other part checks the output signals from the design under test. It can be represented as follows.
![Capture2](https://user-images.githubusercontent.com/104454253/166088950-634be5a4-7d5a-4b43-9990-711f8f660aaf.JPG)

**Simulation**: RTL design is checked for adherence to its design specification using simulation by giving sample inputs. This helps finding and fixing bugs in the RTL design in the early stages of design development. 

**Simulator**: Simulator is the tool used for this process. It looks for changes on input signals to evaluate outputs. No change in output if there is no change in input signals
Here is the flow of frondend design:

![Capture1](https://user-images.githubusercontent.com/104454253/166088866-80a4e792-7db7-4bf2-b3b5-b4b9b92452a8.JPG)

## 2.2 Introduction to open source simulator iverilog and gtkwave
**iverilog**: iverilog stands for Icarus Verilog. Icarus Verilog is an implementation of the Verilog hardware description language. It supports the 1995, 2001 and 2005 versions of the standard, portions of SystemVerilog, and some extensions.

**Gtkwave**: GTKWave is a fully featured GTK+ based wave viewer for Unix, Win32, and Mac OSX which reads LXT, LXT2, VZT, FST, and GHW files as well as standard Verilog VCD/EVCD files and allows their viewing. 

### 2.2.1 Lab examples using iverilog and gtkwave

We were introducted to Linux operating system and were made aware of the basic commands. Using **git clone** command we've cloned library files like standard cell library, primitives which are used for synthesis and few verilog codes for practice.

![cloning files](https://user-images.githubusercontent.com/104454253/166092143-aa26356f-46a3-435b-b3d6-0d2fb4d216c2.JPG)

![cloning_githubfiles](https://user-images.githubusercontent.com/104454253/166092154-d93b2204-88e3-4848-ae98-ff5114ea2afe.JPG)

![verilog codes](https://user-images.githubusercontent.com/104454253/166092156-fab94f09-c0a1-4ee3-af24-046fd6aef2d5.JPG)

In this session, I've performed simulation of multiplexer. I've added both the RTL design code and test bench code in iverilog to generate vcd file which I used in gtkwave generator to get the output waveformes after simulation. The output was generated by taking the inputs from the testbench code. 

![muxcommands](https://user-images.githubusercontent.com/104454253/166117682-8c5149a4-8df9-456c-8e9a-744d6693243d.JPG)

Here is the code and gtkwave snippets:<br />
	module good_mux (input i0 , input i1 , input sel , output reg y); <br />
		always @ (*)<br />
		begin<br />
			if(sel)<br />
			y <= i1;<br />
			else <br />
			y <= i0;<br />
		end<br />
	endmodule**<br />


	`timescale 1ns / 1ps<br />
	module tb_good_mux;<br />
	// Inputs<br />
	reg i0,i1,sel;<br />
	// Outputs<br />
	wire y;<br />
      		// Instantiate the Unit Under Test (UUT), name based instantiation<br />
		good_mux uut (.sel(sel),.i0(i0),.i1(i1),.y(y));<br />
		//good_mux uut (sel,i0,i1,y);  //order based instantiation<br />
	initial begin<br />
		$dumpfile("tb_good_mux.vcd");<br />
		$dumpvars(0,tb_good_mux);<br />
		// Initialize Inputs<br />
		sel = 0;<br />
		i0 = 0;<br />
		i1 = 0;<br />
		#300 $finish;<br />
	end<br />
	always #75 sel = ~sel;<br />
	always #10 i0 = ~i0;<br />
	always #55 i1 = ~i1;<br />
	endmodule**<br />

![goodmuxgtkwaveCapture](https://user-images.githubusercontent.com/104454253/166117453-7f4918e9-acb4-4ad5-b35a-0933c8578312.JPG)


## 2.3 Introduction to Yosys syntheseizer

**Synthesis**: Synthesis transforms the simple RTL design into a gate-level netlist with all the constraints as specified by the designer. In simple language, Synthesis is a process that converts the abstract form of design to a properly implemented chip in terms of logic gates.

Synthesis takes place in multiple steps:
- Converting RTL into simple logic gates.
- Mapping those gates to actual technology-dependent logic gates available in the technology libraries.
- Optimizing the mapped netlist keeping the constraints set by the designer intact.

**Synthesizer**: It is a tool we use to convert out RTL design code to netlist. Yosys is the tool I've used in this workshop.
Here is the flow of above processess.

![rtl-netlist](https://user-images.githubusercontent.com/104454253/166097298-41d913ee-640d-4e1e-9e70-5bf427f35ef4.JPG)

**Yosys**:Yosys is a framework for RTL synthesis and more. It currently has extensive Verilog-2005 support and provides a basic set of synthesis algorithms for various application domains. Yosys is the core component of most our implementation and verification flows.

I was given an overview of the operation of the tool and the files we'll need to provide the tool to give the required netlist. We give RTL design code, .lib file which has all the building blocks of the netlist. Using these two files, Yosys synthesizer generates a netlist file. .lib basically is a collection of logical modules like, And, Or, Not etc.... These are equivalent gate level representation of the RTL code. 

Below are the commands to perform above synthesis.

RTL Design  - read_verilog
.lib        - read_liberty
netlist file- write_verilog

**Operational flow of Yosys Synthesizer**

![Synthesizer](https://user-images.githubusercontent.com/104454253/166094901-27c70c0d-8ef2-4a34-a4b2-7307af492698.JPG)

**Verification of Synthesized design**: In order to make sure that there are no errors in the netlist, we'll have to verify the synthesized circuit. The netlist verification flow can be seen in the below image:

![Synthesisgtkwave](https://user-images.githubusercontent.com/104454253/166095185-f82dbbe0-afb4-43ac-8ec6-6b75491d6b58.JPG)

The gtkwave output for the netlist should match the output waveform for the RTL design file. As netlist and design code have same set of inputs and outputs, we can use the same testbench and compare the waveforms.

**Introduction to loigc synthesis**: Below is the snippet RTL code and equivalent digital circuit:

![sample rtl](https://user-images.githubusercontent.com/104454253/166097112-0fb5685c-fe88-4ca0-8ecf-bc014de46088.JPG)

In the above image, mapping of code and digital circuit is done using Synthesis.

**.lib**: It is a collection of logical modules like, And, Or, Not etc...It has different flvors of same gate like 2 input AND gate, 3 input AND gate etc... with different performace speed.

**Need for different flavours of gate**: In order to make a faster circuit, the clock frequency should be high. For that the time period of the clock should be as low as possible. However, in a sequential circuit, clock period depends on three factors so that data is not lost or to be glitch free.

For the below circuit the three factors are
- Clock to Q of flipflop A
- Propagation delay of combinational circuit
- Setuptime of flipflop B
![Timedelay circuit](https://user-images.githubusercontent.com/104454253/166098730-33bf0734-abec-466f-abe2-a2ac6813b5e0.JPG)

The equation is as follows

![Time](https://user-images.githubusercontent.com/104454253/166097710-2c1099e3-6323-496c-8eb7-12ee04c12096.JPG)

As per the above equation, for a smaller propagation delay, we need faster cells.
But again, why do we have faster cells? This is to ensure that there are no HOLD time violations at B flipflop.
**This complete collection forms .lib**

**Faster Cells vs Slower Cells**: 
Load in digital circuit is of **Capacitence**. Faster the charging or dicharging of capacitance, lesser is the celll delay. However, for a quick charge/ discharge of capacitor, we need transistors capable of sourcing more current i.e, we need WIDE TRANSISTORS. 

Wider transistors have lesser delay but consume more area and power. Narrow transistors are other way around. Faster cells come with a cost of area and power.

**Selection of the Cells**: We'll need to guide the Synthesizer to choose the flavour of cells that is optimum for implementation of logic circuit. Keeping in view of previous observations of faster vs slower cells,to avoid hold time violations, larger circuits, sluggish circuits, we offer guidance to synthesizer in the form of **Constraints**.

Below is an illustration of Synthesis.

![Screenshot (44)](https://user-images.githubusercontent.com/104454253/166099264-e3842e91-1a27-44ae-830c-0757dc5b1a5e.png)

### 2.3.1 Labs on Yosys introduction

Invoking Yosys:

![invoking yosys](https://user-images.githubusercontent.com/104454253/166099491-8ee3ad06-1b1e-4483-9451-2372f08aab9b.JPG)

Snippet below illustrates reading .lib, design and choosing the module to synthesize:

![yosys1](https://user-images.githubusercontent.com/104454253/166100005-8a2e45e9-2977-4743-b475-515996a046d9.JPG)

**Generating Netlist**: The logic of good_mux will be realizable using gates in the sky130_fd_sc_hd__tt_025C_1v80.lib file.

![yosys2](https://user-images.githubusercontent.com/104454253/166100160-7b8c5847-62b4-49e9-9a90-c4f4c8ff840f.JPG)

Below is the snippet showing the synthesis results and synthesized circuit for multiplexer.

![Yosys3](https://user-images.githubusercontent.com/104454253/166176463-19a38375-d3e8-42b4-8de1-65aaa3ecb35e.JPG)

**Netlist Code**

![yosys4](https://user-images.githubusercontent.com/104454253/166101176-50cf9b44-3ca7-4d1c-bb2e-392498d82ddd.JPG)

**Simplified netlist code**: This code consisits of additional switch. To further simplify, we use below command

![Yosys5](https://user-images.githubusercontent.com/104454253/166101451-5d963a17-4cb7-47fd-818d-c7d34df2665c.JPG)

# 3. DAY2-Timing libs, hierarchical, flat synthesis, efficient flop coding styles

## 3.1 Introduction to timing .libs
### 3.1.1 LAB- Introduction to dot Lib

This lab guides us through the .lib files where we have all the gates coded in. According to the below parameters the libraries will be characterized to model the variations.

![lib1](https://user-images.githubusercontent.com/104454253/166105787-19a638a3-fe01-4fcf-828d-0b56a6acb8f7.JPG)

With in the lib file, the gates are delared as follows to meet the variations due to process, temperatures and voltages.

![lib3](https://user-images.githubusercontent.com/104454253/166106366-2368d29d-d79d-41e1-880d-1bfbdd4e9b2d.JPG)

For the above example, for all the 32 cominations i.e 2^5 (5 is no.of variables), the delay, power and all the related parameters for each gate are mentioned.

![lib4](https://user-images.githubusercontent.com/104454253/166106592-cd478c97-95de-4513-be9f-84a34fc966ca.JPG)

This image displays the power consumtion comparision.

![lib5](https://user-images.githubusercontent.com/104454253/166107259-6fa398a4-2099-4da3-9b93-818c2c3f2404.JPG)

Below image is the delay order for the different flavor of gates.

![delay_libraries](https://user-images.githubusercontent.com/104454253/166187423-d21465e1-abc3-4ad0-a534-60f8e706ab6f.JPG)

### 3.1.2 LAB Hierarchical synthesis and flat synthesis

**multiple_module**
	module sub_module2 (input a, input b, output y);<br />
		assign y = a | b;<br />
	endmodule<br />

	module sub_module1 (input a, input b, output y);<br />
		assign y = a&b;<br />
	endmodule<br />


	module multiple_modules (input a, input b, input c , output y);<br />
	wire net1;<br />
	sub_module1 u1(.a(a),.b(b),.y(net1));  //net1 = a&b<br />
	sub_module2 u2(.a(net1),.b(c),.y(y));  //y = net1|c ,ie y = a&b + c;<br />
	endmodule<br />

This is the schematic as per the connections in the above module.

![94d6174a-bcb0-4cad-bef6-a0537ef32ef1](https://user-images.githubusercontent.com/104454253/166108402-f96b7fa9-3d8f-4f05-b4cb-89e443e43718.jpg)

However, the yosys synthesizer generates the following schematic instead of the above one and with in the submodules, the connections are made

![lab5](https://user-images.githubusercontent.com/104454253/166108721-0e13ddc4-6c31-45bb-b735-560646e7497e.JPG)

The synthesizer considers the module hierarcy and does the mapping accordting to instantiation. Here is the hierarchical netlist code for the  multiple_modules:

	module multiple_modules(a, b, c, y); <br />
		  input a;<br />
 		 input b;<br />
 		 input c;<br />
		  wire net1;<br />
 		 output y;<br />
 	  sub_module1 u1 (.a(a),.b(b),.y(net1) );<br />
	  sub_module2 u2 (.a(net1),.b(c),.y(y));<br />
	endmodule<br />
	
	module sub_module1(a, b, y);<br />
 	 wire _0_;<br />
 	 wire _1_;<br />
 	 wire _2_;<br />
 	 input a;<br />
 	 input b;<br />
 	 output y;<br />
 	 sky130_fd_sc_hd__and2_0 _3_ (.A(_1_),.B(_0_),.X(_2_));<br />
 	 assign _1_ = b;<br />
 	 assign _0_ = a;<br />
 	 assign y = _2_;<br />
	endmodule<br />

	module sub_module2(a, b, y);<br />
  	wire _0_;<br />
 	 wire _1_;<br />
 	 wire _2_;<br />
  	input a;<br />
  	input b;<br />
 	 output y;<br />
 	 sky130_fd_sc_hd__lpflow_inputiso1p_1 _3_ (.A(_1_),.SLEEP(_0_),.X(_2_) );<br />
 	 assign _1_ = b;<br />
 	 assign _0_ = a;<br />
 	 assign y = _2_;<br />
	endmodule**<br />

Flattened netlist:

In flattened netlist, the hierarcies are flattend out and there is single module i.e, gates are instantiated directly instead of sub_modules. Here is the flattened netlist code for the  multiple_modules:

	module multiple_modules(a, b, c, y);<br />
 		 wire _0_;<br />
  		 wire _1_;<br />
 		 wire _2_;<br />
 		 wire _3_;<br />
		 wire _4_;<br />
		 wire _5_;<br />
 		 input a;<br />
 		 input b;<br />
 		 input c;<br />
 		 wire net1;<br />
 		 wire \u1.a ;<br />
		 wire \u1.b ;<br />
		 wire \u1.y ;<br />
		 wire \u2.a ;<br />
		 wire \u2.b ;<br />
 		 wire \u2.y ;<br />
  		output y;<br />
 		 sky130_fd_sc_hd__and2_0 _6_ (<br />
  		  .A(_1_),<br />
  		 .B(_0_),<br />
   		 .X(_2_)<br />
  		);<br />
 		 sky130_fd_sc_hd__lpflow_inputiso1p_1 _7_ (<br />
  		  .A(_4_),<br />
 		  .SLEEP(_3_),<br />
  		  .X(_5_)<br />
 		 );<br />
 		 assign _4_ = \u2.b ;<br />
 		 assign _3_ = \u2.a ;<br />
 		 assign \u2.y  = _5_;<br />
 		 assign \u2.a  = net1;<br />
		 assign \u2.b  = c;<br />
 		 assign y = \u2.y ;<br />
		 assign _1_ = \u1.b ;<br />
		 assign _0_ = \u1.a ;<br />
		 assign \u1.y  = _2_;<br />
		 assign \u1.a  = a;<br />
		 assign \u1.b  = b;<br />
 		 assign net1 = \u1.y ;<br />
		endmodule**<br />

The commands to get the hierarchical and flattened netlists is shown below:

**yosys> write_verilog -noattr multiple_modules_hier.v**

8. Executing Verilog backend.
Dumping module `\multiple_modules'.
Dumping module `\sub_module1'.
Dumping module `\sub_module2'.

**yosys> !gvim multiple_modules_hier.v**

11. Shell command: gvim multiple_modules_hier.v

**yosys> flatten**

12. Executing FLATTEN pass (flatten design).
Deleting now unused module sub_module1.
Deleting now unused module sub_module2.
<suppressed ~2 debug messages>

**yosys> write_verilog -noattr multiple_modules_flat.v**

13. Executing Verilog backend.
Dumping module `\multiple_modules'.

**yosys> !gvim multiple_modules_flat.v**

14. Shell command: gvim multiple_modules_flat.v

This is the synthyesized circuit for a flattened netlist. Here u1 and u2 are flattened and directly or gates are realized.

![lab51](https://user-images.githubusercontent.com/104454253/166112988-1b02eea6-a6e8-4a7e-8eee-0772182f914f.JPG)

Here is the synthesized circuit of sub_module1. We are also generating module level synthesis so that if there is a top module with multiple and same sub_modules, we can synthesize it once and can use and connect the same netlist multiple times in the top module netlist.

Another reason to generate module level synthesis and then stictch them together is to avoid errors in a top module if its massive and consists of several sub modules. Generating netlist for synthesis and then stiching it together in top level becomes easier and reduces risk of output mismatch.

We control this synthesis using **synth -top <module_name>** command

![lab52](https://user-images.githubusercontent.com/104454253/166113791-5c245c1c-727a-4f15-aaec-9fef1b817aec.JPG)

### 3.1.3 Various Flop coding styles and optimization

**Why Flops and Flop coding styles part1**

In this session, the discussion was about how to code various types of flops and various styles of coding a flop.

**Why a Flop?**
 In a combinational circuit, the output changes after the propagation delay of the circuit once inputs are changed. During the propagation of data, if there are different paths with different propagation delays, there might be a chance of getting a glitch at the output.<br />
 If there are multiple combinational circuits in the design, the occurances of glitches are more thereby making the output unstable.<br />
To curb this drawback, we are going for flops to store the data from the cominational circuits. When a flop is used, the output of combinational circuit is stored in     it and it is propagated only at the posedge or negedge of the clock so that the next combinational circuit gets a glitch free input thereby stabilising the output.
 
 We use initialize signals or control pins called **set** and **reset** on a flop to initialize the flop, other wise a garbage value to sent out to the next combinational circuit. These control pins can be synchronous or asynchronous.
 
#### 3.1.3.1 Lab flop synthesis simulations
 
 **d-flipflop with asynchronous reset**- Here the output **q** goes low whenever reset is high and will not wait for the clock's posedge, i.e irrespective of clock, the output is changed to low.
 
 ![f143804e-5d1a-49d1-9b00-9227785d3e29](https://user-images.githubusercontent.com/104454253/166116145-8fbbacb1-e453-465a-9e41-f21de2337190.jpg)<br />
 
	 module dff_asyncres ( input clk ,  input async_reset , input d , output reg q );<br />
		always @ (posedge clk , posedge async_reset)<br />
		begin<br />
			if(async_reset)<br />
				q <= 1'b0;<br />
			else	<br />
				q <= d;<br />
		end<br />
	endmodule<br />

**Simulation**:

![dffasyncreset](https://user-images.githubusercontent.com/104454253/166189496-f1a82003-1944-40f9-894c-01a91343049e.JPG)

**Synthesized circuit**:

![dasyncressimulation](https://user-images.githubusercontent.com/104454253/166189644-8dc711f0-30a9-49ee-9a2b-b2370c2e6d18.JPG)

 **d-flipflop with asynchronous set**- Here the output **q** goes high whenever set is high and will not wait for the clock's posedge, i.e irrespective of clock, the output is changed to high.
 
![ecc23aa6-e840-46bd-84e3-4eb2b6db0eee](https://user-images.githubusercontent.com/104454253/166116302-08fbdab3-58b3-4ab1-b793-5de48ac86146.jpg)

	module dff_async_set ( input clk ,  input async_set , input d , output reg q );<br />
		always @ (posedge clk , posedge async_set)<br />
		begin<br />
			if(async_set)<br />
				q <= 1'b1;<br />
			else	<br />
				q <= d;<br />
		end<br />
	endmodu<br />le

**Simulation**:

![dasynset](https://user-images.githubusercontent.com/104454253/166189676-5d8af125-e6a9-4ce2-9c17-42b4bac7f5bc.JPG)

**Synthesized circuit**:

![fasyncsetsynthesis](https://user-images.githubusercontent.com/104454253/166189725-579e3fcc-7031-472a-869c-4d7fe0f1fe98.JPG)

**d-flipflop with synchronous reset**- Here the output **q** goes low whenever reset is high and at the positive edge of the clock. Here the reset of the output depends on the clock.

![433468fc-7853-49be-8e81-45fd23297c6c](https://user-images.githubusercontent.com/104454253/166116449-152fffef-c1f5-492b-9101-9a0d431b8a88.jpg)

	module dff_syncres ( input clk , input async_reset , input sync_reset , input d , output reg q );<br />
		always @ (posedge clk )<br />
		begin<br />
			if (sync_reset)<br />
				q <= 1'b0;<br />
			else	<br />
				q <= d;<br />
		end<br />
	endmodule<br />

**Simulation**:

![dsyncres](https://user-images.githubusercontent.com/104454253/166189752-d25f1a3a-e3ee-4612-bd57-ac1cce6ceb45.JPG)

**Synthesized circuit**:

![dsyncresetsynthesis](https://user-images.githubusercontent.com/104454253/166189803-8217d42b-ccf1-4726-b805-c716cb982009.JPG)

**d-flipflop with synchronous and asynchronbous reset**- Here the output **q** goes low whenever asynchronous reset is high where output doesn't depend on clock and also when synchronous reset is high and posedge of clock occurs.

![3c78d01e-7f60-4e51-8150-c8b2dd414957](https://user-images.githubusercontent.com/104454253/166116820-d30a6781-ee51-4fc4-83bb-55e316f992d0.jpg)

	module dff_asyncres_syncres ( input clk , input async_reset , input sync_reset , input d , output reg q );<br />
		always @ (posedge clk , posedge async_reset)<br />
		begin<br />
			if(async_reset)<br />
				q <= 1'b0;<br />
			else if (sync_reset)<br />
				q <= 1'b0;<br />
			else	<br />
				q <= d;<br />
		end<br />
	endmodule<br />

**Simulation**:

![dsyncasyncresJPG](https://user-images.githubusercontent.com/104454253/166189828-374865ed-59d9-417c-9946-a4eb35baa8ff.JPG)

**Synthesized circuit**:

![dffasyncsynceressyhtesis](https://user-images.githubusercontent.com/104454253/166189860-8b65cdee-0ae1-409b-807c-d6bf7c4d30d8.JPG)


#### 3.1.3.2 Interesting optimisations

This lab session deals with some automatic and interesting optimisations of the circuits based on logic. In the below example, multiplying a number with 2 doesn't need any additional hardeware and only needs connecting the bits from **a** to **y** and grounding the LSB bit of y is enough and the same is realized by Yosys.

	module mul2 (input [2:0] a, output [3:0] y);<br />
		assign y = a * 2;<br />
	endmodule<br />

![8e686520-9e94-4c61-b8cf-6ac376a519c9](https://user-images.githubusercontent.com/104454253/166120664-44f5cd53-02bb-4457-bdb4-e8e02f0f64e4.jpg)

**Synthesized circuit**:

![mult2](https://user-images.githubusercontent.com/104454253/166121029-b478d2bc-145a-40ad-9307-c7006fe0320a.JPG)

When it comes to multiplying with powers of 2, it just needs shifting as shown in the below image:

![1781ff88-add9-4b73-b8ec-51257e3074f2](https://user-images.githubusercontent.com/104454253/166120876-1cbc110d-2760-4ab3-9199-7aae4ba2dffb.jpg)

**Netlist for the above schematic**

![mult2netlist](https://user-images.githubusercontent.com/104454253/166121272-723a7aff-dc3e-455d-b623-34d90c8a6508.JPG)

Special case of multiplying **a** with **9**. The result is shown in the below image:

![24c130d3-f95b-4d89-a280-4089cf9839ef](https://user-images.githubusercontent.com/104454253/166121185-a63f798f-2eb2-4510-a589-722f75424bd1.jpg)

The schematic for the same is shown below:

![mult9](https://user-images.githubusercontent.com/104454253/166121402-86c8ab0a-f8dc-490c-bd68-3a97ac53970d.JPG)

**Netlist for the above schematic**

![mult8netlist](https://user-images.githubusercontent.com/104454253/166121455-11f0dee0-cc32-4438-8797-43853aa2bc07.JPG)

# 4. Day3 Combinational and sequential optmizations

## 4.1 Combinational logic optimization with examples

Optimising the combinational logic circuit is squeezing the logic to get the most optimized digital design so that the circuit finally is area and power efficient. This is achieved by the synthesis tool using various techniques and gives us the most optimized circuit.

**Techniques for optimization**:
- Constant propagation which is Direct optimizxation technique
- Boolean logic optimization using K-map or Quine McKluskey

Here is an example for **Constant Propagation**

![optimizations1](https://user-images.githubusercontent.com/104454253/166127772-9ff3dc8e-c5e2-4621-8070-d300df31667e.JPG)

In the above example, if we considor the trasnsistor level circuit of output Y, it has 6 MOS trasistors and when it comes to invertor, only 2 transistors will be sufficient. This is achieved by making A as contstant and propagating the same to output.

Example for **Boolean logic optimization**:

Let's consider an example concurrent statement **assign y=a?(b?c:(c?a:0)):(!c)**

The above expression is using a ternary operator which realizes a series of multiplexers, however, when we write the boolean expression at outputs of each mux and simplify them further using boolean reduction techniques, the outout **y** turns out be just **~(a^c)**

Command to optimize the circuit by yosys is **yosys> opt_clean -purge**

**Example-1**

	module opt_check (input a , input b , output y);
		assign y = a?b:0;
	endmodule

**Optimized circuit**

![opt_check](https://user-images.githubusercontent.com/104454253/166197017-cca6b780-cc9e-43b9-9c21-3c5a7f8c3947.JPG)

**Example-2**

	module opt_check2 (input a , input b , output y);
		assign y = a?1:b;
	endmodule

![opt_check2](https://user-images.githubusercontent.com/104454253/166197032-9cfd996e-0351-4c78-ab88-544977174f8b.JPG)

**Example-3**

	module opt_check3 (input a , input b, input c , output y);
		assign y = a?(c?b:0):0;
	endmodule

![opt_check3](https://user-images.githubusercontent.com/104454253/166197047-542213e7-2c6e-4f61-b391-6beadc48022a.JPG)

**Example-4**

	module opt_check4 (input a , input b , input c , output y);
		assign y = a?(b?(a & c ):c):(!c);
	endmodule
 
 ![opt_check4](https://user-images.githubusercontent.com/104454253/166197170-bbf59d4e-0457-48ce-8ad9-6d36e0c4ffbb.JPG)

**Example- 5**

	module sub_module(input a , input b , output y);
		assign y = a & b;
	endmodule



	module multiple_module_opt2(input a , input b , input c , input d , output y);
		wire n1,n2,n3;
		sub_module U1 (.a(a) , .b(1'b0) , .y(n1));
		sub_module U2 (.a(b), .b(c) , .y(n2));
		sub_module U3 (.a(n2), .b(d) , .y(n3));
		sub_module U4 (.a(n3), .b(n1) , .y(y));
	endmodule

![multiplemoduleopt2](https://user-images.githubusercontent.com/104454253/166197277-56a666ff-eae9-45e8-bff6-afcab3e8e583.JPG)

**Example-6**

		module sub_module1(input a , input b , output y);
		 assign y = a & b;
		endmodule

		module sub_module2(input a , input b , output y);
		 assign y = a^b;
		endmodule

		module multiple_module_opt(input a , input b , input c , input d , output y);
		wire n1,n2,n3;
		sub_module1 U1 (.a(a) , .b(1'b1) , .y(n1));
		sub_module2 U2 (.a(n1), .b(1'b0) , .y(n2));
		sub_module2 U3 (.a(b), .b(d) , .y(n3));

		assign y = c | (b & n1); 
		endmodule

![multiple_moduleopt](https://user-images.githubusercontent.com/104454253/166197287-6681bdc1-ff07-4350-b16e-ae2fc1ababbe.JPG)

### 4.2 Sequential Logic Optimization with examples

Below are the various techniques used for sequential logic optimisations:<br />
-Basic
  - Sequential contant propagation
- Advanced
  - State optimisation
  - Retiming
  - Sequential Logic Cloning (Floor Plan Aware Synthesis)
 
#### 4.2.1 Basic

**Sequential contant propagation**-Here only the first logic can be optimized as the output of flp always zero. However for the second flop, the output changes continuously, therefor it cannot be used for contant propagation.

![1e34f504-64e1-411d-a668-5294c6ea78f5](https://user-images.githubusercontent.com/104454253/166128292-7faf6384-792a-4455-a847-350bb95b631f.jpg)

![6ef09bec-38c3-4a13-901b-0221461c9aca](https://user-images.githubusercontent.com/104454253/166128295-40ddcdda-9b4e-4a0f-a27e-5c5bd9a8bb3e.jpg)

#### 4.2.2. Advanced
**State Optimisation**: This is optimisation of unused state. Using this technique we can come up with most optimised state machine.

**Cloning**: This is done when performing PHYSICAL AWARE SYNTHESIS. Lets consider a flop A which is connected to flop B and flop C through a combination logic. If B and C are placed far from A in the flooerplan, there is a routing path delay. To avoid this, we connect A to two intermediate flops and then from these flops the output is sent to B and C thereby decreasing the delay. This process is called cloning since we are generating two new flops with same functionality as A.

**Retiming**: Retiming is a powerful sequential optimization technique used to move registers across the combinational logic or to optimize the number of registers to improve performance via power-delay trade-off, without changing the input-output behavior of the circuit. 

**Example-1**
In the synthesis report, we'll see that a Dflop was inferred in this example.
module dff_const1(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
	if(reset)
		q <= 1'b0;
	else
		q <= 1'b1;
end

endmodule

**Example-2**
module dff_const2(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
	if(reset)
		q <= 1'b1;
	else
		q <= 1'b1;
end

endmodule

**SKY130RTL D3SK3 L2 Lab07 Sequential Logic Optimisations part2**
**Example-3**

module dff_const3(input clk, input reset, output reg q);
reg q1;

always @(posedge clk, posedge reset)
begin
	if(reset)
	begin
		q <= 1'b1;
		q1 <= 1'b0;
	end
	else
	begin
		q1 <= 1'b1;
		q <= q1;
	end
end

endmodule

**Example4**

module dff_const4(input clk, input reset, output reg q);
reg q1;

always @(posedge clk, posedge reset)
begin
	if(reset)
	begin
		q <= 1'b1;
		q1 <= 1'b1;
	end
	else
	begin
		q1 <= 1'b1;
		q <= q1;
	end
end

endmodule

**Example5**

module dff_const5(input clk, input reset, output reg q);
reg q1;

always @(posedge clk, posedge reset)
begin
	if(reset)
	begin
		q <= 1'b0;
		q1 <= 1'b0;
	end
	else
	begin
		q1 <= 1'b1;
		q <= q1;
	end
end

endmodule

**SKY130RTL D3SK4 L1 Seq optimisation unused outputs part1**
**Example1**

module counter_opt (input clk , input reset , output q);
reg [2:0] count;
assign q = count[0];

always @(posedge clk ,posedge reset)
begin
	if(reset)
		count <= 3'b000;
	else
		count <= count + 1;
end

endmodule

**SKY130RTL D3SK4 L2 Seq optimisation unused outputs part2**

Updated counter logic- 

All the other blocks in synthesizer are for incrementing the counter but the output is only from the three input NOR gate.

module counter_opt (input clk , input reset , output q);
reg [2:0] count;
assign q = {count[2:0]==3'b100};

always @(posedge clk ,posedge reset)
begin
	if(reset)
		count <= 3'b000;
	else
		count <= count + 1;
end

endmodule

**DAY4-GLS, blocking vs non-blocking and Synthesis-Simulation mismatch**

**GLS, Synthesis-Simulation mismatch and Blocking/Non-blocking statements**
**SKY130RTL D4SK1 L1 GLSConceptsAndFlowUsingIverilog**
As the synthesizer doen't look for sensitivity list and it looks only for the statements in procedural block, it infers correct circuit  and if we simulate the netlist code, there will be a synthesis simulation mismatch
To avoid the synthesis and simulation mismatch. it is very important to check the behavpior of the circuit first and then match it with the expected output seen in simulation and make sure there are no synthesis and simulation mismatches. This is why we use GLS.

**SKY130RTL D4SK2 L1 Lab GLS Synth Sim Mismatch part1**

module ternary_operator_mux (input i0 , input i1 , input sel , output y);
	assign y = sel?i1:i0;
	endmodule
	
**SKY130RTL D4SK2 L2 Lab GLS Synth Sim Mismatch part2**
module bad_mux (input i0 , input i1 , input sel , output reg y);
always @ (sel)
begin
	if(sel)
		y <= i1;
	else 
		y <= i0;
end
endmodule

**SKY130RTL D4SK3 L1 Lab Synth sim mismatch blocking statement part1**

Here the output is depending on the past value of x which is dependednt on a and b and it appears like a flop.
module blocking_caveat (input a , input b , input  c, output reg d); 
reg x;
always @ (*)
begin
	d = x & c;
	x = a | b;
end
endmodule

**SKY130RTL D4SK3 L2 Lab Synth sim mismatch blocking statement part2**

Here the netlist is generated and the respective gtkwave is generated

**DAY5- Optimization in synthesis**

**If Case constructs**
**SKY130RTL D5SK1 L1 IF CASE Constructs part1**
if is mainly used to create priority logic

syntax
nested if elseif syntax and tell about priorities
Dangers with IF
**SKY130RTL D5SK1 L2 IF CASE Constructs part2**
continuation
Case construct and dangers of case construct

**SKY130RTL D5SK1 L3 IF CASE Constructs part3**
caveat 2 of case

comparison btwn if, elseif, elseif, else and case

**SKY130RTL D5SK2 L1 Lab Incomplete IF part1**

Example1

module incomp_if (input i0 , input i1 , input i2 , output reg y);
always @ (*)
begin
	if(i0)
		y <= i1;
end
endmodule

**SKY130RTL D5SK2 L2 Lab Incomplete IF part2**
Example2-

module incomp_if2 (input i0 , input i1 , input i2 , input i3, output reg y);
always @ (*)
begin
	if(i0)
		y <= i1;
	else if (i2)
		y <= i3;

end
endmodule

**SKY130RTL D5SK3 L1 Lab incomplete overlapping Case part1**
Example1- 
module partial_case_assign (input i0 , input i1 , input i2 , input [1:0] sel, output reg y , output reg x);
always @ (*)
begin
	case(sel)
		2'b00 : begin
			y = i0;
			x = i2;
			end
		2'b01 : y = i1;
		default : begin
		           x = i1;
			   y = i2;
			  end
	endcase
end
endmodule

**SKY130RTL D5SK3 L2 Lab incomplete overlapping Case part2**

module comp_case (input i0 , input i1 , input i2 , input [1:0] sel, output reg y);
always @ (*)
begin
	case(sel)
		2'b00 : y = i0;
		2'b01 : y = i1;
		default : y = i2;
	endcase
end
endmodule

**Example3**

module partial_case_assign (input i0 , input i1 , input i2 , input [1:0] sel, output reg y , output reg x);
always @ (*)
begin
	case(sel)
		2'b00 : begin
			y = i0;
			x = i2;
			end
		2'b01 : y = i1;
		default : begin
		           x = i1;
			   y = i2;
			  end
	endcase
end
endmodule


Example-4
**SKY130RTL D5SK3 L3 Lab incomplete overlapping Case part3**
module bad_case (input i0 , input i1, input i2, input i3 , input [1:0] sel, output reg y);
always @(*)
begin
	case(sel)
		2'b00: y = i0;
		2'b01: y = i1;
		2'b10: y = i2;
		2'b1?: y = i3;
		//2'b11: y = i3;
	endcase
end

endmodule
**SKY130RTL D5SK3 L4 Lab incomplete overlapping Case part4** 
bad_case synthesis

**SKY130RTL D5SK4 L1 For Loop and For Generate part1**
Generate for loop is used for instantaing hardware

**SKY130RTL D5SK4 L2 For Loop and For Generate part2**
For loop for mux and demux
FOR Generate
**SKY130RTL D5SK4 L2 For Loop and For Generate part3**
For Generate example

**SKY130RTL D5SK5 L1 Lab For and For Generate part1**
simulation, gtkwave, synthesis, gtkwave of synthesis.
module mux_generate (input i0 , input i1, input i2 , input i3 , input [1:0] sel  , output reg y);
wire [3:0] i_int;
assign i_int = {i3,i2,i1,i0};
integer k;
always @ (*)
begin
for(k = 0; k < 4; k=k+1) begin
	if(k == sel)
		y = i_int[k];
end
end
endmodule

**SKY130RTL D5SK5 L1 Lab For and For Generate part2**

module demux_case (output o0 , output o1, output o2 , output o3, output o4, output o5, output o6 , output o7 , input [2:0] sel  , input i);
reg [7:0]y_int;
assign {o7,o6,o5,o4,o3,o2,o1,o0} = y_int;
integer k;
always @ (*)
begin
y_int = 8'b0;
	case(sel)
		3'b000 : y_int[0] = i;
		3'b001 : y_int[1] = i;
		3'b010 : y_int[2] = i;
		3'b011 : y_int[3] = i;
		3'b100 : y_int[4] = i;
		3'b101 : y_int[5] = i;
		3'b110 : y_int[6] = i;
		3'b111 : y_int[7] = i;
	endcase

end
endmodule


module demux_generate (output o0 , output o1, output o2 , output o3, output o4, output o5, output o6 , output o7 , input [2:0] sel  , input i);
reg [7:0]y_int;
assign {o7,o6,o5,o4,o3,o2,o1,o0} = y_int;
integer k;
always @ (*)
begin
y_int = 8'b0;
for(k = 0; k < 8; k++) begin
	if(k == sel)
		y_int[k] = i;
end
end
endmodule

**SKY130RTL D5SK5 L3 Lab For and For Generate part3**

module rca (input [7:0] num1 , input [7:0] num2 , output [8:0] sum);
wire [7:0] int_sum;
wire [7:0]int_co;

genvar i;
generate
	for (i = 1 ; i < 8; i=i+1) begin
		fa u_fa_1 (.a(num1[i]),.b(num2[i]),.c(int_co[i-1]),.co(int_co[i]),.sum(int_sum[i]));
	end

endgenerate
fa u_fa_0 (.a(num1[0]),.b(num2[0]),.c(1'b0),.co(int_co[0]),.sum(int_sum[0]));


assign sum[7:0] = int_sum;
assign sum[8] = int_co[7];
endmodule

module fa (input a , input b , input c, output co , output sum);
endmodule

