+++
author = "Artem Derkach"
title = "Embedded Development"
date = "2021-11-07"
description = "Notes for setting embedded development workflow"
tags = [
    "embedded",
]
+++

Notes for setting embedded development workflow. 
<!--more-->

Notes below describing flow for `STM32F446RE` which is `STM32F` family. 
Nevertheless, this notes can be used for other MCUs.

## Content
software components
- linker script
- startup file
- [libraries]({{< ref "#libraries" >}})
  
software development components
- software development environments
- build tools
- compilation
- flash
- debug
- microcontroller documentation

## Libraries
Libraries or Packs is a collection of software to help you with development:
- CMSIS modules
- Hardware Access Layer (HAL) + Low Level (LL)
- Drivers
- Examples

Packs for `STM32F`:
- [STM32CubeF4](https://github.com/STMicroelectronics/STM32CubeF4)
- [Keil Packs](https://www.keil.com/dd2/pack/) includes all available packs and can be used for different controllers