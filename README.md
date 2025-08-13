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
