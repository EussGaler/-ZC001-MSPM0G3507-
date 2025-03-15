# **[ZC001]基于64pin_MSPM0G3507的1.69寸LCD驱动程序**

ZC001--自存001

---

## **1. 引脚定义**

### **1.1 spi TFT液晶LCD屏**

| IO口 | 用途 | 说明 |
| :--: | :--: | :--: |
| PB09 | SCL | GPIO |
| PB08 | SDA | GPIO |
| PB10 | RES | GPIO |
| PB11 | DC | GPIO |
| PB14 | CS | GPIO |
| PB26 | BLK | GPIO |

### **1.2 其他**

| 外设/IO口 |   用途   | 说明 |
| :--: | :------: | :--: |
| UART0 | 调试 | 串口通信 |
| PA10 | UART0串口通信 | 开发板TX |
| PA11 | UART0串口通信 | 开发板DX |
| PB22 | 开发板LED上电测试 | GPIO |

## **2.说明**

1. Sanae_Pic图片几乎占用了单片机FLASH全部128k，比较极限
