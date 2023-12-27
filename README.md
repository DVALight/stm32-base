# stm32-test

STM32 CMake base configuration and HAL core for use in projects.

## File structure
### src/
Your main source code goes here

### src/core/
Core source code for STM32 HAL
- stm32f4xx_hal_conf.h - enable different HAL module includes, some basic config
- stm32f4xx_hal_msp.c - HAL MSP callbacks to prepare hardware for HAL modules
- stm32f4xx_hal_it.c - interrupt / exception handlers
- syscalls.c - POSIX syscalls overrides
- sysmem.c - sbrk implementation for heap allocator 