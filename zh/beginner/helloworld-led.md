# HelloWorld - 点亮一颗LED

## 1 Arduino经典的setup和loop函数在哪里？

对于 Arduino，最经典的莫过于 `setup` 和 `loop` 函数。这两个函数位于 BSP 目录下的 `applications` 文件夹内 `arduino_main.cpp` 文件中。以 STM32F411 Nucleo 板为例，这两个函数位于：`bsp/stm32/stm32f411-st-nucleo/applications/arduino_main.cpp` 文件中，在开启 RTduino 软件包后，你可以直接在工程的 Applications 组中找到它。

## 2 点亮LED并通过串口向外输出

将 `arduino_main.cpp` 中的内容全部用以下内容代替：

```c
#include <Arduino.h>

void setup(void)
{
    // put your setup code here, to run once:
    pinMode(LED_BUILTIN, OUTPUT);
    // LED_BUILTIN 表示板载LED灯，不同的BSP的LED灯对应的引脚不同，
    // 但是 LED_BUILTIN 会提前预设好，因此用户可以无需关心板载LED灯的
    // 引脚编号具体是什么，可以直接调用 LED_BUILTIN 。
}

void loop(void)
{
    // put your main code here, to run repeatedly:
    digitalWrite(LED_BUILTIN, !digitalRead(LED_BUILTIN));
    Serial.println("Hello RTduino!");
    delay(100);
}
```

编译并下载到板卡中后，就可以看到，板载的LED已经开始闪烁，并且串口开始输出 "Hello RTduino!" 了。

> ⚠️注意：
> 
> 由于 RT-Thread 的 main.c 文件内，也会默认闪烁一个 LED 灯，如果板子上只有一个 LED 灯的话，两个线程会发生干涉。但是你会发现这个 LED 的闪烁速度明显变快了。因为 main.c 那边的闪烁周期是 1000ms，上面这个例程是 200ms。
> 

## 3 板卡引脚分布

由于每个 BSP 的板子设计、以及芯片型号等，引脚分布是有区别的，因此需要到指定 BSP 的 `applications/arduino` 文件夹下的README.md文件查看详细信息。例如：

- [STM32F401 Nucleo板的Arduino引脚布局的详细说明](https://github.com/RT-Thread/rt-thread/tree/master/bsp/stm32/stm32f401-st-nucleo/applications/arduino_pinout)
- [STM32F411 Nucleo板的Arduino引脚布局的详细说明](https://github.com/RT-Thread/rt-thread/tree/master/bsp/stm32/stm32f411-st-nucleo/applications/arduino_pinout)
- [STM32F407星火一号的Arduino引脚布局的详细说明](https://github.com/RT-Thread/rt-thread/tree/master/bsp/stm32/stm32f407-rt-spark/applications/arduino_pinout)
- [STM32L475潘多拉板的Arduino引脚布局的详细说明](https://github.com/RT-Thread/rt-thread/tree/master/bsp/stm32/stm32l475-atk-pandora/applications/arduino_pinout)
