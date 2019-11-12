# GNU RISC-V toolchain for macOS

<div>
<img src="https://raw.githubusercontent.com/metalcode-eu/darwin-riscv/master/images/RISCV.png" alt="RISC-V" width="20%">
<img src="https://raw.githubusercontent.com/metalcode-eu/darwin-riscv/master/images/GNU.png" alt="GNU" width="20%">
<img src="https://raw.githubusercontent.com/metalcode-eu/darwin-riscv/master/images/macOS.png" alt="macOS" width="20%">
</div>

This repository is the original macOS version of the GNU Compiler for RISCV 
packaged for Visual Studio Code: 
[RISC-V Toolchain ](https://github.com/riscv/riscv-gnu-toolchain)

# Kendryte OpenOCD
The Open On-Chip Debugger version for the Kendryte K210 is included.

## Prerequisites
The RISCV compiler depends on the following libraries. 

| library | description                                                       |
|---------|-------------------------------------------------------------------|
| isl     | Integer Set Library for the polyhedral model                      |
| libmpc  | library for the arithmetic of high precision complex numbers      |

<img src="https://raw.githubusercontent.com/metalcode-eu/darwin-riscv/master/images/Homebrew.png" alt="Homebrew" width="10%" style="float: right;">

The Open On-Chip Debugger uses a USB debug probe. You need the USB-library 
installed on your Mac to use the debug probe. Use the Homebrew package manager 
to install the 'libusb' library.

See [Homebrew](https://brew.sh) for more information. 

> /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"  
> brew install libusb

# Install
In Visual Studio Code goto extensions (shift+cmd+x), search for '*metalcode-eu*'
and install the extension that is suited for your operating system. 

The extension adds a path to the Kendryte K210 openocd version. You can use in 
tasks.json.

- riscv.bin
- riscv.include
- riscv.lib
- kendryte.openocd

## Example tasks.json
```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "build path",
      "type": "shell",
      "command": "make path",
      "options": {
        "env": {
          "BIN": "${config:riscv.bin}",
          "INCLUDE": "${config:riscv.include}",
          "LIB": "${config:riscv.lib}",
          "GDB": "${config:riscv.gdb}",
          "OPENOCD": "${config:kendryte.openocd}",
        }
      },
      "group": {
        "kind": "build",
        "isDefault": true,
      },
      "problemMatcher": "$gcc"
    }
  ]
}
```
## Example makefile 
```make
.PHONY: path

path:
	@echo $(BIN)
	@echo $(INCLUDE)
	@echo $(LIB)
	@echo $(GDB)
	@echo $(OPENOCD)

	@echo
	@$(BIN)/riscv64-unknown-elf-gcc --version 
	@echo
	@$(GDB) --version 
	@echo
	@$(OPENOCD) --version 
```

# Version 1.0.2
This repository uses the following software versions:
- riscv64-unknown-elf-gcc (GCC) 9.2.0
- GNU gdb (GDB) 8.3.0.20190516-git
- Kendryte Open On-Chip Debugger For RISC-V v0.2.2 (2019-01-17)
