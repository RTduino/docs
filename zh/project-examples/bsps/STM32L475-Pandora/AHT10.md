# STM32L475潘多拉板驱动AHT10温湿度传感器

本文介绍如何在 STM32L475 潘多拉开发板使用基于 RTduino 环境，使用 Arduino 库驱动板载 AHT10 温湿度传感器。

潘多拉板目录位于 `rt-thread\bsp\stm32\stm32l475-atk-pandora`

```Kconfig
Hardware Drivers Config --->
    Onboard Peripheral Drivers --->
        [*] Compatible with Arduino Ecosystem (RTduino)
        [*]   Enable Arduino AHT10 sensor library
        [*]     Enable Arduino AHT10 sensor library demo  
```

其中：

- Compatible with Arduino Ecosystem (RTduino)：本 BSP 开启 RTduino，具备对 Arduino 库的兼容能力。
- Enable Arduino AHT10 sensor library：使能 Arduino AHT10 驱动库，会自动依赖 Arduino [Adafruit AHT10/20温湿度传感器库](/zh/library-examples/sensors/Adafruit/Adafruit-AHTX0/Adafruit-AHTX0)。
- Enable Arduino AHT10 sensor library demo：开启 AHT10 演示示例，开机自动运行。

在串口终端输出如下：

```shell
Temperature: 28.214455 *C
Humidity: 22.776508 %
Temperature: 28.206253 *C
Humidity: 22.781277 %
Temperature: 28.205299 *C
Humidity: 22.791576 %
Temperature: 28.207207 *C
Humidity: 22.804832 %
Temperature: 28.205299 *C
Humidity: 22.796726 %
Temperature: 28.201103 *C
Humidity: 22.804070 %
Temperature: 28.200340 *C
Humidity: 22.813702 %
Temperature: 28.198051 *C
Humidity: 22.813606 %
```
