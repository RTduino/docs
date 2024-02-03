# 以命令行方式编译运行sketch文件

在 Arduino 中，编程源码文件被称之为 "sketch" (草稿之意)，其文件后缀为 `.ino`，具体 Arduino IDE 对 sketch 文件的编译方法，参见 [Arduino CLI文档](https://arduino.github.io/arduino-cli/0.33/sketch-build-process/) 。

RTduino支持对 `.ino` 文件的直接编译，方法很简单，使用命令 `scons -j20 --sketch=".ino文件的绝对路径"` 即可完成编译，其中 `-j20` 表示20个CPU并发编译（取决于计算机有多少个核心）以提高编译速度，`--sketch=""` 用于指定 `.ino` 文件的绝对路径。这样RTduino就会单独创建一个新的线程运行该sketch文件内的源码。
