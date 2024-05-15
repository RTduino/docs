# 如何为RT-Thread BSP适配RTduino

**本章介绍采用纯手工方式**将 RT-Thread BSP 适配到 RTduino。目前，部分 RT-Thread BSP 已经支持自动化辅助工具pinout-generator来生成对接模板，方便用户快速对接，详情参考[pinout-generator工具](/zh/manual/adapt/bsp/pinout-generator/pinout-generator.md)章节。

## 1 确认BSP驱动已经适配

RTduino 功能依赖于如下 RT-Thread 设备框架和驱动的支持，对接前需要确认对应 RTduino 功能的 BSP 驱动是否已经完成适配。

| RTduino功能             | RT-Thread BSP驱动  |
| --------------------- | ---------------- |
| ditigalRead / Write() | PIN驱动            |
| analogWrite()         | PWM驱动            |
| analogRead()          | ADC驱动            |
| Serial                | 串口驱动             |
| Wire                  | I2C驱动            |
| SPI                   | SPI驱动            |
| Tone / noTone()       | 硬件定时器(hwtimer)驱动 |
| Servo                 | PWM驱动            |

## 2 创建文件夹和文件

需要在某个BSP的applications文件夹下创建如下文件、文件夹：

参考示例BSP：[STM32F401 Nucleo板applications文件夹](https://github.com/RT-Thread/rt-thread/tree/master/bsp/stm32/stm32f401-st-nucleo/applications/arduino_pinout) | [STM32F411 Nucleo板applications文件夹](https://github.com/RT-Thread/rt-thread/tree/master/bsp/stm32/stm32f411-st-nucleo/applications/arduino_pinout) | [STM32L475 潘多拉板applications文件夹](https://github.com/RT-Thread/rt-thread/tree/master/bsp/stm32/stm32l475-atk-pandora/applications)

### 2.1 arduino_main.cpp文件

该文件是 Arduino 的编程入口，提供 setup 和 loop 函数。在 loop 函数默认以 200ms 为周期，闪烁 Arduino 内建 LED 灯（LED_BUILTIN）。如例：

```c
#include <Arduino.h>

void setup(void)
{
    /* put your setup code here, to run once: */
    Serial.begin();
    Serial.println("Hello RTduino!");
}

void loop(void)
{
    /* put your main code here, to run repeatedly: */
    delay(1000);
}
```

如果 LED_BUILTIN 被 RT-Thread 默认主线程的 LED 闪烁示例占用，可以在 loop 函数内以 `Serial.println("Hello Arduino\n");` 代替频闪 LED。如例：

```c
#include <Arduino.h>

void setup(void)
{
    /* put your setup code here, to run once: */
    Serial.begin();
    Serial.println("Hello RTduino!");
}

void loop(void)
{
    /* put your main code here, to run repeatedly: */
    delay(1000);
}
```

### 2.2 arduino_pinout文件夹

需要在applications文件夹下创建arduino_pinout文件夹，这个文件夹主要包含 `arduino_pinout.c` 和 `arduino_pinout.h` 两个关键的文件，这两个文件是对接的关键。用户只需要做好这两个文件，即可完成与RTduino的对接。

同时，这个文件夹内也需要SConscript脚本文件，以及提供Arduino引脚布局的README说明文档。请参照上面的示例BSP来完成对这两个文件的编写。

### 2.3 arduino_pinout.c 文件的编写

`arduino_pinout.c` 内需要完成一个IO编号和功能的映射表。由于Arduino的习惯是采用1-13 (D0-D13) 以及 A0-A5的引脚编号，而正规的MCU的引脚编号一般都是PA1之类，因此需要将MCU真正的引脚编号与Arduino引脚编号映射起来。

以下段代码来举例讲解：

```c
/* 按照先数字引脚后模拟引脚的顺序从0开始，一定要按序排列 */
/* 可以按照板卡实际IO情况，灵活调整功能，不一定非得按照Arduino UNO的引脚功能布局，但是建议按此布局设计 */
/*
 * {Arduino Pin, RT-Thread Pin [, Device Name, Channel]}
 * [] means optional
 * Digital pins must NOT give the device name and channel.
 * Analog pins MUST give the device name and channel(ADC, PWM or DAC).
 * Arduino Pin must keep in sequence.
 */
const pin_map_t pin_map_table[]=
{
    {D0, GET_PIN(A,3), "uart2"},        /* Serial-Rx */
    {D1, GET_PIN(A,2), "uart2"},        /* Serial-Tx */
    {D2, GET_PIN(A,10)},
    {D3, GET_PIN(B,3), "pwm2", 2},      /* PWM */
    {D4, GET_PIN(B,5)},
    {D5, GET_PIN(B,4), "pwm3", 1},      /* PWM */
    {D6, GET_PIN(B,10), "pwm2", 3},     /* PWM */
    {D7, GET_PIN(A,8)},
    {D8, GET_PIN(A,9)},
    {D9, GET_PIN(C,7), "pwm3", 2},      /* PWM */
    {D10, GET_PIN(B,6), "pwm16", -1},   /* PWM */
    {D11, GET_PIN(A,7), "pwm17", 1},    /* PWM */
    {D12, GET_PIN(A,6)},
    {D13, GET_PIN(A,5)},                /* LED_BUILTIN */
    {D14, GET_PIN(B,9), "i2c1"},        /* I2C-SDA (Wire) */
    {D15, GET_PIN(B,8), "i2c1"},        /* I2C-SCL (Wire) */
    {D16, GET_PIN(C,13)},               /* USER KEY */
    {A0, GET_PIN(A,0), "adc1", 0},      /* ADC */
    {A1, GET_PIN(A,1), "adc1", 1},      /* ADC */
    {A2, GET_PIN(A,4), "adc1", 4},      /* ADC */
    {A3, GET_PIN(B,0), "adc1", 8},      /* ADC */
    {A4, GET_PIN(C,1), "adc1", 11},     /* ADC */
    {A5, GET_PIN(C,0), "adc1", 10},     /* ADC */
    {A6, RT_NULL, "adc1", 17},          /* ADC, On-Chip: internal reference voltage, ADC_CHANNEL_VREFINT */
    {A7, RT_NULL, "adc1", 16},          /* ADC, On-Chip: internal temperature sensor, ADC_CHANNEL_TEMPSENSOR */
};
```

如上截取展示了 IO 编号和功能映射表，每一行用花括号包裹（实际是一个结构体）来建议一个IO的映射关系：

```
{Arduino 引脚编号, RT-Thread 引脚编号(通过 GET_PIN 宏获取), 复用功能的设备名(PWM、ADC 或 DAC), 该复用功能设备的通道号}
```

其中，Arduino引脚编号，即是第一个参数，是必填的，D0 - Dx 或者是 A0 - Ax。**注意一定要按先数字引脚后模拟引脚照顺序来填写**。

RT-Thread 引脚编号，即第二个参数，rt_pin_write 中引脚编号填什么，这里就填什么，一般使用 `GET_PIN` 宏来获取。注意：D0、D1 以及I2C、SPI IO 需要将此参数略过。

后两个参数是复用功能 IO 才需要填写的，普通引脚只需要略过即可。

### 2.4 arduino_pinout.h 文件的编写

该文件主要负责定义各种宏，包括：

D0、A0等引脚的数字宏，该宏一定要按照先数字引脚后模拟引脚的顺序进行排号。

定义默认开启的一些硬件功能，如下表所示：

```c
/* pins alias. Must keep in sequence */
/* 按照先数字引脚后模拟引脚的顺序从0开始，一定要按序排列 */
/* 可以按照板卡实际IO情况，灵活调整功能，不一定非得按照Arduino UNO的引脚功能布局，但是建议按此布局设计 */
#define D0        (0)
#define D1        (1)
#define D2        (2)
#define D3        (3)
#define D4        (4)
#define D5        (5)
#define D6        (6)
#define D7        (7)
#define D8        (8)
#define D9        (9)
#define D10       (10)
#define D11       (11)
#define D12       (12)
#define D13       (13)
#define D14       (14)
#define D15       (15)
#define D16       (16)
#define A0        (17)
#define A1        (18)
#define A2        (19)
#define A3        (20)
#define A4        (21)
#define A5        (22)
#define A6        (23)
#define A7        (24)

#define RTDUINO_PIN_MAX_LIMIT A7 /* 定义该宏，RTduino将对用户输入引脚有效性进行检查，该宏应为引脚宏的最大值 */

#define F_CPU  84000000L /* CPU: 80MHz，定义CPU的主频 */
#define LED_BUILTIN  D13 /* Default Built-in LED，定义Arduino内置LED的引脚编号 */

/* 定义I2C设备名称，在使用Wire库时会直接调用。可选，如果没有I2C功能，不需要定义该宏 */
#define RTDUINO_DEFAULT_IIC_BUS_NAME            "i2c1"

/* 定义SPI设备名称，在使用SPI库时会直接调用。可选，如果没有SPI功能，不需要定义该宏 */
#define RTDUINO_DEFAULT_SPI_BUS_NAME            "spi1"
#define SS  D7 /* 定义SPI片选引脚 */

/* 
   定义高精度定时器设备名称，该宏主要是提供us时基信号使用。
   所有Cortex-M核MCU均不需要定义此宏，RTduino会自动调用systick来计算us级时间戳。
   非Cortex-M核的MCU需要提供一个硬件定时器来提供us级时间戳。
   依赖该定时器的函数有:
   - micros() [位于core/wiring.c] 提供us级时间戳，该函数为若函数，默认使用用户
     提供的1us级硬件高精度定时器，用户也可以在pins_arduino.c中重写该函数，由用户自定义us时钟提供方法。
   - pulseIn() pulseInLong() [位于core/wiring_pulse.c] 依赖 micros()函数
 */
#define RTDUINO_1US_HWTIMER_DEVICE_NAME     "timer9"

/* Tone() noTone() [位于core/tone.c] 功能使用的高精度硬件定时器，如果不需要该功能可以不定义该宏 */
#define RTDUINO_TONE_HWTIMER_DEVICE_NAME    "timer7"

/* 如果有串口2可以定义串口2的设备名称，若没有可直接不定义此宏 */
#define RTDUINO_SERIAL2_DEVICE_NAME          "uart2"

/* 如果有串口3可以定义串口3的设备名称，若没有可直接不定义此宏 */
#define RTDUINO_SERIAL2_DEVICE_NAME          "uart3"
```

## 3 修改Kconfig文件

Kconfig 文件位于 BSP 的 board 文件夹下：

参考示例BSP：[STM32F401 Nucleo板Kconfig](https://github.com/RT-Thread/rt-thread/blob/master/bsp/stm32/stm32f401-st-nucleo/board/Kconfig) | [STM32F411 Nucleo板Kconfig](https://github.com/RT-Thread/rt-thread/blob/master/bsp/stm32/stm32f411-st-nucleo/board/Kconfig) | [STM32L475 潘多拉板Kconfig](https://github.com/RT-Thread/rt-thread/blob/master/bsp/stm32/stm32l475-atk-pandora/board/Kconfig)

```Kconfig
menu "Onboard Peripheral Drivers"
    config BSP_USING_STLINK_TO_USART
        bool "Enable STLINK TO USART (uart2)"
        select BSP_USING_UART
        select BSP_USING_UART2
        default y

    #增加 BSP_USING_ARDUINO 配置选项
    config BSP_USING_ARDUINO
        bool "Compatible with Arduino Ecosystem (RTduino)"
        select PKG_USING_RTDUINO
        select BSP_USING_STLINK_TO_USART
        select BSP_USING_GPIO
        select BSP_USING_ADC
        select BSP_USING_ADC1
        select BSP_USING_PWM
        select BSP_USING_PWM1
        select BSP_USING_PWM1_CH1
        select BSP_USING_PWM2
        select BSP_USING_PWM2_CH2
        select BSP_USING_PWM2_CH3
        select BSP_USING_PWM3
        select BSP_USING_PWM3_CH1
        select BSP_USING_PWM3_CH2
        select BSP_USING_PWM4
        select BSP_USING_PWM4_CH1
        select BSP_USING_I2C
        select BSP_USING_I2C1
        select BSP_USING_SPI
        select BSP_USING_SPI1
        select BSP_SPI1_TX_USING_DMA
        select BSP_SPI1_RX_USING_DMA
        select RTDUINO_USING_WIRE
        select RTDUINO_USING_SPI
        imply RTDUINO_USING_SERVO
        default n

endmenu
```

需要在 `Onboard Peripheral Drivers` 栏下增加 `BSP_USING_ARDUINO` 配置选项，并依赖相应的 PWM、ADC、UART、I2C 以及 SPI 等设备框架，满足一键化开启RTduino的能力。

## 4 编写Arduino引脚布局(pinout)的README说明文档

示例：[STM32F401 Nucleo的Arduino引脚布局说明文档](https://github.com/RT-Thread/rt-thread/blob/master/bsp/stm32/stm32f401-st-nucleo/applications/arduino_pinout/README.md) | [STM32F411 Nucleo的Arduino引脚布局说明文档](https://github.com/RT-Thread/rt-thread/blob/master/bsp/stm32/stm32f411-st-nucleo/applications/arduino_pinout/README.md) | [STM32L475潘多拉的Arduino引脚布局说明文档](https://github.com/RT-Thread/rt-thread/blob/master/bsp/stm32/stm32l475-atk-pandora/applications/arduino_pinout/README.md)

该文档需位于`applications/arduino_pinout/README.md`，主要介绍该BSP下的Arduino引脚编号和引脚功能，以及注意事项等。

## 5 SPI 对接注意事项

SPI 功能对于板卡来说是非必须功能，如果板卡支持，则尽量对接。

SPI的功能属性不是默认IO模式，即若SPI引脚与部分PWM和通用IO功能冲突，PWM或者通用IO才是板卡上电之后的默认IO模式，用户调用 `SPI.begin()` 时，会将SPI端口**永久不可逆地**转换为 SPI-MOSI、SPI-MISO、SPI-CLK。

SPI不使用硬件片选，由Arduino应用程序自己进行软件片选操作，因此适配仅需关心SPI-MOSI、SPI-MISO 和 SPI-CLK三个引脚。SPI-CS这个引脚就是一个正常的通用IO，通过 `digitalWrite()` 函数在 Arduino 应用程序中自行操作。适配时，需要在 `pins_arduino.c` 文件中实现 `switchToSPI()` 函数，详见PR：https://github.com/RT-Thread/rt-thread/pull/7901。
