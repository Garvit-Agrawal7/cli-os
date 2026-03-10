# CLI OS
### In development
A minimal, CLI-based operating system built from scratch in C and x86 Assembly, targeting the x86 architecture.

---

## Overview

CLI OS is a bare-metal operating system that boots directly on x86 hardware or inside an emulator. It implements all core OS components from the bootloader to a functional interactive shell — without relying on any existing OS or standard library.

---

## Architecture

- **Target CPU:** x86 (32-bit protected mode)
- **Primary Language:** C
- **Assembly:** NASM (Netwide Assembler)
- **Bootloader:** GRUB (Multiboot specification)
- **Emulator:** QEMU (Quick Emulator)
- **Cross-compiler:** `i686-elf-gcc`

---

## Components

### Boot
- GRUB-based bootloader via Multiboot header
- Assembly kernel entry point — sets up stack, calls `kernel_main()`

### CPU Initialization
- **GDT** — Global Descriptor Table defining kernel/user memory segments
- **IDT** — Interrupt Descriptor Table mapping exceptions and hardware IRQs
- **PIC** — Programmable Interrupt Controller remapping IRQ vectors

### Drivers
- **VGA Text Mode** — Memory-mapped output at `0xB8000`, supports `kprintf()`
- **PIT** — Programmable Interval Timer (IRQ0), drives system tick and scheduling basis
- **Keyboard** — Scan code reader on IRQ1 with ASCII translation

### Memory Management
- **Physical Memory Manager** — Frame allocator using a bitmap over the GRUB memory map
- **Paging** — Page directory/table setup, CR0 paging enabled, kernel mapped to higher half (`0xC0000000`)
- **Heap Allocator** — `kmalloc()` / `kfree()` via free-list allocator with coalescing

### Shell
- Input loop driven by keyboard driver
- Tokenizes and dispatches commands to built-in handlers
- Built-in commands: `help`, `echo`, `clear`, `halt`

---

## License

MIT License. See [LICENSE](LICENSE) for details.
