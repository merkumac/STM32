# STM32F031 Development environment setup on MacOS
In order to start having fun with the STM32F031K MCUs, one has to setup the
development environment. That I have found problematic, when I was following
the Programista magazine's Cortex M0 programming tutorial. So I tried to leave
Windows behind an go with MacOS Catalina.

## Installing the necessary toolchain


### 1. Install Apple XCode and Command Line Tools
That's all the good Unix stuff.

```
% xcode-select --install
```

Just pay attention, as you will need to accept the licenses.

### 2. Install Homebrew
That is if you're not using it yet. Great instructions are on the
[Homebrew main site](https://brew.sh/)

### 3. GNU ARM Embedded Toolchain
A cross-compiler plus the binutils like objdump, gdb etc. I chose to use the
Homebrew formula maintained by the ARM Mbed team:

```
% brew tap ArmMbed/homebrew-formulae
% brew install ArmMbed/formulae/arm-none-eabi-gcc

(...)

% arm-none-eabi-gcc --version
arm-none-eabi-gcc (GNU Tools for Arm Embedded Processors 9-2019-q4-major) 9.2.1 20191025 (release) [ARM/arm-9-branch revision 277599]
Copyright (C) 2019 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```

As I like convenience, and I am using also regular GNU GCC, I like to alias
the ARM toolchain tools to have an `a` prefix to the regular tool name. So for
the compiler, instead of calling `arm-none-eabi-gcc`, I'm just calling `agcc`.
It is not at all necessary, but if you'd like that, edit your shell rc dotfile,
and add the following lines:

```
alias agcc='arm-none-eabi-gcc'
alias aas='arm-none-eabi-as'
alias ald='arm-none-eabi-ld'
alias agdb='arm-none-eabi-gdb'
alias aobjdump='arm-none-eabi-objdump'
```

### 4. Install OpenOCD
```
brew install open-ocd

(...)

% openocd --version
Open On-Chip Debugger 0.10.0
Licensed under GNU GPL v2
For bug reports, read
	http://openocd.org/doc/doxygen/bugs.html
```

OpenOCD requires the configuration (`*.cfg`) files in order to work properly.
After installation, the configuration files will be in the following folders:

```
% ls /usr/local/Cellar/open-ocd/0.10.0/share/openocd/scripts/board
% ls /usr/local/Cellar/open-ocd/0.10.0/share/openocd/scripts/interface
% ls /usr/local/Cellar/open-ocd/0.10.0/share/openocd/scripts/target
```

With the development board connected, and having proper USB cable (The one I
got with my STM32F031 board is simply not enough to work with my MBP, but
switching to a random USB B to USB mini cable, seems fix everything), you
should see similar output of openOCD startup:

```
% openocd -f /usr/local/Cellar/open-ocd/0.10.0/share/openocd/scripts/interface/stlink-v2-1.cfg -f /usr/local/Cellar/open-ocd/0.10.0/share/openocd/scripts/target/stm32f0x.cfg
Open On-Chip Debugger 0.10.0
Licensed under GNU GPL v2
For bug reports, read
	http://openocd.org/doc/doxygen/bugs.html
Info : auto-selecting first available session transport "hla_swd". To override use 'transport select <transport>'.
Info : The selected transport took over low-level target control. The results might differ compared to plain JTAG/SWD
adapter speed: 1000 kHz
adapter_nsrst_delay: 100
none separate
Info : Unable to match requested speed 1000 kHz, using 950 kHz
Info : Unable to match requested speed 1000 kHz, using 950 kHz
Info : clock speed 950 kHz
Info : STLINK v2 JTAG v25 API v2 SWIM v14 VID 0x0483 PID 0x374B
Info : using stlink api v2
Info : Target voltage: 3.255336
Info : stm32f0x.cpu: hardware has 4 breakpoints, 2 watchpoints
```

# TODO:
- Add info about the st-link and usage
