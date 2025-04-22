![image](https://github.com/user-attachments/assets/ae62e536-c6dd-44e5-ba76-fc969c1544ed)# Counter_4bit_up-down

## Aim:

To write a verilog code for 4bit up/down counter and verify the functionality using Test bench. 

## Tools used for ASIC Flow:

1.	nclaunch- Used for Functional Simulation
   
## Design Information and Bock Diagram:

	An up/down counter is a digital counter which can be set to count either from 0 to
MAX_VALUE or MAX_VALUE to 0.

	The direction of the count(mode) is selected using a single bit input. The module has 3 inputs - clk, reset which is active high and a Up Or Down mode input. 
The output is Counter which is 4 bit in size.

	When Up mode is selected, counter counts from 0 to 15 and then again from 0 to 15.

	When Down mode is selected, counter counts from 15 to 0 and then again from 15 to 0.

	Changing mode doesn't reset the Count value to zero.

	You have to apply high value to reset, to reset the Counter output.
 
![image](https://github.com/user-attachments/assets/efe1095e-989e-4005-b53b-e9dc50d4025c)

## Fig 1: 4 Bit Up/Down Counter

## Creating a Work space :

	Create a folder in your name (Note: Give folder name without any space) and Create a new sub-Directory name it as Exp2 or counter_design for the Design and open a terminal from the Sub-Directory.
Functional Simulation: 

	Invoke the cadence environment by type the below commands 

	tcsh (Invokes C-Shell) 

	source /cadence/install/cshrc (mention the path of the tools) 
      (The path of cshrc could vary depending on the installation destination)
      
	After this you can see the window like below 


## Fig 2: Invoke the Cadence Environment

![Screenshot 2025-04-22 180754](https://github.com/user-attachments/assets/0dfa0fb5-afb3-4b92-bf1f-8c54b92db5cf)


## Creating Source Code:

	In the Terminal, type gedit <filename>.v or <filename>.vhdl depending on the HDL Language you are to use (ex: 4b_up_downCount.v).

	A Blank Document opens up into which the following source code can be typed down.

(Note : File name should be with HDL Extension)

### Verilog code for 4-Bit Up-Down Counter:

`timescale 1ns / 1ps
module up_down_counter(
    input wire clk,       
    input wire reset,     
    input wire up_down,   
    input wire enable,    
    output reg [3:0] count 
);


    always @(posedge clk or posedge reset) begin
        if (reset) 
        begin
            count <= 4'b0000;
        end

        else if (enable)
        begin
            if (up_down)
            begin
                count <= count + 1'b1;
            end
            else begin
                count <= count - 1'b1;
            end
        end
    end

endmodule


## Creating Test bench:

### Test-bench code for 4-Bit Up-Down Counter:

`timescale 1ns / 1ps

module up_down_counter_tb();

    reg clk;
    reg reset;
    reg up_down;
    reg enable;
    wire [3:0] count;
    

    up_down_counter uut(
        .clk(clk),
        .reset(reset),
        .up_down(up_down),
        .enable(enable),
        .count(count)
    );
    

    initial begin
        clk = 0;
        forever #5 clk = ~clk;  
    end
    

    initial begin

        reset = 1;
        up_down = 1;
        enable = 0;
        

        #20 reset = 0;
        

        #10 enable = 1;
        #200;  
        

        up_down = 0;
        #200;  
        

        enable = 0;
        #50;
        

        reset = 1;
        #20 reset = 0;
        

        #50;
        

        $finish;
    end
    

    initial begin
        $monitor("Time: %t, Reset: %b, Up/Down: %b, Enable: %b, Count: %b", 
                 $time, reset, up_down, enable, count);
    end
    
endmodule

### To Launch Simulation tool
	linux:/> nclaunch -new&            // “-new” option is used for invoking NCVERILOG for the first time for any design

	linux:/> nclaunch&                 // On subsequent calls to NCVERILOG

It will invoke the nclaunch window for functional simulation we can compile,elaborate and simulate it using Multiple step

## Fig 3: Setting Multi-step simulation

![Screenshot 2025-04-22 180855](https://github.com/user-attachments/assets/3f266b1d-eea7-49d8-92e6-db8d85249792)

![Screenshot 2025-04-22 181128](https://github.com/user-attachments/assets/27e440c1-5592-4b58-beb5-5e51af40c702)

Select Multiple Step and then select “Create cds.lib File” as shown in below figure

Click the cds.lib file and save the file by clicking on Save option

## Fig 4: cds.lib file Creation

![Screenshot 2025-04-22 181212](https://github.com/user-attachments/assets/de1fb214-8518-43ba-b2f9-1d86206d69ea)

	Save cds.lib file and select the correct option for cds.lib file format based on the  HDL Language and Libraries used.

	Select “Don’t include any libraries (verilog design)” from “New cds.lib file” and click on “OK” as in below figure

	We are simulating verilog design without using any libraries

## Fig 5: Selection of Don’t include any libraries


![Screenshot 2025-04-22 181212](https://github.com/user-attachments/assets/b6738d0a-4001-4cf3-935a-5c86844326a1)




	A Click “OK” in the “nclaunch: Open Design Directory” window

	A ‘NCLaunch window’ appears as shown in figure below

	Left side you can see the HDL files. Right side of the window has worklib and snapshots directories listed.

	Worklib is the directory where all the compiled codes are stored while Snapshot will have output of elaboration which in turn goes for simulation

## Fig 6: Nclaunch Window

![Screenshot 2025-04-22 181333](https://github.com/user-attachments/assets/879edc84-cce9-4dac-b9f3-208d442b9752)


To perform the function simulation, the following three steps are involved Compilation, Elaboration and Simulation.

## Step 1: Compilation:– Process to check the correct Verilog language syntax and usage 

	Inputs: Supplied are Verilog design and test bench codes 

	Outputs: Compiled database created in mapped library if successful, generates report else error reported in log file 

## Steps for compilation:

1. Create work/library directory (most of the latest simulation tools creates automatically)
  
2. Map the work to library created (most of the latest simulation tools creates automatically)
  
3. Run the compile command with compile options

i.e Cadence IES command for compile: ncverilog +access+rwc -compile fa.v

Left side select the file and in Tools : launch verilog compiler with current selection will get enable. Click it to compile the code 

Worklib is the directory where all the compiled codes are stored while Snapshot will have output of elaboration which in turn goes for simulation 

## Fig 7: Compiled database in worklib


![Screenshot 2025-04-22 181554](https://github.com/user-attachments/assets/567d19db-bf59-461e-af32-001d87a68184)


	After compilation it will come under worklib you can see in right side window

	Select the test bench and compile it. It will come under worklib. Under Worklib you can see the module and test-bench. 

	The cds.lib file is an ASCII text file. It defines which libraries are accessible and where they are located.
It contains statements that map logical library names to their physical directory paths. For this Design, you will define a library called “worklib”

## Step 2: Elaboration:– To check the port connections in hierarchical design 

	Inputs: Top level design / test bench Verilog codes

	Outputs: Elaborate database updated in mapped library if successful, generates report else error reported in log file 

	Steps for elaboration – Run the elaboration command with elaborate options 

1.	It builds the module hierarchy
	
3.	Binds modules to module instances
  
5.	Computes parameter values
  
7.	Checks for hierarchical names conflicts
  
9.	It also establishes net connectivity and prepares all of this for simulation
    
	After elaboration the file will come under snapshot. Select the test bench and simulate it. 

## Fig 8: Elaboration Launch Option

![Screenshot 2025-04-22 181633](https://github.com/user-attachments/assets/6e01a269-52db-49e6-8370-feea8aa01f3d)

### Step 3: Simulation: – Simulate with the given test vectors over a period of time to observe the output behaviour. 

	Inputs: Compiled and Elaborated top level module name 

	Outputs: Simulation log file, waveforms for debugging 

	Simulation allow to dump design and test bench signals into a waveform 

	Steps for simulation – Run the simulation command with simulator options

## Fig 9: Design Browser window for simulation

![Screenshot 2025-04-22 181711](https://github.com/user-attachments/assets/c4e3e5cc-992d-43f0-87a6-7c1d3274fbc0)



### Result

The functionality of a 4bit_up-down asynchronous reset Counter was successfully verified using a test bench and simulated with the nclaunch tool.

