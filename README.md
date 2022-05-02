# RTL-design-using-Verilog-with-SKY130-Technology
![Verilog-flyer](https://user-images.githubusercontent.com/104454253/166084640-128e6351-1739-4b38-a3ce-76459da921b5.png)
# Table of contents
 - [1. Introduction](#1-Introduction)
 - [2. Day-1- Introduction to Verilog RTL design and Synthesis](#2-Day-1--Introduction-to-Verilog-RTL-design-and-Synthesis)
    - [2.1 Various aspects of frontend design](#21-Various-aspects-of-frontend-design)
    - [2.2 Introduction to open source simulator iverilog and gtkwave](#22-Introduction-to-open-source-simulator-iverilog-and-gtkwave)
     	- [2.2.1 Lab examples using iverilog and gtkwave](#221-Lab-examples-using-iverilog-and-gtkwave)
    - [2.3 Introduction to Yosys synthesizer](#23-Introduction-to-Yosys-synthesizer)
        - [2.3.1 Labs on Yosys introduction](#231-Labs-on-Yosys-introduction)
 - [3. DAY2-Timing libs, hierarchical, flat synthesis, efficient flop coding styles](#3-DAY2-Timing-libs-hierarchical-flat-synthesis-efficient-flop-coding-styles)
    - [3.1 Introduction to timing .libs](#31-Introduction-to-timing-libs)
        - [3.1.1 LAB- Introduction to dot Lib](#311-LAB--Introduction-to-dot-Lib)
    - [3.2 LAB- Hierarchical synthesis and flat synthesis](#32-LAB-Hierarchical-synthesis-and-flat-synthesis)
    - [3.3 Various Flop coding styles and optimization](#33-Various-Flop-coding-styles-and-optimization)
         - [3.3.1 Lab- flop synthesis simulations](#331-Lab-flop-synthesis-simulations)
         - [3.3.2 Interesting optimisations](#332-Interesting-optimisations)
 - [4. Day3- Combinational and sequential optmizations](#4-Day3--Combinational-and-sequential-optmizations)
    - [4.1 Combinational logic optimization with examples](#41-Combinational-logic-optimization-with-examples)
    - [4.2 Sequential logic optimization with examples](#42-Sequential-logic-optimization-with-examples)
        - [4.2.1 Basic](#421-Basic)
        - [4.2.2 Advanced](#422-Advanced)
        - [4.2.3 Sequential optimisation of unused outputs](#423-Sequential-optimisation-of-unused-outputs)
 - [5. DAY4- GLS, blocking vs non-blocking and Synthesis-Simulation mismatch](#5-DAY4--GLS-blocking-vs-non-blocking-and-Synthesis-Simulation-mismatch)
    - [5.1 GLS, Synthesis-Simulation mismatch and Blocking, Non-blocking statements](#51-GLS-Synthesis-Simulation-mismatch-and-Blocking-Non-blocking-statements)
        - [5.1.1 GLS Concepts And Flow Using Iverilog](#511-GLS-Concepts-And-Flow-Using-Iverilog)
        - [5.1.2 Synthesis Simulation Mismatch](#512-Synthesis-Simulation-Mismatch)
    - [5.2 Lab- GLS Synth Sim Mismatch](#52-Lab--GLS-Synth-Sim-Mismatch)
    - [5.3 Lab- Synthesis simulation mismatch blocking statement](#53-Lab--Synthesis-simulation-mismatch-blocking-statement)
 - [6. DAY5- if, case, for loop and for generate](#6-DAY5--if-case-for-loop-and-for-generate)
    - [6.1 If and Case constructs](#61-If-and-Case-constructs)
       - [6.1.1 If construct](#611-If-construct)
       - [6.1.2 Case construct](#612-Case-construct)
    - [6.2 Lab- Incomplete IF](#62-Lab--Incomplete-IF)
    - [6.3 Lab- incomplete overlapping Case](#63-Lab--incomplete-overlapping-Case)
    - [6.4 For Loop and For Generate](#64-For-Loop-and-For-Generate)
    - [6.5 Lab- For and For Generate](#65-Lab--For-and-For-Generate)
 - [7. Word of Thanks](#7-Word-of-Thanks)
# 1. Introduction
This report is a final submission of 5-day workshop from [VLSI Sytem Design-IAT](https://www.vlsisystemdesign.com/) on RTL design and synthesis using open source tools, in particular iVerilog, GTKWave, Yosy and Skywater 130nm Standard Cell Libraries  
# 2. Day-1- Introduction to Verilog RTL design and Synthesis
## 2.1 Various aspects of frontend design
**RTL Design**: In simple terms RTL design or Register Transfer Level design is a method in which we can transfer data from one register to another. In RTL design we write code for Combinational and Sequential circuits in HDL(Hardware Description Language) like Verilog or VerilogHDL which can model logical and hardware operation. RTL design can be one code or set of verilog codes. **One key note is that we need to write RTL design with optimized and synthesizable (realizable as physical gates)**.

**Sample RTL design outline:**

	module module_name (port list);
		//declarations;
		//initializations;
		//continuos concurrent assigments;
		//procedural blocks;
	endmodule

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

	module good_mux (input i0 , input i1 , input sel , output reg y); 
		always @ (*)
		begin
			if(sel)
			y <= i1;
			else 
			y <= i0;
		end
	endmodule**


	`timescale 1ns / 1ps
	module tb_good_mux;
	// Inputs
	reg i0,i1,sel;
	// Outputs
	wire y;
      		// Instantiate the Unit Under Test (UUT), name based instantiation
		good_mux uut (.sel(sel),.i0(i0),.i1(i1),.y(y));
		//good_mux uut (sel,i0,i1,y);  //order based instantiation
	initial begin
		$dumpfile("tb_good_mux.vcd");
		$dumpvars(0,tb_good_mux);
		// Initialize Inputs
		sel = 0;
		i0 = 0;
		i1 = 0;
		#300 $finish;
	end
	always #75 sel = ~sel;
	always #10 i0 = ~i0;
	always #55 i1 = ~i1;
	endmodule**

![goodmuxgtkwaveCapture](https://user-images.githubusercontent.com/104454253/166117453-7f4918e9-acb4-4ad5-b35a-0933c8578312.JPG)


## 2.3 Introduction to Yosys synthesizer

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

- RTL Design  - read_verilog
- .lib        - read_liberty
- netlist file- write_verilog

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

## 3.2 LAB- Hierarchical synthesis and flat synthesis

**multiple_module**<br />

	module sub_module2 (input a, input b, output y);
		assign y = a | b;
	endmodule
	
	module sub_module1 (input a, input b, output y);
		assign y = a&b;
	endmodule


	module multiple_modules (input a, input b, input c , output y);
	wire net1;
	sub_module1 u1(.a(a),.b(b),.y(net1));  //net1 = a&b
	sub_module2 u2(.a(net1),.b(c),.y(y));  //y = net1|c ,ie y = a&b + c;
	endmodule

This is the schematic as per the connections in the above module.

![94d6174a-bcb0-4cad-bef6-a0537ef32ef1](https://user-images.githubusercontent.com/104454253/166108402-f96b7fa9-3d8f-4f05-b4cb-89e443e43718.jpg)

However, the yosys synthesizer generates the following schematic instead of the above one and with in the submodules, the connections are made

![lab5](https://user-images.githubusercontent.com/104454253/166108721-0e13ddc4-6c31-45bb-b735-560646e7497e.JPG)

The synthesizer considers the module hierarcy and does the mapping accordting to instantiation. Here is the hierarchical netlist code for the  multiple_modules:

	module multiple_modules(a, b, c, y);
		  input a;
 		 input b;
 		 input c;
		  wire net1;
 		 output y;
 	  sub_module1 u1 (.a(a),.b(b),.y(net1) );
	  sub_module2 u2 (.a(net1),.b(c),.y(y));
	endmodule
	
	module sub_module1(a, b, y);
 	 wire _0_;
 	 wire _1_;
 	 wire _2_;
 	 input a;
 	 input b;
 	 output y;
 	 sky130_fd_sc_hd__and2_0 _3_ (.A(_1_),.B(_0_),.X(_2_));
 	 assign _1_ = b;
 	 assign _0_ = a;
 	 assign y = _2_;
	endmodule

	module sub_module2(a, b, y);
  	wire _0_;
 	 wire _1_;
 	 wire _2_;
  	input a;
  	input b;
 	 output y;
 	 sky130_fd_sc_hd__lpflow_inputiso1p_1 _3_ (.A(_1_),.SLEEP(_0_),.X(_2_) );
 	 assign _1_ = b;
 	 assign _0_ = a;
 	 assign y = _2_;
	endmodule

Flattened netlist:

In flattened netlist, the hierarcies are flattend out and there is single module i.e, gates are instantiated directly instead of sub_modules. Here is the flattened netlist code for the  multiple_modules:

	module multiple_modules(a, b, c, y);
 		 wire _0_;
  		 wire _1_;
 		 wire _2_;
 		 wire _3_;
		 wire _4_;
		 wire _5_;
 		 input a;
 		 input b;
 		 input c;
 		 wire net1;
 		 wire \u1.a ;
		 wire \u1.b ;
		 wire \u1.y ;
		 wire \u2.a ;
		 wire \u2.b ;
 		 wire \u2.y ;
  		output y;
 		 sky130_fd_sc_hd__and2_0 _6_ (
  		  .A(_1_),
  		 .B(_0_),
   		 .X(_2_)
  		);
 		 sky130_fd_sc_hd__lpflow_inputiso1p_1 _7_ (
  		  .A(_4_),
 		  .SLEEP(_3_),
  		  .X(_5_)
 		 );
 		 assign _4_ = \u2.b ;
 		 assign _3_ = \u2.a ;
 		 assign \u2.y  = _5_;
 		 assign \u2.a  = net1;
		 assign \u2.b  = c;
 		 assign y = \u2.y ;
		 assign _1_ = \u1.b ;
		 assign _0_ = \u1.a ;
		 assign \u1.y  = _2_;
		 assign \u1.a  = a;
		 assign \u1.b  = b;
 		 assign net1 = \u1.y ;
		endmodule

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

### 3.3 Various Flop coding styles and optimization

**Why Flops and Flop coding styles part1**

In this session, the discussion was about how to code various types of flops and various styles of coding a flop.

**Why a Flop?**

 In a combinational circuit, the output changes after the propagation delay of the circuit once inputs are changed. During the propagation of data, if there are different paths with different propagation delays, there might be a chance of getting a glitch at the output.<br />
 If there are multiple combinational circuits in the design, the occurances of glitches are more thereby making the output unstable.<br />
To curb this drawback, we are going for flops to store the data from the cominational circuits. When a flop is used, the output of combinational circuit is stored in     it and it is propagated only at the posedge or negedge of the clock so that the next combinational circuit gets a glitch free input thereby stabilising the output.
 
 We use initialize signals or control pins called **set** and **reset** on a flop to initialize the flop, other wise a garbage value to sent out to the next combinational circuit. These control pins can be synchronous or asynchronous.
 
### 3.3.1 Lab- flop synthesis simulations
 
 **d-flipflop with asynchronous reset**- Here the output **q** goes low whenever reset is high and will not wait for the clock's posedge, i.e irrespective of clock, the output is changed to low.
 
 ![f143804e-5d1a-49d1-9b00-9227785d3e29](https://user-images.githubusercontent.com/104454253/166116145-8fbbacb1-e453-465a-9e41-f21de2337190.jpg)<br />
 
	 module dff_asyncres ( input clk ,  input async_reset , input d , output reg q );
		always @ (posedge clk , posedge async_reset)
		begin
			if(async_reset)
				q <= 1'b0;
			else	
				q <= d;
		end
	endmodule

**Simulation**:

![dffasyncreset](https://user-images.githubusercontent.com/104454253/166189496-f1a82003-1944-40f9-894c-01a91343049e.JPG)

**Synthesized circuit**:

![dasyncressimulation](https://user-images.githubusercontent.com/104454253/166189644-8dc711f0-30a9-49ee-9a2b-b2370c2e6d18.JPG)

 **d-flipflop with asynchronous set**- Here the output **q** goes high whenever set is high and will not wait for the clock's posedge, i.e irrespective of clock, the output is changed to high.
 
![ecc23aa6-e840-46bd-84e3-4eb2b6db0eee](https://user-images.githubusercontent.com/104454253/166116302-08fbdab3-58b3-4ab1-b793-5de48ac86146.jpg)

	module dff_async_set ( input clk ,  input async_set , input d , output reg q );
		always @ (posedge clk , posedge async_set)
		begin
			if(async_set)
				q <= 1'b1;
			else
				q <= d;
		end
	endmodule

**Simulation**:

![dasynset](https://user-images.githubusercontent.com/104454253/166189676-5d8af125-e6a9-4ce2-9c17-42b4bac7f5bc.JPG)

**Synthesized circuit**:

![fasyncsetsynthesis](https://user-images.githubusercontent.com/104454253/166189725-579e3fcc-7031-472a-869c-4d7fe0f1fe98.JPG)

**d-flipflop with synchronous reset**- Here the output **q** goes low whenever reset is high and at the positive edge of the clock. Here the reset of the output depends on the clock.

![433468fc-7853-49be-8e81-45fd23297c6c](https://user-images.githubusercontent.com/104454253/166116449-152fffef-c1f5-492b-9101-9a0d431b8a88.jpg)

	module dff_syncres ( input clk , input async_reset , input sync_reset , input d , output reg q );
		always @ (posedge clk )
		begin
			if (sync_reset)
				q <= 1'b0;
			else	
				q <= d;
		end
	endmodule

**Simulation**:

![dsyncres](https://user-images.githubusercontent.com/104454253/166189752-d25f1a3a-e3ee-4612-bd57-ac1cce6ceb45.JPG)

**Synthesized circuit**:

![dsyncresetsynthesis](https://user-images.githubusercontent.com/104454253/166189803-8217d42b-ccf1-4726-b805-c716cb982009.JPG)

**d-flipflop with synchronous and asynchronbous reset**- Here the output **q** goes low whenever asynchronous reset is high where output doesn't depend on clock and also when synchronous reset is high and posedge of clock occurs.

![3c78d01e-7f60-4e51-8150-c8b2dd414957](https://user-images.githubusercontent.com/104454253/166116820-d30a6781-ee51-4fc4-83bb-55e316f992d0.jpg)

	module dff_asyncres_syncres ( input clk , input async_reset , input sync_reset , input d , output reg q );
		always @ (posedge clk , posedge async_reset)
		begin
			if(async_reset)
				q <= 1'b0;
			else if (sync_reset)
				q <= 1'b0;
			else	
				q <= d;
		end
	endmodule

**Simulation**:

![dsyncasyncresJPG](https://user-images.githubusercontent.com/104454253/166189828-374865ed-59d9-417c-9946-a4eb35baa8ff.JPG)

**Synthesized circuit**:

![dffasyncsynceressyhtesis](https://user-images.githubusercontent.com/104454253/166189860-8b65cdee-0ae1-409b-807c-d6bf7c4d30d8.JPG)


### 3.3.2 Interesting optimisations

This lab session deals with some automatic and interesting optimisations of the circuits based on logic. In the below example, multiplying a number with 2 doesn't need any additional hardeware and only needs connecting the bits from **a** to **y** and grounding the LSB bit of y is enough and the same is realized by Yosys.

	module mul2 (input [2:0] a, output [3:0] y);
		assign y = a * 2;
	endmodule

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

# 4. Day3- Combinational and sequential optmizations

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

![62695d29-f76d-426c-ab99-af6fbb2abda0](https://user-images.githubusercontent.com/104454253/166292324-f3243d68-55c1-4829-a836-8177edc79613.jpg)

	module opt_check (input a , input b , output y);
		assign y = a?b:0;
	endmodule

**Optimized circuit**

![opt_check](https://user-images.githubusercontent.com/104454253/166197017-cca6b780-cc9e-43b9-9c21-3c5a7f8c3947.JPG)

**Example-2**

![e748028c-ea3d-4d58-8f5a-71a8248e4dd5](https://user-images.githubusercontent.com/104454253/166292286-6b3fd349-23af-463e-988e-863038a542d8.jpg)

	module opt_check2 (input a , input b , output y);
		assign y = a?1:b;
	endmodule

![opt_check2](https://user-images.githubusercontent.com/104454253/166197032-9cfd996e-0351-4c78-ab88-544977174f8b.JPG)

**Example-3**

![bfcc0b60-1b3e-4f45-a5cf-88b33f4e7dcf](https://user-images.githubusercontent.com/104454253/166292243-7fa03ef2-bce9-418f-8830-e9587d459aef.jpg)

	module opt_check3 (input a , input b, input c , output y);
		assign y = a?(c?b:0):0;
	endmodule

![opt_check3](https://user-images.githubusercontent.com/104454253/166197047-542213e7-2c6e-4f61-b391-6beadc48022a.JPG)

**Example-4**

![a42e5eb3-2966-4fb0-bad3-6c6a6fa12798](https://user-images.githubusercontent.com/104454253/166292378-3ebdc824-de41-4385-9f7f-1654e71c0ff0.jpg)

	module opt_check4 (input a , input b , input c , output y);
		assign y = a?(b?(a & c ):c):(!c);
	endmodule
 
 ![opt_check4](https://user-images.githubusercontent.com/104454253/166197170-bbf59d4e-0457-48ce-8ad9-6d36e0c4ffbb.JPG)

**Example- 5**

![7c0faa7e-cffc-44f3-988a-17e2e4857675](https://user-images.githubusercontent.com/104454253/166292469-dea41b7c-40ed-4b5b-b9f7-30ad3de0bad2.jpg)

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

![89cc6397-defe-4bba-a767-6e28c99ba0de](https://user-images.githubusercontent.com/104454253/166292486-d2041066-ab26-4010-b253-93c510c51674.jpg)

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

**Sequential contant propagation**- Here only the first logic can be optimized as the output of flop is always zero. However for the second flop, the output changes continuously, therefor it cannot be used for contant propagation.

![1e34f504-64e1-411d-a668-5294c6ea78f5](https://user-images.githubusercontent.com/104454253/166128292-7faf6384-792a-4455-a847-350bb95b631f.jpg)

![6ef09bec-38c3-4a13-901b-0221461c9aca](https://user-images.githubusercontent.com/104454253/166128295-40ddcdda-9b4e-4a0f-a27e-5c5bd9a8bb3e.jpg)

#### 4.2.2. Advanced
**State Optimisation**: This is optimisation of unused state. Using this technique we can come up with most optimised state machine.

**Cloning**: This is done when performing PHYSICAL AWARE SYNTHESIS. Lets consider a flop A which is connected to flop B and flop C through a combination logic. If B and C are placed far from A in the flooerplan, there is a routing path delay. To avoid this, we connect A to two intermediate flops and then from these flops the output is sent to B and C thereby decreasing the delay. This process is called cloning since we are generating two new flops with same functionality as A.

**Retiming**: Retiming is a powerful sequential optimization technique used to move registers across the combinational logic or to optimize the number of registers to improve performance via power-delay trade-off, without changing the input-output behavior of the circuit. 

**Example-1**<br />
Here flop will be inferred as the output is not constant. <br />

	module dff_const1(input clk, input reset, output reg q);
		always @(posedge clk, posedge reset)
		begin
			if(reset)
				q <= 1'b0;
			else
				q <= 1'b1;
		end
	endmodule

![bdf6fb4e-ee93-4f76-bbef-e25a8fe8aeda](https://user-images.githubusercontent.com/104454253/166292590-1d76f35e-f83f-486e-a154-1e0a7a1441fb.jpg)

**Simulation**

![optimdff_const1](https://user-images.githubusercontent.com/104454253/166199133-1d0a9a00-eeb4-46ab-9115-25383ec1bcbd.JPG)

**Synthesis**<br />
In the synthesis report, we'll see that a Dflop was inferred in this example.

![opt_dff_const1synthesis](https://user-images.githubusercontent.com/104454253/166199378-0679aafd-5663-4622-a59b-04a09332db14.JPG)

![optmdff_const1report](https://user-images.githubusercontent.com/104454253/166199406-6ca38935-e5cf-4138-857b-05700d258356.JPG)

**Example-2**<br />
Here flop will not be inferred as the output is always high. <br />

	module dff_const2(input clk, input reset, output reg q);
		always @(posedge clk, posedge reset)
		begin
			if(reset)
				q <= 1'b1;
			else
				q <= 1'b1;
		end
	endmodule

![7a01ae65-de9b-4e33-812e-206eb3c2733f](https://user-images.githubusercontent.com/104454253/166292615-3af99c64-305e-434a-b21b-907a30f83ab0.jpg)

**Simulation**

![optdff_const2](https://user-images.githubusercontent.com/104454253/166199273-8728b4d5-11fc-4652-8b32-698df254856f.JPG)

**Synthesis**

![optdff_const2synthesis](https://user-images.githubusercontent.com/104454253/166199696-4bc5c938-8381-4690-9b07-b59ba407862d.JPG)

![optdffconst2report](https://user-images.githubusercontent.com/104454253/166199711-7a5f51df-a740-4c9c-a3ef-67d40e32c12a.JPG)

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


**Simulation***

![gtkwavedff_const3](https://user-images.githubusercontent.com/104454253/166200122-0c960389-20b1-497c-b1be-12a084944a77.JPG)

**Synthesis**

![optdff_const3](https://user-images.githubusercontent.com/104454253/166200223-a658f4f2-3ac0-4503-8292-298ba5e194ea.JPG)

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

**Simulation***

![gtkwavedff_const4](https://user-images.githubusercontent.com/104454253/166200334-a0d60601-1092-49ef-8fbc-477c99210e92.JPG)

**Synthesis**

![optsynthdff_const4](https://user-images.githubusercontent.com/104454253/166200427-53425dc0-93f6-4137-882c-8f64d614103f.JPG)

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

**Simulation***

![gtkwavedff_const5](https://user-images.githubusercontent.com/104454253/166200555-0ba4be08-592d-4fbd-8628-6e3a39f2999c.JPG)

**Synthesis**

![synthdff_const5](https://user-images.githubusercontent.com/104454253/166200573-6d8e2ed8-5e3f-423a-880f-f825e94ea457.JPG)

### 4.2.3 Sequential optimisation of unused outputs
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
		
![2f8f6e43-102f-4fd7-aad2-585b0a55098d](https://user-images.githubusercontent.com/104454253/166292769-44c0406c-4fbb-4a3e-9345-2b308783a7de.jpg)

**Synthesis**

![synthcounter_opt](https://user-images.githubusercontent.com/104454253/166201168-cadde883-f4dd-4de1-9e09-00dd5d06647c.JPG)

**Updated counter logic-** 

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

![0e890967-cf43-4be0-8f53-968fe98c7f55](https://user-images.githubusercontent.com/104454253/166292799-97d7ee9c-1000-4dcc-935c-a5ae56a74f8e.jpg)

**Synthesis**

All the other blocks in synthesizer are for incrementing the counter but the output is only from the three input NOR gate.

![synthcounter_optupdated](https://user-images.githubusercontent.com/104454253/166201230-5459b740-9774-444e-b6db-ad6ccb8147ef.JPG)

# 5. DAY4- GLS, blocking vs non-blocking and Synthesis-Simulation mismatch

# 5.1 GLS, Synthesis-Simulation mismatch and Blocking, Non-blocking statements
### 5.1.1 GLS Concepts And Flow Using Iverilog

**What is GLS- Gate Level Simulation?**:<br />
GLS is generating the simulation output by running test bench with netlist file generated from synthesis as design under test. Netlist is logically same as RTL code, therefore, same test bench can be used for it.

**Why GLS?**:<br />
We perform this to verify logical correctness of the design after synthesizing it. Also ensuring the timing of the design is met.

Below picture gives an insight of the procedure. Here while using iverilog, we also include gate level verilog models to generate GLS simulation.

![Screenshot (49)](https://user-images.githubusercontent.com/104454253/166256679-1ac9167a-1358-4c60-bbdb-0f6423f0faa3.png)

### 5.1.2 Synthesis Simulation Mismatch

There are three main reasons for Synthesis Simulation Mismatch:<br />
- Missing sensitivity list in always block
- Blocking vs Non-Blocking Assignments
- Non standard Verilog coding

**Missing sensitivity list in always block:**<br />

If the consider - Example-2, we can see the only **sel** is mentioned in the sensitivity list. During the simulation, the waveforms will resemble a latched output but the simulation of netlist will not infer this as the synthesizer will only look at the statements with in the procedural block and not the sensitivity list.

As the synthesizer doen't look for sensitivity list and it looks only for the statements in procedural block, it infers correct  circuit  and if we simulate the netlist code, there will be a synthesis simulation mismatch.

To avoid the synthesis and simulation mismatch. It is very important to check the behaviour of the circuit first and then match it with the expected output seen in simulation and make sure there are no synthesis and simulation mismatches. This is why we use GLS.

**Blocking vs Non-Blocking Assignments**:

Blocking statements execute the statemetns in the order they are written inside the always block. Non-Blocking statements execute all the RHS and once always block is entered, the values are assigned to LHS. This will give mismatch as sometimes, improper use of blocking statements can create latches. Please click here to go to example - Example4

## 5.2 Lab- GLS Synth Sim Mismatch

**Example-1**

	module ternary_operator_mux (input i0 , input i1 , input sel , output y);
		assign y = sel?i1:i0;
	endmodule
	
**Simulation**

![gtkternary_mux](https://user-images.githubusercontent.com/104454253/166204654-3946e42a-3e48-478e-9144-37b7eeadc8cd.JPG)

**Synthesis**

![synthesisternary_mux](https://user-images.githubusercontent.com/104454253/166204718-12da283d-277d-4d72-8c02-93521cda3abe.JPG)

**Netlist Simulation**

![simGLSternary_mux](https://user-images.githubusercontent.com/104454253/166204813-8022428f-9848-4bf2-9c22-9dc730b1305b.JPG)

# Example-2

	module bad_mux (input i0 , input i1 , input sel , output reg y);
		always @ (sel)
		begin
			if(sel)
				y <= i1;
			else 
				y <= i0;
		end
	endmodule

**Simulation**

![simbad_mux](https://user-images.githubusercontent.com/104454253/166204842-37f6ce2e-6e3f-40f4-8565-291621584acf.JPG)

**Synthesis**

![synthbad_mux](https://user-images.githubusercontent.com/104454253/166205989-ebd33255-d75a-4f1e-a126-bd0da6ea3c0f.JPG)

**Netlist Simulation**

![simbadmuxnetlist](https://user-images.githubusercontent.com/104454253/166204853-14865a7a-2b9a-4af0-b552-4a1a647ad56c.JPG)

**MISMATCH**<br />

![Capture](https://user-images.githubusercontent.com/104454253/166259062-c3cb7fb2-e28a-4e12-b2d6-83812c5f2582.JPG)

**Example-3**

	module good_mux (input i0 , input i1 , input sel , output reg y);
		always @ (*)
		begin
			if(sel)
				y <= i1;
			else 
				y <= i0;
		end
	endmodule
	
**Simulation**

![gtkwavegood_mux](https://user-images.githubusercontent.com/104454253/166206079-3b4799e8-c110-409a-8630-6bba41d4e52b.JPG)

**Synthesis**

![synthgood_mux](https://user-images.githubusercontent.com/104454253/166206100-6ac5d20c-89ec-48ba-bfb8-20113468e350.JPG)

**Netlist Simulation**

![gtkgood_muxnetlist](https://user-images.githubusercontent.com/104454253/166206120-d45f4a7e-9f9a-4392-ba28-dcdd45777ded.JPG)

## 5.3 Lab- Synthesis simulation mismatch blocking statement

Here the output is depending on the past value of x which is dependednt on a and b and it appears like a flop.

# Example4

	module blocking_caveat (input a , input b , input  c, output reg d); 
	reg x;
	always @ (*)
		begin
		d = x & c;
		x = a | b;
	end
	endmodule

**Simulation**

![gtkwaveblockingcaveat](https://user-images.githubusercontent.com/104454253/166208218-70206c9a-ceb7-436c-8119-9da2eb2d166f.JPG)

**Synthesis**

![synthesisblocking_caveat](https://user-images.githubusercontent.com/104454253/166208245-a5b60e7d-68f3-41bf-950e-bd86632ccbae.JPG)

**Netlist Simulation**

![gtkwaveblocking_caveat_netlist](https://user-images.githubusercontent.com/104454253/166208291-d7e20448-3617-4001-8f31-f9f91cc0cb95.JPG)

**MISMATCH**

![Capture2](https://user-images.githubusercontent.com/104454253/166260812-1832a4ba-8f95-4de3-8129-6b6c39ed17ce.JPG)

# 6. DAY5- if, case, for loop and for generate

## 6.1 If and Case constructs

### 6.1.1 If construct
The construct **if** is mainly used to create priority logic. In a nested if else construct, the conditions are given priority from top to bottom. Only if the condition is satisfied, if statement is executed and the compiler comes out of the block. If condition fails, it checks for next condition and so on as shown below.

**Syntax for nested if else**

	if (<condition 1>)
	begin
	-----------
	-----------
	end
	else if (<condition 2>)
	begin
	-----------
	-----------
	end
	else if (<condition 3>)
	.
	.
	.
	
**Dangers with IF**:

If use a bad coding style i.e, using incomplete if else constructs will infer a latch. We definetly don't require an unwanted latch in a combinational circuit.
When an incomplete construct is used, if all the conditions are failed, the input is latched to the output and hence we don't get desired output unless we need a latch.

This can be shown in below example:

![0ed60c6c-b272-4bc6-939b-df13f76fc063](https://user-images.githubusercontent.com/104454253/166262011-21800dd6-0427-4558-a75e-6bf928117bba.jpg)

### 6.1.2 Case construct

**Syntax**

	case(statement)
	  case1: begin
  	       --------
		 --------
		 end
 	 case2: begin
    	     --------
		 --------
		 end
 	 default:
	 endcase
 
 In case construct, the execution checks for all the case statements and whichever satisfies the statement, that particular statement is executed.If there is no match, the default statement is executed. But here unlike if construct, the execution doesn't stop once statement is satisfied, but it continues further.
 
**Caveats in Case**<br />
Caveats in case occur due to two reasons. One is **incomplete case statements** and the other is **partial assignments in case statements.**

## 6.2 Lab- Incomplete IF

This incomplete if construct forms a connection between i0 and output y i.e, D-latch with input as i1 and i0 will be the enable for it.<br />
**Example-1**

	module incomp_if (input i0 , input i1 , input i2 , output reg y);
	always @ (*)
	begin
		if(i0)
			y <= i1;
	end
	endmodule

**Simulation**

![simulationincomif](https://user-images.githubusercontent.com/104454253/166212734-9af50022-ebdd-4546-9227-25a35759d8f6.JPG)

**Synthesis**

![sythesisincomp_if](https://user-images.githubusercontent.com/104454253/166212767-d60021fa-9102-4edb-83c7-cd87b3439f24.JPG)

**Example-2**<br />
The below code is equivalent to two 2:1 mux with i0 and i2 as select lines with i1 and i3 as inputs respectively. Here as well, the output is connected back to input in the form of a latch with an enable input of **OR** of i0 and i2.

	module incomp_if2 (input i0 , input i1 , input i2 , input i3, output reg y);
		always @ (*)
		begin
			if(i0)
				y <= i1;
			else if (i2)
				y <= i3;
		end
	endmodule

**Simulation**

![gtkwaveincomp_if2](https://user-images.githubusercontent.com/104454253/166212933-520affd3-c367-4bbf-8ffc-14d30ca05ebe.JPG)

**Synthesis**

![synthesisincomp_if2](https://user-images.githubusercontent.com/104454253/166212941-6ca8d979-1e4b-4ae1-b5f2-a74a229f5629.JPG)

## 6.3 Lab- incomplete overlapping Case

**Example-1**<br />
Thie is an example of incomplete case where other two combinations 10 and 11 were not included. This is infer a latch for the multiplexer and connect i2 and i3 with the output.

	module incomp_case (input i0 , input i1 , input i2 , input [1:0] sel, output reg y);
		always @ (*)
		begin
		case(sel)
			2'b00 : y = i0;
			2'b01 : y = i1;
		endcase
		end
	endmodule

**Simulator**

![simulationincomp_case](https://user-images.githubusercontent.com/104454253/166213607-d8de1148-7963-45b9-88fc-46531de026ce.JPG)

**Synthesis**

![synthesisincom_Case](https://user-images.githubusercontent.com/104454253/166213609-c8221a76-2bc7-4ef6-b519-e76d5ee7211a.JPG)

**Example-2- Complete case**

This is the case of complete case statements as the default case is given. If the actual case statements don't execute, the compiler directly executes the default statements and a latch is not inferred.

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

**Simulation**

![simulationcomp_case](https://user-images.githubusercontent.com/104454253/166213705-5443daf5-2996-46e3-900a-c0991edb1fb0.JPG)

**Synthesis**

![synthesiscomp_case](https://user-images.githubusercontent.com/104454253/166213716-d37ad0f3-f111-4bd5-bf73-c77558a48909.JPG)

**Example-3**<br />
In the below example, y is present in all the case statements and it had particular outut for all cases. There no latch is inferred in case of y. 
When it comes to x, it is not assigned for the input 01, therefore a latch is inferred here.

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

**Simulation**

![simulationpartialcaseassign](https://user-images.githubusercontent.com/104454253/166214052-38f2b8ff-af9e-445a-be5e-699021c7a3ef.JPG)

**Synthesis**

![synthesispartial_case_assign](https://user-images.githubusercontent.com/104454253/166214065-30f2cab1-a8c3-4c6b-92c4-c7b9728ca399.JPG)

**Example-4-Bad case construct**

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
	
**Simulation**

![simulationbad_case](https://user-images.githubusercontent.com/104454253/166214296-1423f7e9-9e53-46ae-9f2d-5ca7d8d364d4.JPG)

**Synthesis**

![synthesisbad_case](https://user-images.githubusercontent.com/104454253/166214308-b4232ff5-300c-426d-9a58-e612a90cfd77.JPG)

**Netlist simulation**

![simulationbad_case_netlist](https://user-images.githubusercontent.com/104454253/166214909-3c04f425-ea44-4456-8c76-7b81b236b86a.JPG)

## 6.4 For Loop and For Generate

**For Loop**<br />
- For look is used in always block
- It is used for excecuting expressions alone

**Generate For loop**<br />
- Generate for loop is used for instantaing hardware
- It should be used only outside always block

For loop can be used to generate larger circuits like 256:1 multiplexer or 1-256 demultiplexer where the coding style of smaller mux is not feesible and can have human errors since we would need to include huge number of combinations.

FOR Generate can be used to instantiate any number of sub modules with in a top module. For example, if we need a 32 bit ripple carry adder, instead of instantiating 32 full adders, we can write a generate for loop and connect the full adders appropriately.

## 6.5 Lab- For and For Generate

**Example-1- Mux using generate**<br />
Here for loop is used to design a 4:1 mux. This can also be written using case or if else block, however, for a large size mux, only for loop model is feasible.

	module mux_for (input i0 , input i1, input i2 , input i3 , input [1:0] sel  , output reg y);
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

**Simulation**

![simulation_mux_generate](https://user-images.githubusercontent.com/104454253/166215022-1f6a3eff-2d8f-445d-9791-1fa84d997325.JPG)

**Synthesis**

![synthesis_mux_generate](https://user-images.githubusercontent.com/104454253/166215028-2f33e2ac-5094-42e0-b765-1a69da3d7eb9.JPG)

**Netlist Simulation**

![simulation_mux_generate_netlist](https://user-images.githubusercontent.com/104454253/166215037-7c6da2c4-65fd-4ec9-915b-6d02b9df515f.JPG)

**Example-2-Demux using Case**

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

**Simulation**

![sim_demux_case](https://user-images.githubusercontent.com/104454253/166215304-63d9a1ef-6840-4d13-a096-7f728a706d76.JPG)

**Synthesis**

![synthdemux_case](https://user-images.githubusercontent.com/104454253/166215350-db45ff4f-6346-45ed-8d0d-40db10039446.JPG)

**Netlist Simulation**

**Example-3-Demux using Generate**

The code in above example is big and also there is a chance of human error wile writing the code. However, using for loop as shown below, this drawback can be elimiated to a great extent.

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

**Simulation**

![simulationdemux_generate](https://user-images.githubusercontent.com/104454253/166215467-b947a96a-cce8-4dab-b551-8d05109bfc7c.JPG)

**Synthesis**

![synthdemux_generate](https://user-images.githubusercontent.com/104454253/166215477-0cb58974-29e0-42ae-adf9-c96e1addefb5.JPG)

**Example-4- Ripple carry adder using fulladder**

In this Ripple carry adder example, unlike instantiating fulladder for 8 times, generate for loop is used to instantiate the fulladder for 7 times and only for first full adder, it is instantiated seperately. Using the same code, just by changing bus sizes and condition of for loop, we can design any required size of ripple carry adder.

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

**Simulation**

![sim_rca](https://user-images.githubusercontent.com/104454253/166215641-bb752eb7-9709-4c0a-a6b7-cce9cb585097.JPG)

**Synthesis**

![synth_rca](https://user-images.githubusercontent.com/104454253/166215658-dc566124-519d-477d-8e4a-7b990b40c191.JPG)

**Netlist Simulation**

![sim_rca_netlist](https://user-images.githubusercontent.com/104454253/166215690-7afb3dc5-9890-42b7-acd7-cc31b336f6e3.JPG)

# 7. Word of Thanks

I would like to thanks the team of VLSI System Design and Chipspirit for the collaborative workshop. The sessions are well structure and I'm looking forward for future events. My special thanks to Mr. Kunal and Mr. Shon Taware for helping us out with the flow and technical issues. I wish VSD team and Chipspirit team all the very best for your **MAKE IN INDIA** vision.
