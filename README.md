Z80.js
======
This is an emulator for the Z80 processor, written in JavaScript and TypeScript. It is developed to serve as a component of an emulator for a larger system which incorporates a Z80 as its CPU.

> [!NOTE]
> Since the original project was archived by Molly Howell, it is now continued by Antonino Porcino ([@nippur72](https://github.com/nippur72) on GitHub). This version has been converted to ES Modules and TypeScript.

Installation
------------
You can install this package directly from GitHub via npm:

```bash
npm install github:nippur72/Z80.js
```

Usage
-----
Using the emulator is fairly simple: call the `Z80` constructor function, passing it a `core` object. This object represents the system's memory and I/O bus, and must implement the `Z80Core` interface containing the following functions:

- `mem_read(address: number): number` - Returns the byte at the specified memory address.
- `mem_write(address: number, value: number): void` - Writes the specified byte to the specified memory address.
- `io_read(port: number): number` - Returns a byte read from the given I/O port.
- `io_write(port: number, value: number): void` - Writes the specified byte to the specified I/O port.

### TypeScript / ES Modules Example

```typescript
import { Z80, Z80Core } from 'z80-js';

// Define the emulator core
const core: Z80Core = {
   mem_read(address) {
      return 0; // your memory read implementation
   },
   mem_write(address, value) {
      // your memory write implementation
   },
   io_read(port) {
      return 0; // your I/O read implementation
   },
   io_write(port, value) {
      // your I/O write implementation
   }
};

// Instantiate the Z80 emulator
const z80 = new Z80(core);

// Reset the CPU
z80.reset();

// Run a single instruction (returns the number of T-cycles taken)
const cycles = z80.run_instruction();
```

API Reference
-------------
The `Z80` constructor returns a `Z80Instance` object containing the following public API:

- `reset()` - Resets the processor.
- `run_instruction(): number` - Runs the instruction the program counter is currently pointing at, advances the program counter, and returns the number of T cycles taken (including any cycles spent handling interrupts that were triggered).
- `interrupt(non_maskable: boolean, data?: number)` - Triggers an interrupt. `non_maskable` should be true if it is an NMI. `data` represents the value placed on the data bus if appropriate (e.g. for IM 0 or IM 2).
- `getState(): Z80State` - Returns an object representing the internal state of the Z80 (all registers, flags, halted state, etc.).
- `setState(state: Z80State)` - Replaces the internal state of the Z80 with the given state object.

Known Issues
============
The undocumented flags, sometimes called the X and Y flags, or the 3 and 5 flags, will most likely take on incorrect values as the result of a BIT n, (HL) instruction.

Memory refresh is not emulated. The R register exists and should maintain the correct value, but it isn't used in any way aside from the LD A, R instruction.

License
=======
This code is copyright Molly Howell and contributors and is made available under the MIT license. The text of the MIT license can be found in the LICENSE.md file in this repository.
