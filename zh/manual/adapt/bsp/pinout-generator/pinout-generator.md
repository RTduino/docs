# pinout-generator工具

[pinout-generator](https://github.com/RTduino/pinout-generator) 是一款使用Qt框架编写的代码生成软件。它致力于帮助开发人员快速、高效地将 RTduino 对接至 RT-Thread BSP，软件以图形界面的方式来替代了用户手动修改文件对接的繁琐过程，通过简单的图形化操作快速生成对接 RT-Thread BSP 需要的各个文件并完成对接。该软件不仅预防了用户手动对接过程中出现误操作的问题，同时也大大提高了对接效率。目前该软件已经支持的芯片系列为STM32、瑞萨等，当然用户也可根据实际需要自行添加芯片系列，参考[如何支持新MCU索引](/zh/manual/adapt/bsp/pinout-generator/add-mcu)文档自行添加。

pinout-generator工具的主要特点包括：
- 使用图形界面进行操作，简单易用
- 支持多种芯片系列，可根据需要自行添加
- 快速生成对接 RT-Thread BSP 需要的各个文件
- 避免手动对接过程中的误操作问题
- 提高对接效率

![add-mcu-adapt](./figures/add-mcu-adapt.png)
