```
## Difference Between CISC and RISC (Qualcomm Interview Context)

**CISC (Complex Instruction Set Computer)** and **RISC (Reduced Instruction Set Computer)** are two types of CPU architecture designs.

### CISC:
- Has complex instructions that can perform multiple operations in a single instruction.
- Instructions are often **variable in length** and may take **multiple clock cycles** to execute.
- Focuses on reducing the number of instructions per program, at the cost of more complex hardware.

**Example**:  
- **x86 architecture** (used in Intel processors) is CISC.  
- Instruction like `MUL` can directly multiply two memory operands.

### RISC:
- Uses a smaller set of **simple instructions**, each typically executed in **one clock cycle**.
- Instructions are of **uniform size** and follow a **load/store** architecture.
- Emphasizes compiler optimization and simpler hardware design.

**Example**:  
- **ARM architecture** (used in Qualcomm Snapdragon) is RISC.  
- Multiplication would use multiple instructions like:
  - `LDR` (load data)
  - `MUL` (multiply)
  - `STR` (store result)
```
