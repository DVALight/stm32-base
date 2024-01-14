cmake_minimum_required(VERSION 3.16)

# STM32_TOOLCHAIN_PATH, STM32_TARGET_TRIPLET, STM32_CUBE_F4_PATH must be present in environment variables
# check https://github.com/DVALight/stm32-cmake for more info

project(stm32-base C ASM)
set(SOURCES
    src/core/syscalls.c
    src/core/sysmem.c
    src/hal_isr.c
    src/hal_msp.c
    src/main.c
)

set(HEADERS
    src/stm32f4xx_hal_conf.h
    src/hal_isr.h
)


# change to your MCU family and model!
if(NOT MCU_FAMILY)
    set(MCU_FAMILY              "F4"            CACHE INTERNAL "STM32 MCU family")
    set(MCU_MODEL               "F401VE"        CACHE INTERNAL "STM32 MCU model")
endif()

#set(HAL_COMP_LIST RCC GPIO CORTEX)
list(APPEND HAL_COMP_LIST "STM32${MCU_FAMILY}")
list(APPEND CMSIS_COMP_LIST "STM32${MCU_FAMILY}")

if(NOT STM32_CMAKE_PACKAGES)
    find_package(CMSIS COMPONENTS "${CMSIS_COMP_LIST}" REQUIRED)
    find_package(HAL COMPONENTS "${HAL_COMP_LIST}" REQUIRED)
    set(STM32_CMAKE_PACKAGES 1)
endif()

add_executable(stm32-base ${SOURCES} ${HEADERS})
target_include_directories(stm32-base PRIVATE # change to PUBLIC if you're making library
    src
)
target_compile_definitions(stm32-base PRIVATE
    #HAL_RCC_MODULE_ENABLED
    #HAL_FLASH_MODULE_ENABLED
    #HAL_CORTEX_MODULE_ENABLED
)

target_link_libraries(stm32-base
#   HAL::STM32::${MCU_FAMILY}::RCC
#   HAL::STM32::${MCU_FAMILY}::GPIO
   HAL::STM32::${MCU_FAMILY}::CORTEX
   CMSIS::STM32::${MCU_MODEL}
   STM32::NoSys
)

add_custom_command(TARGET stm32-base POST_BUILD
    COMMAND ${CMAKE_OBJCOPY} -O ihex stm32-base.elf stm32-base.hex
)

stm32_print_size_of_target(stm32-base)