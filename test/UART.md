---
sort: 17
---

# UART

## UART 唤醒机制

UART使用异步串行模式进行数据发送与接收，异步指的是没有用时钟信号来同步发送设备进入接收设备，而是通过相同的波特率来处理收发端的同步点。
但是数据总线和UART数据交换是以并行方式进行的。
！[](https://www.analog.com/-/media/images/analog-dialogue/en/volume-54/number-4/articles/uart-a-hardware-communication-protocol/335962-fig-02.svg?w=900)

在SoC中，UART在休眠时是不工作的，需要以唤醒后,使能了主频晶振和IO功能