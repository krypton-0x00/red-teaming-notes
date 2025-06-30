
### 🔧 **Function Names in Binaries**

- When code is **compiled**, most **function names are removed** or obfuscated. This is because names are for humans (not machines), and compilers don’t need them.
    
- However, some names **survive** in the binary. This usually happens in two cases:
    

---

### 📌 **1. Recognized Functions (Using FLIRT Signatures)**

- Tools like **IDA Pro** can **detect known library functions** using **FLIRT (Fast Library Identification and Recognition Technology)**.
    
- FLIRT works by comparing **byte patterns** of known functions (from standard libraries) to code in the binary.
    
- If it finds a match, it **labels the function properly**, e.g., `printf`, `malloc`, etc.
    
- `main()` is hard to detect this way because it's different for every program — but IDA uses some **heuristics** to find it (like checking what's called near the entry point or return value checks).
    

---

### 📌 **2. Names Preserved Due to DLL Imports/Exports**

- When a program **imports a function from a DLL**, the name must remain so the program can **find and call it**.
    
- This happens with Windows API functions (e.g., `CreateFile`, `ReadFile`) and others.
    

---

### 📂 **Imports, Exports, and Modules**

- **Import**: A function your program **uses** from a DLL.
    
- **Export**: A function a DLL **provides** so other programs can use it.
    
- **DLLs** (Dynamic-Link Libraries):
    
    - Shared code that multiple programs can load **without duplicating** it in memory.
        
    - Example: `kernel32.dll`, `ntdll.dll`.
        
- **Modules**: Anything loaded into a process (like EXEs or DLLs). When you run `Loop.exe`, both the `.exe` and any DLLs it uses are considered **modules** of that process.
    

---

### 🧩 **Name Mangling / Decoration**

- In **C++**, function **overloading** means you can have multiple functions with the same name but different parameters.
    
- That’s a problem when **exporting** functions from DLLs (because they need unique names).
    
- The solution: **Name mangling**, where the compiler **encodes function names with parameter info**.
    
    Example:
    
    ```
    void Add(int a, int b);
    // might get mangled to: ?Add@@YAXHH@Z
    ```
    
- This makes function names look like garbage, but they **contain metadata** about return types, namespaces, etc.
    
- Tools can **demangle** these names to help reverse engineers understand them.
    

> **`extern "C"`**: Use this to **disable name mangling** for a function so it's exported with a clean C-style name (but this disables overloading).

---

### 🧪 **Running and Analyzing DLLs**

- **DLLs can’t be run directly**. They must be loaded into a process.
    
- When a DLL is loaded, its special function `DllMain()` is automatically executed.
    
- **DLL injection**: A reverse engineering technique where a DLL is loaded into another process (e.g., via `LoadLibrary()`).
    

---

### 🧰 **Tools and Techniques**

- Windows provides a tool: `rundll32.exe`
    
    - It can be used to run functions from DLLs, but it’s rarely used in practice.
        
- **Better way for reverse engineers**: Write your own small C++ program that:
    
    1. Loads the DLL with `LoadLibraryA()`
        
    2. Uses `GetProcAddress()` to get the function
        
    3. Calls the function
        
    
    Example:
    
    ```cpp
    HMODULE dll = LoadLibraryA("DLL.DLL");
    typedef void(WINAPI* Add_TypeDef)(int, int);
    Add_TypeDef Add = (Add_TypeDef)GetProcAddress(dll, "Add_MangledName");
    Add(1, 2);
    ```
    

> You might need to reverse engineer the DLL first to figure out the **correct function names** and **parameter types**.
e scenes.
```
