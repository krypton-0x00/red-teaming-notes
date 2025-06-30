 **Prologues** and **epilogues** are patterns in assembly functions that set up and clean up the function’s **stack frame**. They’re _not_ explicit instructions, but rather common sequences the compiler inserts at the **beginning** and **end** of a function.

Think of them like this:

> 🧱 **Prologue**: "Let's get the function ready to run."  
> 🧹 **Epilogue**: "Let’s clean up before leaving."

---

## 🔹 What Is a Stack Frame?

A **stack frame** is a block of memory on the stack that a function uses to:

- Store local variables
    
- Save return addresses
    
- Save registers (if needed)
    
- Access parameters (especially if passed on the stack)
    

To manage all that, the function needs to set things up — that’s where **prologues** and **epilogues** come in.

---

## 🔸 Function Prologue (at the start of a function)

The **prologue** usually:

1. Saves the base pointer (RBP)
    
2. Sets a new base pointer for the function
    
3. Allocates space for local variables
    

### 📦 Example Prologue

```asm
push rbp          ; Save caller’s base pointer
mov rbp, rsp      ; Set new base pointer for current function
sub rsp, 0x20     ; Reserve 32 bytes for locals (must be aligned)
```

> 💡 This is like saying: “I saved the previous workspace, set up my own desk, and now I have room to work.”

---

## 🔸 Function Epilogue (at the end of a function)

The **epilogue** does the reverse:

1. Cleans up local variables
    
2. Restores the old base pointer
    
3. Returns to the caller
    

### 🧹 Example Epilogue

```asm
mov rsp, rbp      ; Reset stack pointer
pop rbp           ; Restore caller’s base pointer
ret               ; Return to caller
```

Or, more compactly (often seen in optimized builds):

```asm
leave             ; Does `mov rsp, rbp` + `pop rbp`
ret
```

---

## 🧠 Visual Example

Suppose you have this C++ function:

```cpp
int add(int a, int b) {
    int result = a + b;
    return result;
}
```

### Possible x64 Assembly:

```asm
add:
    push rbp
    mov rbp, rsp
    sub rsp, 0x10         ; Reserve space for local variable

    mov DWORD PTR [rbp-4], ecx   ; 'a' in ECX → saved to stack
    mov DWORD PTR [rbp-8], edx   ; 'b' in EDX → saved to stack

    mov eax, DWORD PTR [rbp-4]
    add eax, DWORD PTR [rbp-8]
    mov DWORD PTR [rbp-0xC], eax ; store result

    mov eax, DWORD PTR [rbp-0xC] ; load result into return register
    leave
    ret
```

---

## 🔍 Prologue/Epilogue Breakdown

### Prologue

```asm
push rbp
mov rbp, rsp
sub rsp, 0x10
```

### Epilogue

```asm
leave
ret
```

---

## ✅ Why Do We Need Them?

- To **preserve the caller's context** (stack & base pointers)
    
- To keep functions **isolated and safe** — each one has its own stack frame
    
- To **align the stack properly** (important for calling functions & performance)
    
- To restore **non-volatile registers** (like `RBX`, `RBP`, etc.)
    

---

## 🧩 Compiler Optimization Notes

- In some simple functions, **prologues/epilogues may be omitted** if no locals or saved registers are needed.
    
- They may be **shorter or inlined** using `leave` or direct `ret`.

#### Inlining


One compiler/linker optimization that makes noticeable changes to the code is inlining. When a function gets inlined it's essentially pasted where the call would be instead of making a call. See the following example.

**Without** Function Inlining:

```cpp
int Add(int x, int y){
    return x+y;
}
int main(){
    int x = RandInt();
    int res = Add(x, 5)
}
```

**With** Function Inlining:

```cpp
int main(){
    int x = RandInt();
    int res = x + 5;
}
```