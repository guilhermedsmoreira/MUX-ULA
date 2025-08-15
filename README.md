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
