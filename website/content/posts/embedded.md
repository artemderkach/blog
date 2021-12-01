+++
author = "Artem Derkach"
title = "Embedded Development"
date = "2021-11-30"
description = "Notes for setting embedded development workflow"
tags = [
    "embedded",
]
draft = true
+++

Notes for setting embedded development workflow. 
<!--more-->

Notes below describing flow for `STM32F446RE` which is `STM32F` family. 
Nevertheless, this notes can be used for other MCUs.
<br><br>


## Content
software components
- [Linker Script]({{< ref "#linker-script" >}})
- startup file
- [Libraries]({{< ref "#libraries" >}})
  
software development components
- [Documentation]({{< ref "#documentation" >}})
- software development environments
- build tools
- [Build Process]({{< ref "#build-process" >}})
- flash
- debug
- microcontroller documentation
<br><br>

## Linker Script
[article about linker script](https://blog.thea.codes/the-most-thoroughly-commented-linker-script/)
Linker script combines `.o` into single `.elf`.
<br><br>


## Libraries
Libraries or Packs is a collection of software to help you with development:
- CMSIS modules
- Hardware Access Layer (HAL) + Low Level (LL)
- Drivers
- Examples

Packs for `STM32F`:
- [STM32CubeF4](https://github.com/STMicroelectronics/STM32CubeF4)
- [Keil Packs](https://www.keil.com/dd2/pack/) includes all available packs and can be used for different controllers

## Documentation
Documentation for controller can be gathered from [datasheet](https://www.st.com/en/microcontrollers-microprocessors/stm32f446re.html) (`STM32F446RE`)
<br><br>


## Build Process
As build tool `arm-none-eabi-gcc` is used, it is a `gcc` variation for `ARM`.
[gcc docs](https://gcc.gnu.org/onlinedocs/gcc-11.2.0/gcc/)

Build process consists of 4 steps:
- preprocess (`-E` to stop after this stage). 
- assemble (`-S` to stop after this stage)
- compile (`-c` to stop after this stage)
- link
### Compiler Flags
### Machine-Dependent Options
Option starts with `-m`

[Available arm options](https://gcc.gnu.org/onlinedocs/gcc-11.2.0/gcc/ARM-Options.html)

Cortex-M options ([source](https://launchpadlibrarian.net/177524521/readme.txt)):
```
--------------------------------------------------------------------
| ARM Core | Command Line Options                       | multilib |
|----------|--------------------------------------------|----------|
|Cortex-M0+| -mthumb -mcpu=cortex-m0plus                | armv6-m  |
|Cortex-M0 | -mthumb -mcpu=cortex-m0                    |          |
|Cortex-M1 | -mthumb -mcpu=cortex-m1                    |          |
|          |--------------------------------------------|          |
|          | -mthumb -march=armv6-m                     |          |
|----------|--------------------------------------------|----------|
|Cortex-M3 | -mthumb -mcpu=cortex-m3                    | armv7-m  |
|          |--------------------------------------------|          |
|          | -mthumb -march=armv7-m                     |          |
|----------|--------------------------------------------|----------|
|Cortex-M4 | -mthumb -mcpu=cortex-m4                    | armv7e-m |
|(No FP)   |--------------------------------------------|          |
|          | -mthumb -march=armv7e-m                    |          |
|----------|--------------------------------------------|----------|
|Cortex-M4 | -mthumb -mcpu=cortex-m4 -mfloat-abi=softfp | armv7e-m |
|(Soft FP) | -mfpu=fpv4-sp-d16                          | /softfp  |
|          |--------------------------------------------|          |
|          | -mthumb -march=armv7e-m -mfloat-abi=softfp |          |
|          | -mfpu=fpv4-sp-d16                          |          |
|----------|--------------------------------------------|----------|
|Cortex-M4 | -mthumb -mcpu=cortex-m4 -mfloat-abi=hard   | armv7e-m |
|(Hard FP) | -mfpu=fpv4-sp-d16                          | /fpu     |
|          |--------------------------------------------|          |
|          | -mthumb -march=armv7e-m -mfloat-abi=hard   |          |
|          | -mfpu=fpv4-sp-d16                          |          |
|----------|--------------------------------------------|----------|
|Cortex-R4 | [-mthumb] -march=armv7-r                   | armv7-ar |
|Cortex-R5 |                                            | /thumb   |
|Cortex-R7 |                                            |          |
|(No FP)   |                                            |          |
|----------|--------------------------------------------|----------|
|Cortex-R4 | [-mthumb] -march=armv7-r -mfloat-abi=softfp| armv7-ar |
|Cortex-R5 | -mfpu=vfpv3-d16                            | /thumb   |
|Cortex-R7 |                                            | /softfp  |
|(Soft FP) |                                            |          |
|----------|--------------------------------------------|----------|
|Cortex-R4 | [-mthumb] -march=armv7-r -mfloat-abi=hard  | armv7-ar |
|Cortex-R5 | -mfpu=vfpv3-d16                            | /thumb   |
|Cortex-R7 |                                            | /fpu     |
|(Hard FP) |                                            |          |
|----------|--------------------------------------------|----------|
|Cortex-A* | [-mthumb] -march=armv7-a                   | armv7-ar |
|(No FP)   |                                            | /thumb   |
|----------|--------------------------------------------|----------|
|Cortex-A* | [-mthumb] -march=armv7-a -mfloat-abi=softfp| armv7-ar |
|(Soft FP) | -mfpu=vfpv3-d16                            | /thumb   |
|          |                                            | /softfp  |
|----------|--------------------------------------------|----------|
|Cortex-A* | [-mthumb] -march=armv7-a -mfloat-abi=hard  | armv7-ar |
|(Hard FP) | -mfpu=vfpv3-d16                            | /thumb   |
|          |                                            | /fpu     |
--------------------------------------------------------------------
```

Based on table above options for `STM32F446RE` is:
- `-mlittle-endian` (default for arm processors)
- `-mthumb` (instruction set). Can also be `-marm` or `-mthumb-interwork`
- `-mcpu=cortex-m4`
- `-march=armv7e-m`
- `-mfloat-abi=hard` (chip supports floating point instructions)
- `-mfpu=fpv4-sp-d16`

#### Architecture
`-mcpu=cortex-m4`

<br><br>


### Linker Flags (`LDFLAGS`)
`-T` - to include linker script (`-Tstm324f.ld`)
<br><br>

