Features
5-stage pipeline:
IF (Instruction Fetch)
ID (Instruction Decode)
EX (Execute)
MEM (Memory Access)
WB (Write Back)
Full RV32I instruction support
RVC (Compressed Instruction) decompressor
Hazard Detection Unit
Data Forwarding Unit
Pipeline Stall Logic
Branch Handling
Synthesizable RTL

Architecture
Instruction Memory
        |
        V
+----------------+
| IF Stage       |
+----------------+
        |
        V
+----------------+
| ID Stage       |
| RVC Expander   |
+----------------+
        |
        V
+----------------+
| EX Stage       |
| ALU            |
+----------------+
        |
        V
+----------------+
| MEM Stage      |
+----------------+
        |
        V
+----------------+
| WB Stage       |
+----------------+

Tools Used
Verilog HDL
Vivado
ModelSim / QuestaSim

Verification
50+ directed test cases
Arithmetic instructions
Branch instructions
Load/Store instructions
Compressed instructions

Results
Successful execution of RV32I programs
Correct hazard handling
Accurate compressed instruction expansion
