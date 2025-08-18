# MUX-ULA  
8x1 Multiplexer and ALU implemented in Verilog, with testbench and simulation.

## MUX  
The MUX was described in Verilog with:  

- **Eight 4-bit inputs:** `f0` to `f7`  
- **Three selection inputs:** `sel0`, `sel1`, `sel2`  
- **One 4-bit output:** `op`  

For each bit of `op`, a Boolean expression was assigned based on the selection inputs.  
The logic follows the truth table shown below:  

| x | y | z | Output (`op`) |
|------|------|------|---------------|
|  0   |  0   |  0   | f0            |
|  0   |  0   |  1   | f1            |
|  0   |  1   |  0   | f2            |
|  0   |  1   |  1   | f3            |
|  1   |  0   |  0   | f4            |
|  1   |  0   |  1   | f5            |
|  1   |  1   |  0   | f6            |
|  1   |  1   |  1   | f7            |

### Verilog description
    module mux8x1_4b (f0, f1, f2, f3, f4, f5, f6, f7, sel0, sel1, sel2, op);
    
    input [3:0] f0, f1, f2, f3, f4, f5, f6, f7;
    input sel0, sel1, sel2;
    output [3:0] op;
    
    assign op[3] = ~sel2 & ~sel1 & ~sel0 & f0[3] | ~sel2 & ~sel1 &  sel0 & f1[3] |
                   ~sel2 &  sel1 & ~sel0 & f2[3] | ~sel2 &  sel1 &  sel0 & f3[3] |
                    sel2 & ~sel1 & ~sel0 & f4[3] |  sel2 & ~sel1 &  sel0 & f5[3] |
                    sel2 &  sel1 & ~sel0 & f6[3] |  sel2 &  sel1 &  sel0 & f7[3];

    assign op[2] = ~sel2 & ~sel1 & ~sel0 & f0[2] | ~sel2 & ~sel1 &  sel0 & f1[2] |
                   ~sel2 &  sel1 & ~sel0 & f2[2] | ~sel2 &  sel1 &  sel0 & f3[2] |
                    sel2 & ~sel1 & ~sel0 & f4[2] |  sel2 & ~sel1 &  sel0 & f5[2] |
                    sel2 &  sel1 & ~sel0 & f6[2] |  sel2 &  sel1 &  sel0 & f7[2];

    assign op[1] = ~sel2 & ~sel1 & ~sel0 & f0[1] | ~sel2 & ~sel1 &  sel0 & f1[1] |
                   ~sel2 &  sel1 & ~sel0 & f2[1] | ~sel2 &  sel1 &  sel0 & f3[1] |
                    sel2 & ~sel1 & ~sel0 & f4[1] |  sel2 & ~sel1 &  sel0 & f5[1] |
                    sel2 &  sel1 & ~sel0 & f6[1] |  sel2 &  sel1 &  sel0 & f7[1];

    assign op[0] = ~sel2 & ~sel1 & ~sel0 & f0[0] | ~sel2 & ~sel1 &  sel0 & f1[0] |
                   ~sel2 &  sel1 & ~sel0 & f2[0] | ~sel2 &  sel1 &  sel0 & f3[0] |
                    sel2 & ~sel1 & ~sel0 & f4[0] |  sel2 & ~sel1 &  sel0 & f5[0] |
                    sel2 &  sel1 & ~sel0 & f6[0] |  sel2 &  sel1 &  sel0 & f7[0];
    endmodule

## RTL viewer
  [image]

## Testbench

    `timescale 1ns/1ps

    module tb_mux8x1_4b;
    reg [3:0] f0, f1, f2, f3, f4, f5, f6, f7;
    reg sel0, sel1, sel2;
    wire [3:0] op;

    mux8x1_4b uut (
        .f0(f0), .f1(f1), .f2(f2), .f3(f3), 
        .f4(f4), .f5(f5), .f6(f6), .f7(f7),
        .sel0(sel0), .sel1(sel1), .sel2(sel2),
        .op(op)
    );

    initial begin
        $dumpfile("mux_tb.vcd");
        $dumpvars(0, tb_mux8x1_4b);

        // Estímulos
        f0 = 4'b0000; f1 = 4'b0001; f2 = 4'b0010; f3 = 4'b0011;
        f4 = 4'b0100; f5 = 4'b0101; f6 = 4'b0110; f7 = 4'b0111;

        sel2 = 0; sel1 = 0; sel0 = 0; #10;
        sel2 = 0; sel1 = 0; sel0 = 1; #10;
        sel2 = 0; sel1 = 1; sel0 = 0; #10;
        sel2 = 0; sel1 = 1; sel0 = 1; #10;
        sel2 = 1; sel1 = 0; sel0 = 0; #10;
        sel2 = 1; sel1 = 0; sel0 = 1; #10;
        sel2 = 1; sel1 = 1; sel0 = 0; #10;
        sel2 = 1; sel1 = 1; sel0 = 1; #10;

        $finish;
    end
    endmodule

## Results

[image]

## ULA  
The ULA was described in Verilog with:  

- **Two 4-bit inputs:** `a`, `b`  
- **Three selection inputs:** `x`, `y`, `z`  
- **One 4-bit output:** `s`  
- **Eight internal wires (`d0–d7`)**, each corresponding to one arithmetic or logical operation.  

The wires are connected to the inputs of the multiplexer (`mux8x1_4b`), which selects the desired operation according to the selection inputs.  

| Wire | Operation |
|------|------------|
| d0   | a + b      |
| d1   | a - b      |
| d2   | a << b     |
| d3   | a >> b     |
| d4   | a & b      |
| d5   | a \| b     |
| d6   | a ^ b      |
| d7   | ~a         |

### Verilog description
```verilog
module ula (a,b,x,y,z,s);
    input [3:0] a, b;
    input x, y, z;
    output [3:0] s;

    wire [3:0] d0,d1,d2,d3,d4,d5,d6,d7;

    assign d0 = a + b;
    assign d1 = a - b;
    assign d2 = a << b;
    assign d3 = a >> b;
    assign d4 = a & b;
    assign d5 = a | b;
    assign d6 = a ^ b;
    assign d7 = ~a;

    mux8x1_4b muxout(
        .f0(d0), .f1(d1), .f2(d2), .f3(d3),
        .f4(d4), .f5(d5), .f6(d6), .f7(d7),
        .sel0(x), .sel1(y), .sel2(z), .op(s)
    );
endmodule


## TestBench
    `timescale 1ns/1ps

module tb_ula;

    reg [3:0] a, b;
    reg x, y, z;
    wire [3:0] s;

    // Instancia a ULA
    ula dut (
        .a(a),
        .b(b),
        .x(x),
        .y(y),
        .z(z),
        .s(s)
    );

    initial begin
        // arquivo de dump para GTKWave
        $dumpfile("ula_tb.vcd");
        $dumpvars(0, tb_ula);

        // valores de entrada fixos
        a = 4'b1010; // 10 decimal
        b = 4'b0011; // 3 decimal

        // header para console
        $display("Time | sel zyx | a    | b    | s");
        $display("------------------------------------");
        $monitor("%4t |  %b%b%b   | %b | %b | %b", 
                  $time, z, y, x, a, b, s);

        // percorre as 8 combinações de seleção
        {z,y,x} = 3'b000; #10;  // soma
        {z,y,x} = 3'b001; #10;  // subtração
        {z,y,x} = 3'b010; #10;  // shift left
        {z,y,x} = 3'b011; #10;  // shift right
        {z,y,x} = 3'b100; #10;  // AND
        {z,y,x} = 3'b101; #10;  // OR
        {z,y,x} = 3'b110; #10;  // XOR
        {z,y,x} = 3'b111; #10;  // NOT a

        $finish;
    end

    endmodule

## RTL viewer
  [image]

## Results

[image]
