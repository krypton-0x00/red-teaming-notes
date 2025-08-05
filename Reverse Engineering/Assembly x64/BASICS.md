
```markdown
# 🧠 Assembly Notes

## 🗃️ General-Purpose Registers

There are 8 main general-purpose registers:

- `RAX`: Accumulator register. Often stores the **return value** of a function.
- `RBX`: Sometimes used as a **base register** (not to be confused with the base pointer).
- `RCX`: **Counter register**, commonly used as a loop counter.
- `RDX`: Known as the **data register**.
- `RSI`: **Source index**, used as the source pointer in string operations.
- `RDI`: **Destination index**, used as the destination pointer in string operations.
- `RSP`: **Stack pointer**, holds the address of the top of the stack.
- `RBP`: **Base pointer**, holds the address of the base (bottom) of the stack.

<details>
<summary>📦 Extra Registers</summary>

- `R8`: Full 64-bit (8 bytes) register.
- `R8D`: Lower double word (4 bytes).
- `R8W`: Lower word (2 bytes).
- `R8B`: Lower byte.
</details>

---

## 🧾 Instructions Overview

### 🧩 Operand Types

- **Immediate**: Constant value (e.g., `12`) — not a memory address or register.
- **Register**: Refers to registers like `RAX`, `RBX`, `R12`, `AL`, etc.
- **Memory Address**: A location in memory (e.g., `0x7FFF842B`).

### 🧠 Instruction Format

```

(Instruction / Opcode / Mnemonic) ,

````


```asm
mov RAX, 5           ; RAX = 5
lea RAX, num1        ; RAX = address of num1
````

---

## 🧃 LEA vs MOV

### LEA (Load Effective Address)

```asm
LEA RAX, [10]        ; RAX = 10 (not a memory read)
```

Used for address computation:

```asm
RCX = 0x1000
LEA RAX, [RCX + 8]   ; RAX = 0x1008
```

### MOV (Memory Read)

```asm
MOV RAX, [10]        ; RAX = value at memory address 0xA
```

```asm
RCX = 0x1000
[0x1008] = 0x12345678
MOV RAX, [RCX + 8]   ; RAX = *(0x1008) = 0x12345678
```

> 🧠 **Tip**: Square brackets `[ ]` dereference memory addresses in `MOV`, but not in `LEA`.

---

## 🪂 Conditional Branching: The JMP's Mason

### 📌 Unsigned Comparisons

|Instruction|Condition|Flags|
|---|---|---|
|`JB` / `JNAE`|Jump if Below / Not Above or Equal|`CF = 1`|
|`JAE` / `JNB`|Jump if Above or Equal / Not Below|`CF = 0`|
|`JBE` / `JNA`|Jump if Below or Equal / Not Above|`CF = 1 or ZF = 1`|
|`JA` / `JNBE`|Jump if Above / Not Below or Equal|`CF = 0 and ZF = 0`|

### 📌 Signed Comparisons

|Instruction|Condition|Flags|
|---|---|---|
|`JL` / `JNGE`|Jump if Less / Not Greater or Equal|`SF ≠ OF`|
|`JGE` / `JNL`|Jump if Greater or Equal / Not Less|`SF = OF`|
|`JLE` / `JNG`|Jump if Less or Equal / Not Greater|`ZF = 1 or SF ≠ OF`|
|`JG` / `JNLE`|Jump if Greater / Not Less or Equal|`ZF = 0 and SF = OF`|

---

## 🔀 If-Else: Compiler Translation Example

```c
if(x == 4){
    func1();
} else {
    return;
}
```

The compiler might generate:

```c
if(x != 4){
    goto __exit;
}
func1();
__exit:
return;
```

> ⚡ **Compiler Optimization**: Compilers flip logic to reduce jumps and instructions for efficiency.

---

## 🧷 Pointers in Assembly

- Square brackets `[ ]` **dereference** memory addresses.
    

```asm
MOV RAX, [10]         ; RAX = *(0xA)
LEA RAX, [10]         ; RAX = 10 (no memory read)
```

> 🧠 `[var]` means _value at the address held by var_

```asm
let RCX = 0x1000
let Memory at 0x1008 = 0x12345678


				MOV RAX, [RCX + 8]                                        ; RAX = *(0x1000 + 8) = *(0x1008)

                                                        ; RAX = 0x12345678
```


# Flags
Flags are contained in a register called EFLAGS (x86) or RFLAGS (x64)
There is an actual FLAGS register that is 16 bit, but the semantics are just a waste of time.

## Status Flags

Here are the flags you should know. Note that when I say a "flag is set" I mean the flag is set to 1 which is true/on. 0 is false/off.

- **Zero Flag (ZF)** - Set if the result of an operation is zero. Not set if the result of an operation is _not_ zero.
- **Carry Flag (CF)** - Set if the last _unsigned_ arithmetic operation carried (addition) or borrowed (subtraction) a bit beyond the register. It's also set when an operation would be negative if it wasn't for the operation being unsigned.
- **Overflow Flag (OF)** - Set if a _signed_ arithmetic operation is too big for the register to contain.
- **Sign Flag (SF)** - Set if the result of an operation is negative.
- **Adjust/Auxiliary Flag (AF)** - Same as the carry flag but for Binary Coded Decimal (BCD) operations.
- **Parity Flag (PF)** - Set to 1 if the number of bits set in the last 8 bits is even. (10110100, PF=1; 10110101, PF=0)
- **Trap Flag (TF)** - Allows for single-stepping of programs.


EXAMPLES

```asm
mov RAX, 4
cmp RAX, 4
jne 5       ; Line 5 (ret)
call func1
ret                                             ; ZF = 1, OF = 0, SF = 0
```


```asm
mov RAX, 2
sub RAX, 8  ; 2 - 8 = -6                                         ; ZF = 0, OF = 0, SF = 1

```

```asm
mov AL, 75
add AL, 60
; ZF = 0, OF = 1, SF = 1
```
