📘  Single-Cycle RISC-V Processor (RV32I) in Verilog

🧩 Introduction

This project implements a Single-Cycle RISC-V Processor, adhering to the RV32I instruction set architecture (ISA), in Verilog HDL. The processor is designed to complete one instruction per clock cycle, providing a simplified but complete model for instruction execution without pipelining or hazard detection. It serves as a foundational building block for understanding modern CPU architecture and is suitable for educational and prototyping purposes.

🧠 Architectural Overview

The architecture consists of several synchronous and combinational modules, interconnected to fetch, decode, execute, and write results within a single clock cycle. Each instruction is executed in exactly one clock cycle, regardless of complexity.

Major stages in the datapath:

Instruction Fetch (IF) – PC fetches the instruction from memory.

Instruction Decode (ID) – Decoding, register file reading, and immediate extraction.

Execute (EX) – ALU performs the computation.

Memory Access (MEM) – Load/store instructions interact with memory.

Write Back (WB) – ALU/memory result written back to the register file.

🧱 Module Descriptions

🔹 1. Program Counter (program_counter)
Holds the address of the current instruction.

Updates every cycle to either PC + 4 (next sequential instruction) or branch target if branch condition is met.

🔹 2. PC Adder (pc_adder)
Adds 4 to the current PC to fetch the next instruction.

🔹 3. Branch Adder (branch_adder)
Calculates the target address for conditional branches using the immediate offset.

🔹 4. PC Mux (pc_mux)
Chooses between the next sequential PC and the branch target based on the Branch and Zero control signals.

🔹 5. Instruction Memory (Instruction_Memory)
A ROM array containing 32-bit instructions.

Preloaded with R-, I-, S-, B-, U-, and J-type instructions to test the functionality.

🔹 6. Register File (Register_File)
32 general-purpose 32-bit registers.

Supports simultaneous read of two source registers and write-back to a destination register.

🔹 7. Control Unit (main_control_unit)
Decodes the instruction opcode and generates control signals such as:

RegWrite: Enable writing to a register.

ALUSrc: Choose between immediate or register operand.

MemRead: Enable memory read.

MemWrite: Enable memory write.

MemToReg: Choose between ALU result or memory data for writing back.

Branch: Enable branching.

ALUOp: Informs ALU control how to interpret funct fields.

🔹 8. Immediate Generator (immediate_generator)
Extracts and sign-extends immediate fields from various instruction formats:

I-type, S-type, B-type, U-type, and J-type.

🔹 9. ALU Control (ALU_Control)
Interprets funct3, funct7, and ALUOp to generate a 4-bit signal to configure the ALU operation.

🔹 10. ALU (ALU)
Executes arithmetic and logical operations such as:

ADD, SUB, AND, OR, XOR, SLL, SRL, SRA, SLT

Outputs result and a zero flag for branching.

🔹 11. ALU MUX (MUX2to1)
Chooses between register operand or immediate operand based on ALUSrc.

🔹 12. Data Memory (Data_Memory)
Memory array supporting word-addressed read and write.

Used for load (lw) and store (sw) instructions.

🔹 13. Write-Back MUX (MUX2to1_DataMemory)
Chooses the value to be written back to registers:

From ALU or from Data Memory depending on MemToReg.

📜 Instruction Types & Format Support

Instruction Type	Purpose	Format Fields	Examples
R-type	Arithmetic & Logic (reg-reg)	opcode, funct3, funct7	add, sub, and
I-type	Immediate operations & loads	opcode, imm[11:0]	addi, andi, lw
S-type	Store to memory	imm[11:5], imm[4:0]	sw, sh, sb
B-type	Conditional branches	Discontinuous imm bits	beq, bne
U-type	LUI, AUIPC	Top 20-bit immediate	lui, auipc
J-type	Jumps	Discontinuous 20-bit imm	jal

🧪 Simulation & Testing

✅ Testbench Features (RISCV_Top_Tb):
Generates a clock (50ns half-period).

Initializes and releases the reset.

Runs the processor for sufficient time (>100 clock cycles) to test all instructions.

Generates a VCD (waveform.vcd) file for visualization using GTKWave.

🧰 Simulation Steps:

Compile all modules with Icarus Verilog:
iverilog -o riscv_test RISCV_Top.v

Run the simulation:
vvp riscv_test

View waveforms:
gtkwave waveform.vcd

🛠️ Tools & Technologies
Hardware Description Language: Verilog HDL

Simulation: Icarus Verilog / ModelSim / Vivado

Visualization: GTKWave

Editor: VS Code / Vivado HLS / Notepad++

📌 Learning Outcomes

By building this project, you will:

Understand instruction execution cycle in a CPU.

Learn modular digital design using Verilog.

Practice register-level architecture and control signal coordination.

Get hands-on experience with instruction decoding and memory hierarchy.

Improve debugging skills using waveform analysis.

📈 Performance Characteristics

All operations are executed in a single cycle (no pipelining).

The architecture can be extended to handle:


Additional instruction types (e.g., mul, div)

Pipeline stages

Forwarding and hazard detection

Cache or memory-mapped I/O
