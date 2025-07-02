
## 🧠 What Is a Calling Convention?

A **calling convention** is just an agreement between the **caller** (the function that calls another) and the **callee** (the function that is called) on **how to pass parameters**, **return values**, and **who is responsible for cleaning up**.

Think of it as rules of communication so everyone knows:

- Where to put arguments.
    
- Where the return value will be.
    
- What registers/memory will change (or not).
    
- How the stack behaves.
    

---

## 🧱 Windows x64 Calling Convention (aka “fastcall”)

### ✅ Summary of the Rules (We'll explain each)

1. **First 4 parameters (non-floating)** go in **RCX, RDX, R8, R9** — left to right.
    
2. **Floating-point parameters** go in **XMM0–XMM3** — same order.
    
3. If more than 4 parameters: extras are pushed **on the stack**, right to left.
    
4. Caller must **reserve 32 bytes** (4 slots of 8 bytes) for these register arguments **on the stack**.
    
5. Return values go in:
    
    - `RAX` for integers/pointers/etc.
        
    - `XMM0` for floats/doubles.
        
6. Some registers are **volatile (destroyed)** across calls, some are **non-volatile (preserved)**.
    
7. The **caller cleans the stack**, and **allocates shadow space**.
    
8. `this` pointer in C++ is passed in `RCX`.
    

---

### 📍 Let’s Break Down Each Part with Examples

---

### 🔹 1. First Four Parameters

**Non-floating values:**

```asm
function(int a, int b, int c, int d)

MOV RCX, a ; 1st
MOV RDX, b ; 2nd
MOV R8,  c ; 3rd
MOV R9,  d ; 4th
CALL function
```

**Floating-point values:**

```c
function(float a, float b, float c, float d)

MOVSS XMM0, a ; float in XMM0
MOVSS XMM1, b
MOVSS XMM2, c
MOVSS XMM3, d
CALL function
```

**Mixed example:**

```c
function(int, double, int, float)

RCX  -> int
XMM1 -> double
R8   -> int
XMM3 -> float
```

> 🧠 Important: Even if a floating-point value is second in the list, it uses the second _floating-point register_ (`XMM1`), not the second GPR (general-purpose register).

---

### 🔹 2. More Than 4 Parameters

If there are more than 4 parameters, the extras go on the stack — but in **reverse order** (right to left):

```asm
function(1, 2, 3, 4, 5, 6, 7, 8)

; Registers for the first 4:
MOV RCX, 1
MOV RDX, 2
MOV R8,  3
MOV R9,  4

; Stack for the rest (push right to left):
PUSH 8
PUSH 7
PUSH 6
PUSH 5

CALL function
```

---

### 🔹 3. Shadow Space (The "Scratch Space")

Even if the function takes **no parameters**, the caller **must** reserve 32 bytes on the stack (4 slots of 8 bytes) _before the call_.

This is called **shadow space**, and the callee can use it if it wants.

```asm
SUB RSP, 0x28      ; 32 bytes shadow space + alignment
MOV RCX, 1         ; param 1
CALL my_function
ADD RSP, 0x28
```

Why? Because the callee might expect to write to those 4 slots — it might store `RCX`, `RDX`, `R8`, and `R9` there.

---

### 🔹 4. Return Value

- **Integer, pointer, bool** → returned in `RAX`
    
- **Float/double** → returned in `XMM0`
    

```asm
; function returns int
CALL func
; result in RAX
```

```asm
; function returns float
CALL func
; result in XMM0
```

---

### 🔹 5. Volatile vs Non-Volatile Registers

|Volatile (destroyed by callee)|Non-Volatile (must be saved if used)|
|---|---|
|RAX, RCX, RDX, R8–R11|RBX, RBP, RDI, RSI, R12–R15|
|XMM0–XMM5|XMM6–XMM15|

If you want to use non-volatile registers, you must **save and restore** them in the callee.

```asm
PUSH RBX      ; save
...           ; use RBX
POP RBX       ; restore
```

---

### 🔹 6. Accessing Parameters on the Stack

If you have more than 4 parameters, or want to access the **shadow space**, it looks like this:

```asm
; Let's say you're in the callee function
; Stack layout:
; RSP + 0x00 → return address
; RSP + 0x08 → shadow slot 1 (RCX)
; RSP + 0x10 → shadow slot 2 (RDX)
; RSP + 0x18 → shadow slot 3 (R8)
; RSP + 0x20 → shadow slot 4 (R9)
; RSP + 0x28 → 5th parameter (on stack)
; RSP + 0x30 → 6th parameter
```

---

### 🧠 C++ Example + Translation

```cpp
int add(int a, int b) {
    return a + b;
}
```

### Compiled Assembly:

```asm
add:
    ; 'a' in RCX, 'b' in RDX
    MOV EAX, ECX
    ADD EAX, EDX
    RET
```

---

### 🛠 Putting It All Together

```asm
; function(1, 2.0, 3, 4.0, 5)
; Mixed parameters

MOV RCX, 1        ; int → RCX
MOVSD XMM1, 2.0   ; double → XMM1
MOV R8,  3        ; int → R8
MOVSS XMM3, 4.0   ; float → XMM3
PUSH 5            ; int on stack → 5th parameter
CALL function
```

---

## 🔚 Recap (Cheat Sheet)

- `RCX, RDX, R8, R9` → first 4 non-FP arguments
    
- `XMM0–XMM3` → first 4 FP arguments
    
- Extra args → pushed right to left on stack
    
- Shadow space = 32 bytes always allocated
    
- Return: `RAX` or `XMM0`
    
- Volatile registers may be destroyed by call
    
