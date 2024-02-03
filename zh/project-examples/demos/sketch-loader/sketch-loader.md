# sketch loader使用示例

## 1 简介

sketch loader 示例（demo）展示了如何使用 RTduino sketch loader 机制，该机制的介绍参见 RTduino 文档中心 [sketch loader自动初始化机制](/zh/manual/multi-threading/sketch-loader)章节。

### 1.1 仓库地址

- 官方（Github）：https://github.com/RTduino-libraries/sketch-loader-demo
- 镜像源（Gitee）：https://gitee.com/RT-Thread-Mirror/sketch-loader-demo

### 1.2 自动依赖Arduino库

- 无

### 1.3 BSP要求

- 无

## 2 如何运行本示例

本节以**具有 Arduino UNO 标准引脚布局**的BSP为例，讲解如何运行本示例。

### 2.1 开启RTduino

在 BSP 根目录下目录下，进入 `menuconfig` 后，先选择 `Compatible with Arduino Ecosystem (RTduino)`，开启 RTduino，让 BSP 具备兼容 Arduino 生态的能力：

```Kconfig
Hardware Drivers Config --->
    Onboard Peripheral Drivers --->
        [*] Compatible with Arduino Ecosystem (RTduino)
```

### 2.2 开启示例软件包

RTduino 线程间通信示例软件包已经注册到 RT-Thread 软件包中心：

```Kconfig
RT-Thread online packages --->
    Arduino libraries  --->
        Projects and Demos  --->
            [*] RTduino sketch loader demo
```

### 2.3 编译运行

用 `scons -j12` 命令编译，并将 `.bin` 或 `.elf` 文件烧录到板卡中。

板卡上电后打开串口终端，调整接收波特率为115200（RT-Thread 默认波特率），可以看到在串口助手终端上循环打印如下内容：

```shell
loader1 loop() is running...
loader2 loop() is running...
loader2 loop() is running...
loader2 loop() is running...
loader1 loop() is running...
loader2 loop() is running...
loader2 loop() is running...
loader1 loop() is running...
loader2 loop() is running...
loader2 loop() is running...
```
