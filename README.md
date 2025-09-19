# SIMULATION AND IMPLEMENTATION OF 4:1 MULTIPLEXER

## AIM
To design and simulate a 4:1 Multiplexer (MUX) using Verilog HDL in four different modeling styles—Gate-Level, Data Flow, Behavioral, and Structural—and to verify its functionality through a testbench using the Vivado 2023.1 simulation environment. The experiment aims to understand how different abstraction levels in Verilog can be used to describe the same digital logic circuit and analyze their performance.

## APPARATUS REQUIRED
- **Vivado 2023.1**

## Procedure

1. Open **Vivado 2023.1**.  
2. Create a **New RTL Project** and give a name (e.g., `Mux4_to_1`).  
3. Add/create your Verilog files and testbench.  
4. Select an FPGA part (e.g., `xc7a35ticsg324-1L`).  
5. Run **Synthesis** to check for errors.  
6. Run **Simulation** → **Run Behavioral Simulation**.  
7. Observe the waveforms of inputs and outputs.  
8. Adjust simulation time if needed (e.g., 1000ns).  
9. Save the project and take screenshots of results.  
10. Close simulation.  

---

## Logic Diagram
![image](https://github.com/user-attachments/assets/d4ab4bc3-12b0-44dc-8edb-9d586d8ba856)

---

## Truth Table
![image](https://github.com/user-attachments/assets/c850506c-3f6e-4d6b-8574-939a914b2a5f)

---

## Verilog Code

### 4:1 MUX Gate-Level Implementation
```verilog
// Gate Level Modelling - Skeleton
module mux4_gate (
    input  wire I0, I1, I2, I3,
    input  wire S0, S1,
    output wire Y
);

    wire S0n, S1n;
    wire w0, w1, w2, w3;

    not (S0n, S0);
    not (S1n, S1);

    and (w0, I0, S0n, S1n); 
    and (w1, I1, S0,  S1n); 
    and (w2, I2, S0n, S1);  
    and (w3, I3, S0,  S1);  

    or (Y, w0, w1, w2, w3);

endmodule

```
### 4:1 MUX Gate-Level Implementation- Testbench
```verilog
// Testbench Skeleton
`timescale 1ns/1ps
module tb_mux4_gate;

    reg I0, I1, I2, I3;
    reg S0, S1;
    wire Y;

    mux4_gate uut (
        .I0(I0), .I1(I1), .I2(I2), .I3(I3),
        .S0(S0), .S1(S1),
        .Y(Y)
    );

    initial begin
        I0 = 0; I1 = 0; I2 = 0; I3 = 0;
        S0 = 0; S1 = 0;

        #10 I0=1; S0=0; S1=0;   
        #10 I0=0; I1=1; S0=1; S1=0; 
        #10 I1=0; I2=1; S0=0; S1=1; 
        #10 I2=0; I3=1; S0=1; S1=1; 
        #10;

        $stop;
    end

endmodule
```
## Simulated Output Gate Level Modelling

<img width="1855" height="1199" alt="gate mux" src="https://github.com/user-attachments/assets/a2cc153e-8a75-44ec-a991-da4dec6d3c10" />

---
### 4:1 MUX Data flow Modelling
```
module mux4_dataflow (
    input  wire I0, I1, I2, I3,
    input  wire S0, S1,
    output wire Y
);

    assign Y = (S1 == 0 && S0 == 0) ? I0 :
               (S1 == 0 && S0 == 1) ? I1 :
               (S1 == 1 && S0 == 0) ? I2 :
                                      I3; 

endmodule

```
### 4:1 MUX Data flow Modelling- Testbench
```verilog
// Testbench Skeleton
`timescale 1ns/1ps
module tb_mux4_dataflow;

    reg I0, I1, I2, I3;
    reg S0, S1;
    wire Y;

    mux4_dataflow uut (
        .I0(I0), .I1(I1), .I2(I2), .I3(I3),
        .S0(S0), .S1(S1),
        .Y(Y)
    );

    initial begin
        I0=1; I1=0; I2=0; I3=0; S0=0; S1=0;
        #10;

        I0=0; I1=1; I2=0; I3=0; S0=1; S1=0;
        #10;

        I0=0; I1=0; I2=1; I3=0; S0=0; S1=1;
        #10;

        I0=0; I1=0; I2=0; I3=1; S0=1; S1=1;
        #10;

        $stop; 
    end

endmodule

 
```
## Simulated Output Dataflow Modelling

<img width="1850" height="1190" alt="mux data flow" src="https://github.com/user-attachments/assets/587da666-1330-4cf7-8bfe-d7c373397178" />

---
### 4:1 MUX Behavioral Implementation
```verilog
`timescale 1ns / 1ps


module MUX_4_1(i,s,y);
input [3:0]i;
input [1:0]s;
output reg y;
always@(*)
    begin
        case(s)
        2'b00:y=i[0];
        2'b01:y=i[1];
        2'b10:y=i[2];
        2'b11:y=i[3];
        endcase
     end
endmodule
```
### 4:1 MUX Behavioral Modelling- Testbench
```verilog
module tb_mux_4_1;
reg [3:0] i;
reg [1:0] s;
wire y;
MUX_4_1 uut(i, s, y);
initial 
begin
i = 4'b1011;
s = 2'b00; 
#10;
$display("Selection is %b %b- Output = %b", s[1],s[0], y);
s = 2'b01; 
#10;
$display("Selection is %b %b - Output = %b", s[1],s[0] ,y);
s = 2'b10; 
#10;
$display("Selection is %b %b - Output = %b", s[1],s[0], y);

s = 2'b11; 
#10;
$display("Selection is %b %b - Output = %b", s[1],s[0], y);
$finish;
end
endmodule

```
## Simulated Output Behavioral Modelling

<img width="1918" height="1198" alt="image" src="https://github.com/user-attachments/assets/78838708-68c0-44be-92c4-f5ce6efa5d94" />



### 4:1 MUX Structural Implementation

![image](https://github.com/user-attachments/assets/eea81c2c-7dfa-43aa-9cea-1ab4ed54db6c)


```verilog
module mux2_to_1 (
    input wire A,
    input wire B,
    input wire S,
    output wire Y
);
    assign Y = S ? B : A;
endmodule
```
### Testbench Implementation
```verilog
`timescale 1ns / 1ps
module mux4_to_1_structural (
    input wire A,
    input wire B,
    input wire C,
    input wire D,
    input wire S0,
    input wire S1,
    output wire Y
);
    wire w1, w2;

    
    mux2_to_1 m1 (.A(A), .B(B), .S(S0), .Y(w1)); 
    mux2_to_1 m2 (.A(C), .B(D), .S(S0), .Y(w2)); 

   
    mux2_to_1 m3 (.A(w1), .B(w2), .S(S1), .Y(Y));

endmodule
module mux4_to_1_tb;
    reg A, B, C, D, S0, S1;
    wire Y_structural;

    
    mux4_to_1_structural uut (
        .A(A), .B(B), .C(C), .D(D),
        .S0(S0), .S1(S1),
        .Y(Y_structural)
    );

    initial begin
        
        A=0; B=0; C=0; D=0; S0=0; S1=0;

        
        A=1; B=0; C=0; D=0; S0=0; S1=0;
        #10;

       
        A=0; B=1; C=0; D=0; S0=1; S1=0;
        #10;

        
        A=0; B=0; C=1; D=0; S0=0; S1=1;
        #10;

       
        A=0; B=0; C=0; D=1; S0=1; S1=1;
        #10;

        $stop;
    end
endmodule
```
## Simulated Output Structural Modelling

<img width="1828" height="1049" alt="mux struct" src="https://github.com/user-attachments/assets/38b748c6-b77e-4911-9913-481ae86b3711" />

---
### CONCLUSION

In this experiment, a 4:1 Multiplexer was successfully designed and simulated using Verilog HDL across four different modeling styles: Gate-Level, Data Flow, Behavioral, and Structural.The simulation results verified the correct functionality of the MUX, with all implementations producing identical outputs for the given input conditions. of the MUX, with all implementations producing identical outputs for the given input conditions.

