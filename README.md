# EXPERIMENT 3: Simulation of All Flip-Flops using Blocking Statement

## AIM
To design and simulate basic flip-flops (SR, D, JK, and T) using **blocking statements** in Verilog HDL, and verify their functionality through simulation in Vivado 2023.1.

## APPARATUS REQUIRED
- Vivado 2023.1
- Computer with HDL Simulator

## DESCRIPTION
Flip-flops are the basic memory elements in sequential circuits.  
In this experiment, different types of flip-flops (SR, D, JK, T) are modeled using **behavioral modeling** with **blocking assignment (`=`)** inside the `always` block.  
Blocking assignments execute sequentially in the given order, which makes it easier to describe simple synchronous circuits.

## PROCEDURE
1. Open **Vivado 2023.1**.  
2. Create a **New RTL Project** (e.g., `FlipFlop_Simulation`).  
3. Add Verilog source files for each flip-flop (SR, D, JK, T).  
4. Add a testbench file to verify all flip-flops.  
5. Run **Behavioral Simulation**.  
6. Observe waveforms of inputs and outputs for each flip-flop.  
7. Verify that outputs match the truth table.  
8. Save results and capture simulation screenshots.

---

Block Diagram:

<img width="358" height="136" alt="image" src="https://github.com/user-attachments/assets/dcc98336-2c09-49ad-944a-ba2e76753036" />

<img width="572" height="321" alt="image" src="https://github.com/user-attachments/assets/3d9a77b9-4364-4771-8197-3063eb9dc817" />

<img width="437" height="202" alt="image" src="https://github.com/user-attachments/assets/2ebecf28-bbc5-4123-ba4b-cb534ad97492" />
<img width="437" height="202" alt="image" src="https://github.com/user-attachments/assets/8239a322-250f-4070-922f-06b63df3cced" />






VERILOG CODE SR Flip-Flop (Blocking)

```
module srff1(q, s, r, clk, rst);
    input s, r, clk, rst;
    output reg q;

    always @(posedge clk) begin
        if (rst)
            q <= 1'b0;
        else begin
            case ({s, r})
                2'b00: q <= q;
                2'b01: q <= 1'b0;
                2'b10: q <= 1'b1;
                2'b11: q <= 1'bx; // invalid state
                default: q <= 1'bx; // safety default
            endcase
        end
    end
endmodule
```

SR Flip-Flop Test bench

```
module srff1_tb;

  reg clk_t, rst_t, s_t, r_t;
  wire q_t;

  
  srff1 dut (
    .clk(clk_t),
    .rst(rst_t),
    .s(s_t),
    .r(r_t),
    .q(q_t)
  );

  
  initial clk_t = 0;
  always #10 clk_t = ~clk_t;

  
  initial begin
    rst_t = 1;
    s_t = 0;
    r_t = 0;

    #20 rst_t = 0;  // Deactivate reset

    #20 s_t = 1; r_t = 1;  
    #20 s_t = 0; r_t = 1;  
    #20 s_t = 1; r_t = 0;  
    #20 s_t = 0; r_t = 0;  

    #40 $finish;
  end

endmodule
```



SIMULATION OUTPUT

<img width="1205" height="774" alt="image" src="https://github.com/user-attachments/assets/97115db2-c9bd-497c-a853-49a7ab0a22f7" />




JK Flip-Flop (Blocking)

```
module jkf(j, k, q, clk, rst);
  input j, k, clk, rst;
  output reg q;

  always @(posedge clk) begin
    if (rst)
      q = 1'b0;
    else begin
      case ({j, k})
        2'b00: q = q;
        2'b01: q = 1'b0;
        2'b10: q = 1'b1;
        2'b11: q = ~q;
        default: q = 1'bx;
      endcase
    end
  end
endmodule
```
JK Flip-Flop Test bench

```
module jkf_tb;
  reg j_t, k_t, clk_t, rst_t;
  wire q_t;

  jkf dut (
    .j(j_t),
    .k(k_t),
    .clk(clk_t),
    .rst(rst_t),
    .q(q_t)
  );

  initial clk_t = 0;
  always #5 clk_t = ~clk_t;

  initial begin
    rst_t = 1;
    j_t = 0;
    k_t = 0;
    #12 rst_t = 0;

    #10 j_t = 0; k_t = 0;
    #10 j_t = 0; k_t = 1;
    #10 j_t = 1; k_t = 0;
    #10 j_t = 1; k_t = 1;
    #10 j_t = 1; k_t = 1;
    #10 j_t = 0; k_t = 0;
    #10 rst_t = 1;
    #10 rst_t = 0;

    #20 $finish;
  end
endmodule
```


SIMULATION OUTPUT

<img width="1542" height="674" alt="image" src="https://github.com/user-attachments/assets/effc99d1-da06-40be-bc32-a137d5cef33f" />


D Flip-Flop (Blocking)

```
module dff(clk, rst, din, dout);
    input clk, rst, din;
    output reg dout;

    always @(posedge clk)
    begin
        if (rst)
            dout = 1'b0;
        else
            dout = din;
    end
endmodule
```
D Flip-Flop Test bench

```
module dff_tb;

  reg clk, rst, din;
  wire dout;

  dff dut (
    .clk(clk),
    .rst(rst),
    .din(din),
    .dout(dout)
  );

  always #5 clk = ~clk;

  initial begin
    clk = 0;
    rst = 1;
    din = 0;

    #10 rst = 0;
    #10 din = 1;
    #10 din = 0;
    #10 din = 1;
    #10 rst = 1;
    #10 rst = 0;
    #10 din = 0;
    #20 $finish;
  end

endmodule
```

SIMULATION OUTPUT

<img width="1411" height="763" alt="image" src="https://github.com/user-attachments/assets/ae359530-2f64-4662-81ff-4ed54e117a75" />

T Flip-Flop (Blocking)

```
module tff(clk, rst, tin, tout);
  input clk, rst, tin;
  output reg tout;

  always @(posedge clk) begin
    if (rst)
      tout = 1'b0;
    else if (tin)
      tout = ~tout;
    else
      tout = tout;
  end
endmodule
```
T Flip-Flop Test bench

```
module tff_tb;
  reg clk_t, rst_t, tin_t;
  wire tout_t;

  tff dut (.clk(clk_t), .rst(rst_t), .tin(tin_t), .tout(tout_t));

  initial begin
    clk_t = 1'b0;
    rst_t = 1'b1;
    tin_t = 1'b0;

    #20 rst_t = 1'b1;

    #20 rst_t = 1'b0;
    tin_t = 1'b1;

    #20 tin_t = 1'b0;

    #40 $finish;  // End simulation
  end

  always #10 clk_t = ~clk_t;
endmodule
```

SIMULATION OUTPUT

<img width="1536" height="737" alt="image" src="https://github.com/user-attachments/assets/65cd852f-d1f2-452a-9bf4-302315046d73" />


RESULT : All flip-flops (SR, D, JK, T) were successfully simulated using blocking statements in Verilog HDL.The outputs matched the expected truth table values, demonstrating correct sequential behavior.


