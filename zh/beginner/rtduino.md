# RTduino

## 1 简介

RTduino 是[RT-Thread实时操作系统](https://www.rt-thread.org)的 Arduino 生态兼容层，为 [RT-Thread 社区](https://github.com/RT-Thread/rt-thread)的子社区，旨在兼容 Arduino 社区生态来丰富 RT-Thread 社区软件包生态（如上千种分门别类的 Arduino 库，以及 Arduino 社区优秀的开源项目），并降低 RT-Thread 操作系统以及与 RT-Thread 适配的芯片的学习门槛。通过 RTduino，可以让用户使用 Arduino 的函数、编程方法，轻松地将 RT-Thread 和 BSP 使用起来。用户也可以直接使用 [Arduino 社区第三方库](https://www.arduino.cc/reference/en/libraries/)（例如传感器驱动库、算法库等）直接用在 RT-Thread 工程中，极大地补充了 RT-Thread 社区生态。

![framework](./figures/rtduino-framework.png)

## 2 已经适配RTduino的RT-Thread BSP

### 2.1 RTduino官方建议入门BSP

以下 BSP 由 RTduino/RT-Thread 官方支持，其功能得到官方验证，并配有详细入门资料，不会踩坑，建议初学者选择。**请在选择所需的对应BSP芯片厂商。**

<!-- tabs:start -->

## ** STM32 **

- 编译环境：Windows 7/8/10、Ubuntu、MacOS
- 工具：Env
- 工具链：GCC (gcc-arm-none-eabi)

| BSP名称               | 仓库 & 概述                                                                                                                                                                                                                                      | [标准引脚布局](/zh/beginner/arduino-pinout) | DigitalWrite & Read | AnalogWrite / PWM | AnalogRead / ADC | 串口  | I2C | SPI |
| ------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------- | ------------------- | ----------------- | ---------------- | --- | --- | --- |
| STM32F072 Nucleo    | [Github](https://github.com/RT-Thread/rt-thread/tree/master/bsp/stm32/stm32f072-st-nucleo/applications/arduino_pinout) / [Gitee](https://gitee.com/rtthread/rt-thread/tree/master/bsp/stm32/stm32f072-st-nucleo/applications/arduino_pinout) | √                                     | √                   | √                 | √                | √   | √   | √   |
| STM32F401 Nucleo    | [Github](https://github.com/RT-Thread/rt-thread/tree/master/bsp/stm32/stm32f401-st-nucleo/applications/arduino_pinout) / [Gitee](https://gitee.com/rtthread/rt-thread/tree/master/bsp/stm32/stm32f401-st-nucleo/applications/arduino_pinout) | √                                     | √                   | √                 | √                | √   | √   | √   |
| STM32F410 Nucleo    | [Github](https://github.com/RT-Thread/rt-thread/tree/master/bsp/stm32/stm32f410-st-nucleo/applications/arduino_pinout) / [Gitee](https://gitee.com/rtthread/rt-thread/tree/master/bsp/stm32/stm32f410-st-nucleo)                             | √                                     | √                   | √                 | √                | √   | √   | ×   |
| STM32F411 Nucleo    | [Github](https://github.com/RT-Thread/rt-thread/tree/master/bsp/stm32/stm32f411-st-nucleo/applications/arduino_pinout) / [Gitee](https://gitee.com/rtthread/rt-thread/tree/master/bsp/stm32/stm32f411-st-nucleo)                             | √                                     | √                   | √                 | √                | √   | √   | √   |
| STM32F412 Nucleo    | [Github](https://github.com/RT-Thread/rt-thread/tree/master/bsp/stm32/stm32f412-st-nucleo/applications/arduino_pinout) / [Gitee](https://gitee.com/rtthread/rt-thread/tree/master/bsp/stm32/stm32f412-st-nucleo)                             | √                                     | √                   | √                 | √                | √   | √   | √   |
| STM32L476 Nucleo    | [Github](https://github.com/RT-Thread/rt-thread/tree/master/bsp/stm32/stm32l476-st-nucleo/applications/arduino_pinout) / [Gitee](https://gitee.com/rtthread/rt-thread/tree/master/bsp/stm32/stm32l476-st-nucleo)                             | √                                     | √                   | √                 | √                | √   | √   | √   |
| STM32G474 Nucleo    | [Github](https://github.com/RT-Thread/rt-thread/tree/master/bsp/stm32/stm32g474-st-nucleo/applications/arduino_pinout) / [Gitee](https://gitee.com/rtthread/rt-thread/tree/master/bsp/stm32/stm32g474-st-nucleo)                             | √                                     | √                   | √                 | √                | √   | √   | √   |
| STM32U575 Nucleo    | [Github](https://github.com/RT-Thread/rt-thread/tree/master/bsp/stm32/stm32u575-st-nucleo/applications/arduino_pinout) / [Gitee](https://gitee.com/rtthread/rt-thread/tree/master/bsp/stm32/stm32u575-st-nucleo)                             | √                                     | √                   | √                 | √                | √   | √   | ×   |
| STM32F469 Discovery | [Github](https://github.com/RT-Thread/rt-thread/tree/master/bsp/stm32/stm32f469-st-disco/applications/arduino_pinout) / [Gitee](https://gitee.com/rtthread/rt-thread/tree/master/bsp/stm32/stm32f469-st-disco)                               | √                                     | √                   | √                 | √                | √   | √   | √   |
| STM32F103 BluePill  | [Github](https://github.com/RT-Thread/rt-thread/tree/master/bsp/stm32/stm32f103-blue-pill/applications/arduino_pinout) / [Gitee](https://gitee.com/rtthread/rt-thread/tree/master/bsp/stm32/stm32f103-blue-pill)                             | ×                                     | √                   | √                 | √                | √   | √   | √   |
| STM32F401 BlackPill | [Github](https://github.com/RT-Thread/rt-thread/tree/master/bsp/stm32/stm32f401-weact-blackpill/applications/arduino_pinout) / [Gitee](https://gitee.com/rtthread/rt-thread/tree/master/bsp/stm32/stm32f401-weact-blackpill)                 | ×                                     | √                   | √                 | √                | √   | √   | √   |
| STM32F411 BlackPill | [Github](https://github.com/RT-Thread/rt-thread/tree/master/bsp/stm32/stm32f411-weact-blackpill/applications/arduino_pinout) / [Gitee](https://gitee.com/rtthread/rt-thread/tree/master/bsp/stm32/stm32f411-weact-blackpill)                 | ×                                     | √                   | √                 | √                | √   | √   | √   |
| STM32L475潘多拉        | [Github](https://github.com/RT-Thread/rt-thread/tree/master/bsp/stm32/stm32l475-atk-pandora/applications/arduino_pinout) / [Gitee](https://gitee.com/rtthread/rt-thread/tree/master/bsp/stm32/stm32l475-atk-pandora)                         | ×                                     | √                   | √                 | √                | √   | √   | √   |
| STM32L431小熊派        | [Github](https://github.com/RT-Thread/rt-thread/tree/master/bsp/stm32/stm32l431-BearPi/applications/arduino_pinout) / [Gitee](https://gitee.com/rtthread/rt-thread/tree/master/bsp/stm32/stm32l431-BearPi)                                   | ×                                     | √                   | √                 | √                | √   | √   | √   |

## ** Renesas 瑞萨 **

- 编译环境：Windows 7/8/10、Ubuntu、MacOS
- 工具：Env
- 工具链：GCC (gcc-arm-none-eabi)

| BSP名称        | 仓库 & 概述                                                                                                                                                                                                                                    | [标准引脚布局](/zh/beginner/arduino-pinout) | DigitalWrite & Read | AnalogWrite / PWM | AnalogRead / ADC | 串口  | I2C | SPI |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------- | ------------------- | ----------------- | ---------------- | --- | --- | --- |
| 瑞萨 RA6M3 HMI | [Github](https://github.com/RT-Thread/rt-thread/tree/master/bsp/renesas/ra6m3-hmi-board/board/rtduino/arduino_pinout) / [Gitee](https://gitee.com/rtthread/rt-thread/tree/master/bsp/renesas/ra6m3-hmi-board/board/rtduino/arduino_pinout) | √                                     | √                   | √                 | √                | √   | √   | ×   |

## ** Raspberry Pi Pico **

- 编译环境：Ubuntu、MacOS
- 工具：Env
- 工具链：GCC (gcc-arm-none-eabi)

| BSP名称             | 仓库 & 概述                                                                                                                                                                                                                | [标准引脚布局](/zh/beginner/arduino-pinout) | DigitalWrite & Read | AnalogWrite / PWM | AnalogRead / ADC | 串口  | I2C | SPI |
| ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------- | ------------------- | ----------------- | ---------------- | --- | --- | --- |
| Raspberry Pi Pico | [Github](https://github.com/RT-Thread/rt-thread/tree/master/bsp/raspberry-pico/applications/arduino_pinout) / [Gitee](https://gitee.com/rtthread/rt-thread/tree/master/bsp/raspberry-pico/applications/arduino_pinout) | ×                                     | √                   | √                 | √                | √   | ×   | ×   |

<!-- tabs:end -->

### 2.2 RTduino社区适配BSP

以下 BSP 由 RTduino/RT-Thread 社区支持。**请在选择所需的对应BSP芯片厂商。**

<!-- tabs:start -->

## ** STM32 **

| BSP名称                                                                                                                                        | 资料  |
| -------------------------------------------------------------------------------------------------------------------------------------------- | --- |
| [大疆STM32F427 RoboMaster A板](https://github.com/RT-Thread/rt-thread/tree/master/bsp/stm32/stm32f427-robomaster-a/applications/arduino_pinout) |     |
| [大疆STM32F407 Robomaster C型](https://github.com/RT-Thread/rt-thread/tree/master/bsp/stm32/stm32f407-robomaster-c/applications/arduino_pinout) |     |

## ** NXP 恩智浦 **

| BSP名称                                                                                                                            | 资料  |
| -------------------------------------------------------------------------------------------------------------------------------- | --- |
| [NXP LPC55S69 EVK](https://github.com/RT-Thread/rt-thread/tree/master/bsp/lpc55sxx/lpc55s69_nxp_evk/applications/arduino_pinout) |     |

## ** CH32 **

| BSP名称                                                                                                                      | 资料  |
| -------------------------------------------------------------------------------------------------------------------------- | --- |
| [CH32V307V-R1](https://github.com/RT-Thread/rt-thread/tree/master/bsp/wch/risc-v/ch32v307v-r1/applications/arduino_pinout) |     |
| [CH32V208W-R0](https://github.com/RT-Thread/rt-thread/tree/master/bsp/wch/risc-v/ch32v208w-r0/applications/arduino_pinout) |     |

## ** 东软载波 **

| BSP名称                                                                                                              | 资料  |
| ------------------------------------------------------------------------------------------------------------------ | --- |
| [ES32F3696](https://github.com/RT-Thread/rt-thread/tree/master/bsp/essemi/es32f369x/applications/arduino_pinout)   |     |
| [ES32VF2264](https://github.com/RT-Thread/rt-thread/tree/master/bsp/essemi/es32vf2264/applications/arduino_pinout) |     |

<!-- tabs:end -->

## 3 编译工具链与环境

RTduino 作为 RT-Thread 软件包，其本身支持 GCC 工具链以及 Keil AC5、AC6 IDE，但由于 Arduino 社区第三方库均为 GCC 工具链下编写，**因此建议使用 GCC 工具链。RTduino 文档中心将完全基于 GCC 工具链以及 Env + VSCode 编译环境来进行讲解**。

## 4 官网与代码仓库

- 官网: http://www.rtduino.com/
- Github 代码仓库: https://github.com/RTduino/RTduino
- Gitee 代码仓库: https://gitee.com/rtduino/RTduino
- 文档中心（全球）：https://docs.rtduino.com
- 文档中心（国内）：https://rtduino.gitee.io
