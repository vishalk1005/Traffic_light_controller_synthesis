# Traffic_light_controller_Synthesis

## Aim:

Synthesize Traffic Light Controller design using Constraints and analyse area and Power reports.

## Tool Required:

Functional Simulation: Incisive Simulator (ncvlog, ncelab, ncsim)

Synthesis: Genus

### Step 1: Getting Started

Synthesis requires three files as follows,

◦ Liberty Files (.lib)

◦ Verilog/VHDL Files (.v or .vhdl or .vhd)

#### traffic.v.txt

```
`timescale 1 ns / 1 ps
module TrafficLight(input clk, //LED_NS represent the North-South LEDs
		    input rst, //LED_WE represent the West-East LEDs
		    output reg [2:0] LED_NS, LED_WE);
/*Let 100 = Red
      010 = Yellow
      001 = Green 
We have 6 different states, defining them below */

parameter S0 = 6'b000001,   //When NS is green and EW is red
	  S1 = 6'b000010,   //When NS is yellow and EW is red
	  S2 = 6'b000100,   //When NS is red and EW is red
	  S3 = 6'b001000,   //When NS is red and EW is green
	  S4 = 6'b010000,   //When NS is red and EW is yellow
	  S5 = 6'b100000;   //When NS is red and EW is red
//Then the cycle repeats

reg [5:0] state;
reg [3:0] count;

//Defining transition of states
always@(posedge clk or posedge rst)
begin
if(rst) begin    //Active HIGH reset
state <= S0;
count <= 0; end
else 
begin
case(state)
S0: begin 
	if(count == 4'd14)
	begin count <= 0;
	state <= S1; end
	else begin 
	count <= count + 1; state <= S0; end end
S1: begin 
	if(count == 4'd2)
	begin count <= 0;
	state <= S2; end
	else begin 
	count <= count + 1; state <= S1; end end
S2: begin 
	if(count == 4'd2)
	begin count <= 0;
	state <= S3; end
	else begin 
	count <= count + 1; state <= S2; end end
S3: begin 
	if(count == 4'd14)
	begin count <= 0;
	state <= S4; end
	else begin 
	count <= count + 1; state <= S3; end end
S4: begin 
	if(count == 4'd2)
	begin count <= 0;
	state <= S5; end
	else begin 
	count <= count + 1; state <= S4; end end
S5: begin 
	if(count == 4'd2)
	begin count <= 0;
	state <= S0; end
	else begin 
	count <= count + 1; state <= S5; end end
endcase
end
end
/*    100 = Red
      010 = Yellow
      001 = Green     */

//Output of LEDs//

always@(*)
begin
case(state) 
S0: begin LED_NS = 3'b001;  LED_WE = 3'b100; end
S1: begin LED_NS = 3'b010;  LED_WE = 3'b100; end
S2: begin LED_NS = 3'b100;  LED_WE = 3'b100; end
S3: begin LED_NS = 3'b100;  LED_WE = 3'b001; end
S4: begin LED_NS = 3'b100;  LED_WE = 3'b010; end
S5: begin LED_NS = 3'b100;  LED_WE = 3'b100; end
endcase
end

endmodule
```
### Step 2 : Creating an SDC File

•	In your terminal type “gedit input_constraints.sdc” to create an SDC File if you do not have one.

### Step 3 : Performing Synthesis

The Liberty files are present in the library path,

• The Available technology nodes are 180nm ,90nm and 45nm.

• In the terminal, initialise the tools with the following commands if a new terminal is being used.

◦ csh

◦ source /cadence/install/cshrc

• The tool used for Synthesis is “Genus”. Hence, type “genus -gui” to open the tool.

• Genus Script file with .tcl file Extension commands are executed one by one to synthesize the netlist.

#### Synthesis RTL Schematic :
![IMG-20241119-WA0019](https://github.com/user-attachments/assets/d153a3db-613d-4418-8a29-65a173339770)
#### Area report:
![IMG-20241119-WA0009](https://github.com/user-attachments/assets/e7bf2e49-0468-4e96-a8f8-1a3b1d928988)
Power Report:
![IMG-20241119-WA0018](https://github.com/user-attachments/assets/67bf89bf-f230-4d85-80cc-be5f8967f1f0)
### Result:

The generic netlist of Traffic Light Controller has been created, and area, power reports have been tabulated and generated using Genus.
