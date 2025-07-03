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
## 3. ARM Core Families
```
                   ┌────────────────────────────────────┐
                   │          ARM Core Families         │
                   └────────────────────────────────────┘
                                 ▲
                                 │
       ┌────────────────────────┼────────────────────────┐
       │                        │                        │
 ┌─────────────┐         ┌──────────────┐         ┌─────────────┐
 │  Cortex-M   │         │  Cortex-R    │         │  Cortex-A   │
 │ (Microcontroller) │   │ (Real-time)  │         │ (Application)│
 └─────────────┘         └──────────────┘         └─────────────┘
       ▲                        ▲                        ▲
       │                        │                        │
 ┌─────────────┐         ┌──────────────┐         ┌─────────────┐
 │ M0 / M0+    │         │ R5 / R7 / R8 │         │ A5 / A7 / A9│
 │ M3 / M4     │         │              │         │ A15 / A17   │
 │ M23 / M33   │         │              │         │ A53 / A57   │
 │ M35P / M55  │                                │ A72 / A73   │
 │ M85         │                                │ A75 / A76   │
                                                  │ A77 / A78   │
                                                  │ X1 / X2     │
                                                  └─────────────┘

       ┌─────────────────────────────┐
       │     Neoverse (Infrastructure CPUs)     │
       └─────────────────────────────┘
                   ▲
        ┌────────────┬────────────┬────────────┐
        │   E-Series │   N-Series │   V-Series │
        │ (Edge)     │ (Cloud)    │ (High Perf)│
        └────────────┴────────────┴────────────┘
```

* Cortex-A: Application processors (smartphones, tablets)
* Cortex-R: Real-time processors (automotive, industrial)
* Cortex-M: Microcontrollers (IoT, wearables, sensors)
* There are three primary lines in the Neoverse portfolio:
* | Family          | Focus                   | Key Features                             | Target Workloads                                |
| ------------------| ------------------------| ---------------------------------------- | ------------------------------------|
| Neoverse V-Series | Performance             | Out-of-order, high IPC, large caches     | AI/ML, cloud workloads              |
| Neoverse N-Series | General Compute         | Balanced performance and efficiency      | Cloud servers, storage              |
| Neoverse E-Series | Efficiency & Throughput | In-order, high core count, dense compute | Edge devices, networking appliances |


## 4. Interview Q: ARM Architecture Overview
```
                    ┌────────────────────────────┐
                    │      ARM Processor Core    │
                    └────────────────────────────┘
                                  ▲
            ┌────────────────────┼────────────────────┐
            │                    │                    │
  ┌────────────────┐   ┌────────────────────┐  ┌────────────────────┐
  │ Instruction     │   │   Data Processing   │   Control Logic     │
  │ Fetch & Decode  │   │ (ALU, Shifter, MAC) │  │ (FSM, Control Unit) │
  └────────────────┘   └────────────────────┘  └────────────────────┘
            ▲                    ▲                    ▲
            │                    │                    │
  ┌────────────────────────────────────────────────────────┐
  │                Register File (General Purpose + SP, PC)│
  └────────────────────────────────────────────────────────┘
                                 ▲
                                 │
                 ┌────────────────────────────────┐
                 │     Load/Store Unit (LSU)      │
                 └────────────────────────────────┘
                                 ▲
                 ┌────────────────────────────────┐
                 │      Memory Interface Unit     │
                 └────────────────────────────────┘
                            ▲             ▲
                            │             │
                ┌────────────────┐ ┌────────────────────┐
                │   Instruction  │ │     Data Memory    │
                │     Memory     │ │   (RAM/Cache/MMU)  │
                └────────────────┘ └────────────────────┘

       ┌────────────────────────────────────────────────────────┐
       │        Interrupt Controller / Exception Handler        │
       └────────────────────────────────────────────────────────┘

       ┌────────────────────────────────────────────────────────┐
       │             Coprocessor Interface (e.g., FPU)          │
       └────────────────────────────────────────────────────────┘

       ┌────────────────────────────────────────────────────────┐
       │              System Control Block / Debug              │
       └────────────────────────────────────────────────────────┘
```
* Load/Store architecture – memory access only through load/store.
* Pipelined execution – 3-stage to 8-stage pipelines.
* Modes: Multiple execution modes for security and interrupt handling.
* Instruction sets: Supports ARM, THUMB, and Thumb-2.
* ISA versions: ARMv7 (32-bit), ARMv8 (adds 64-bit), ARMv9 (security + AI)

  ### Interview Q: What are modes in ARM?

## 6. ARM Processor Modes
```
                   ┌─────────────────────────────┐
                   │     ARM Processor Modes     │
                   └─────────────────────────────┘
                                 ▲
                                 │
                   ┌─────────────────────────┐
                   │     Two Main Levels     │
                   └─────────────────────────┘
                          ▲             ▲
                          │             │
              ┌───────────────┐   ┌───────────────┐
              │  User Mode    │   │ Privileged    │
              │ (Unprivileged)│   │    Modes      │
              └───────────────┘   └───────────────┘
                                        ▲
         ┌──────────────┬───────────────┬──────────────┬──────────────┬──────────────┬──────────────┐
         │ FIQ Mode     │ IRQ Mode      │ Supervisor   │ Abort Mode   │ Undefined    │ System Mode  │
         │ (Fast Int.)  │ (IRQ Handler) │ (OS Kernel)  │ (Mem aborts) │ (Undef Inst.)│ (Privileged) │
         └──────────────┴───────────────┴──────────────┴──────────────┴──────────────┴──────────────┘

```
## 1. User Mode (usr)
* Privilege Level: Unprivileged
* Purpose: Normal application (user-level) code runs here
* Access: Cannot access privileged system features or control registers
* Registers: Uses general-purpose registers (R0–R15), no banked registers
* Entry: Default mode after boot or when returning from system calls

Key Point: Most user programs run in this mode — no access to hardware or critical resources.

## 2. FIQ Mode (fiq) – Fast Interrupt Request
* Privilege Level: Privileged
* Purpose: Handles high-priority, time-sensitive interrupts
* Registers: Has banked registers R8–R14 and SPSR\_fiq
* Entry: Entered automatically when FIQ interrupt is triggered

Key Point: Specially optimized for # Documents.
## 3. IRQ Mode (irq) – Interrupt Request
* Privilege Level: Privileged
* Purpose: Handles normal hardware interrupts (slower than FIQ)
* Registers: Banked R13 (SP), R14 (LR), and SPSR\_irq
* Entry: Entered automatically when IRQ interrupt is triggered

Key Point: Used for most peripheral interrupt handling (timers, UART, etc.).

## 4. Supervisor Mode (svc)
* Privilege Level: Privileged
* Purpose: Handles OS-level operations and system calls
* Registers: Banked SP (R13), LR (R14), and SPSR\_svc
* Entry: Triggered by the SVC (software interrupt) instruction

Key Point: The OS kernel usually runs in this mode.

## 5. Abort Mode (abt)
* Privilege Level: Privileged
* Purpose: Handles memory access violations (e.g., accessing bad memory)
* Registers: Banked SP (R13), LR (R14), and SPSR\_abt
* Entry: Entered automatically when a prefetch or data abort occurs

Key Point: Used for memory fault handling like segmentation faults or page faults.

## 6. Undefined Mode (und)
* Privilege Level: Privileged
* Purpose: Handles illegal or unsupported instructions
* Registers: Banked SP (R13), LR (R14), and SPSR\_und
* Entry: Entered automatically when an undefined instruction is executed

Key Point: Useful for emulation or capturing software bugs.

## 7. System Mode (sys)
* Privilege Level: Privileged
* Purpose: Runs privileged OS code, but without triggering exceptions
* Registers: Shares user-mode registers (no banking), full system access
* Entry: Software-controlled (e.g., switch from SVC using CPSR)

Key Point: Allows the OS to run user-mode code with full access but no mode switch.

## 8. Monitor Mode (mon) (TrustZone only)
* Privilege Level: Privileged
* Purpose: Handles secure world operations in ARM TrustZone
* Registers: Own banked SP, LR, and SPSR\_mon
* Entry: On secure monitor calls (SMC) or secure exceptions

Key Point: Part of ARM's security extension to separate secure vs. non-secure execution.

## 8. Registers & Banked Registers


* 16 General Purpose Registers (R0–R15):
  * R13 = Stack Pointer (SP)
  * R14 = Link Register (LR)
  * R15 = Program Counter (PC)
* CPSR/SPSR: Current/Saved Program Status Registers (flags, mode, interrupts)
* Banked Registers: FIQ, IRQ, SVC, etc., have private SP/LR/SPSR
  * Enables fast, efficient interrupt handling without saving all registers.

