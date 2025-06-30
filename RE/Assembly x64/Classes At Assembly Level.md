

````markdown
# 🧠 Classes in Assembly (Crash Course)

## 📦 What Are Classes?
- A **class** can store **multiple variables** of **different types**.
- This makes them more complex to reverse engineer compared to structs or flat data.
- Understanding a class requires:
  - Analyzing how it's used in functions.
  - Determining **how many members** exist and **what types** they are.

---

## 🏗️ Example: Class Structure

```cpp
class Human {
public:
	int age;
	float height;
	char* name;
	Human(char* newName, int newAge, float newHeight)
		: age(newAge), height(newHeight), name(newName) {}
};
````

### 🧮 Memory Layout (on x64)

- `int age` → 4 bytes
    
- `float height` → 4 bytes
    
- `char* name` → 8 bytes (pointer = 8 bytes on x64)
    
- **Total = 16 bytes**
    

---

## 🪛 Accessing Fields via Assembly

Let’s say a pointer to the object is stored in `RAX`, and the object lives at `0x4000`.

```asm
mov RAX, 0x4000     ; RAX = base address (start of object)
lea RBX, [RAX+0x4]  ; RBX = address of height (offset 4)
lea RCX, [RAX+0x8]  ; RCX = address of name (offset 8)

mov [RAX], 0x32     ; Store 50 (age)
mov [RBX], 0x48     ; Store 72 (height)
mov [RCX], 0x424F42 ; Store address or value for "BOB" (name)
```

---

## 🧩 What’s Happening Here?

### 🧭 Step 1: Address Calculation (LEA)

- `LEA` is used to compute addresses of fields inside the class.
    
- This gives us pointers to each member without dereferencing yet.
    

### 🧮 Step 2: Store Values in Memory

- `MOV [reg], val` stores actual data **into memory** at the calculated address.
    
- We're **not** copying the fields — we're **writing to them** directly.
    

---

## 🧠 Key Concepts to Remember

- Classes = memory layout of mixed data types.
    
- Fields are accessed by offsets from the base pointer (`RAX` in this case).
    
- `LEA` is for address math, not memory access.
    
- Be cautious: Some class fields may not always appear (e.g., rarely used headers).
    
