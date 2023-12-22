cmake_minimum_required(VERSION 3.16)

set(CMAKE_TOOLCHAIN_FILE ${CMAKE_CURRENT_SOURCE_DIR}/../stm32-cmake/cmake/stm32_gcc.cmake)
set(STM32_CUBE_F4_PATH $ENV{STM32_CUBE_F4_PATH})
set(STM32_TARGET_TRIPLET arm-none-eabi)

set(CMAKE_TRY_COMPILE_TARGET_TYPE "STATIC_LIBRARY")

project(stm32-test C ASM)
set(SOURCES
    main.c
)

set(HEADERS
    stm32f4xx_hal_conf.h
)

set(CMAKE_INCLUDE_CURRENT_DIR TRUE)

set(HAL_COMP_LIST RCC GPIO CORTEX)
set(CMSIS_COMP_LIST "")

list(APPEND CMSIS_COMP_LIST STM32F4)
list(APPEND HAL_COMP_LIST STM32F4)

find_package(CMSIS COMPONENTS "${CMSIS_COMP_LIST}" REQUIRED)
find_package(HAL COMPONENTS "${HAL_COMP_LIST}" REQUIRED)

add_executable(stm32-test ${SOURCES} ${HEADERS})
target_link_libraries(stm32-test
    HAL::STM32::F4::RCC
    HAL::STM32::F4::GPIO
    HAL::STM32::F4::CORTEX
    CMSIS::STM32::F407VG
    STM32::NoSys
)
stm32_print_size_of_target(stm32-test)