# stm32-base

STM32 CMake base configuration and HAL core for use in projects.

based on [stm32-cmake](https://github.com/ObKo/stm32-cmake)

## Setup

To use it for your MCU don't forget to change `MCU_FAMILY` and `MCU_MODEL` variables

## File structure
### src/
- stm32f4xx_hal_conf.h - HAL configuration file. Required by external libraries
- hal_msp.c - your HAL MSP callbacks to prepare hardware for HAL modules
- hal_isr.c - your interrupt / exception handlers
- main.c - your main source code goes here

### src/core/
Core source code for STM32 HAL
- syscalls.c - POSIX syscalls overrides
- sysmem.c - sbrk implementation for heap allocator