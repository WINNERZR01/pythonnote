# 8.TPYBoard v202 I2C操作DS3231时钟模块

> - 学习TPYBoard v202中I2C接口的使用方法
> - 学习使用`DS3231`制作简易的电子时钟
>
> `I2C`(Inter-Integrated Circuit(集成电路总线))
>
> `I2C`总线类型是由飞利浦半导体公司在八十年代初设计出来的一种简单、双向、二线制、同步串行总线，主要是用来连接整体电路(ICS)。
>
> `I2C`是一种多向控制总线，也就是说多个芯片可以连接到同一总线结构下，同时每个芯片都可以作为实时数据传输的控制源。

## MicroPython中I2C类库介绍

```python
from machine import Pin, I2C

# construct an I2C bus
i2c = I2C(scl=Pin(5), sda=Pin(4), freq=100000)

i2c.readfrom(0x3a, 4)   # read 4 bytes from slave device with address 0x3a
i2c.writeto(0x3a, '12') # write '12' to slave device with address 0x3a

buf = bytearray(10)     # 创建一个十个字节的缓存区
i2c.writeto(0x3a, buf)  # write the given buffer to the slave
```

`TPYBoard v202`的`I2C`接口中SCL=Pin(14),SDA=Pin(2)，`I2C`初始化修改为如下：

> ```python
> i2c = I2C(scl=Pin(14), sda=Pin(2), freq=100000)
> ```

## DS3231时钟芯片的基本介绍

DS3231是低成本、高精度I2C实时时钟(RTC)，具有集成的温补晶振(TCXO)和晶体。该器件包含电池输入端，断开主电源时仍可保持精确的计时。

集成晶振提高了器件的长期精确度，并减少了生产线的元件数量。DS3231提供商用级和工业级温度范围，采用16引脚300mil的SO封装。

DS3231内部集成了一个非常精确的数字温度传感器，可通过I2C接口对其进行访问(同读取时间一样)，精度为±3°C。

## 硬件的连接

- `示意图`

> | TPYBoard v202 | DS3231时钟模块 |
> | ------------- | -------------- |
> | 3.3V          | VCC            |
> | GND           | GND            |
> | SDA           | SDA            |
> | SCL           | SCL            |

- `main.py`

  ```python
  import machine
  import time
  from ds3231 import DS3231
  
  ds=DS3231()
  ds.DATE([17,9,1])
  ds.TIME([10,10,10])
  
  while True:
      print('Date:',ds.DATE())
      print('Time:',ds.TIME())
      print('TEMP:',ds.TEMP())
      time.sleep(5)
  ```

# 