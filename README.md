# Reflective Loader for PE Files in Rust

This project provides a Rust implementation for loading and executing Portable Executable (PE) files reflectively. It leverages the Windows API to allocate memory, write to process memory, and create threads, enabling the execution of PE files without touching the disk. The main functionality is encapsulated in the `ReflectiveLoader64` function, which reads a PE file, maps its sections into memory, and resolves imports and relocations before executing its entry point.

## Features

- **PE Header Parsing**: Functions to read and parse DOS and NT headers.
- **Section Mapping**: Allocate memory and map PE sections into the target process's address space.
- **Import Resolution**: Resolve import tables and dynamically load necessary DLLs and their functions.
- **Relocation Fixups**: Apply base relocations if the PE file is not loaded at its preferred base address.
- **Thread Creation**: Execute the PE file's entry point in a new thread within the target process.
- **Memory Cleanup**: Free allocated memory after execution completes.

## Requirements

- Rust compiler and Cargo
- Windows operating system (as it uses Windows API functions)

## Dependencies

This project uses the following external crates:

- `winapi`: For interacting with the Windows API
- `widestring`: For handling wide strings required by some Windows API functions

## Usage

1. **Clone the repository**:

   ```sh
   gh repo clone mranv/peloader64
   cd peloader64
   ```

2. **Build the project**:

   ```sh
   cargo build --release
   ```

3. **Run the main program**:

   Modify the path to your PE file in the `main` function and then run:

   ```sh
   cargo run --release
   ```

## Code Overview

### Main Functions

#### `ReflectiveLoader64`

This is the core function that performs reflective loading of a PE file.

```rust
pub fn ReflectiveLoader64(phandle: *mut c_void, buffer: Vec<u8>) {
    // Function implementation
}
```

#### `FillStructureFromArray`

Fills a structure with data from an array using `WriteProcessMemory`.

```rust
pub fn FillStructureFromArray<T, U>(base: &mut T, arr: &[U]) -> usize {
    // Function implementation
}
```

#### `FillStructureFromMemory`

Fills a structure with data read from memory using `ReadProcessMemory`.

```rust
pub fn FillStructureFromMemory<T>(dest: &mut T, src: *const c_void, prochandle: *mut c_void) -> usize {
    // Function implementation
}
```

#### `GetHeadersSize`

Calculates the size of headers for a PE file.

```rust
pub fn GetHeadersSize(buffer: &Vec<u8>) -> usize {
    // Function implementation
}
```

#### `GetImageSize`

Calculates the total image size of a PE file.

```rust
pub fn GetImageSize(buffer: &Vec<u8>) -> usize {
    // Function implementation
}
```

#### `ReadStringFromMemory`

Reads a null-terminated string from memory.

```rust
pub fn ReadStringFromMemory(baseaddress: *const u8, phandle: *mut c_void) -> String {
    // Function implementation
}
```

### Structs and Enums

The code defines various structs representing PE headers and sections:

- `IMAGE_DOS_HEADER`
- `IMAGE_NT_HEADERS64`
- `IMAGE_SECTION_HEADER`
- `IMAGE_IMPORT_DESCRIPTOR`
- `IMAGE_THUNK_DATA64`
- `IMAGE_OPTIONAL_HEADER64`
- `IMAGE_DATA_DIRECTORY`
- `IMAGE_FILE_HEADER`
- `IMAGE_EXPORT_DIRECTORY`

These structures are used to parse and manipulate the PE file in memory.

### Example Usage in `main`

The `main` function demonstrates how to read a PE file from disk and execute it using the reflective loader.

```rust
fn main() {
    let mut buffer: Vec<u8> = Vec::new();
    let mut fd = File::open(r#"path\to\your\pefile.exe"#).unwrap();
    fd.read_to_end(&mut buffer);

    unsafe {
        ReflectiveLoader64(GetCurrentProcess(), buffer.clone());
    }
}
```

## Disclaimer

This code is for educational purposes only. Use it responsibly and ensure compliance with all applicable laws and regulations. Unauthorized use of this code may violate the terms of service of software and services you interact with.

## Contributing

Feel free to submit issues or pull requests if you have suggestions for improvements or find bugs.

## License

This project is licensed under the MIT License. See the `LICENSE` file for details.
