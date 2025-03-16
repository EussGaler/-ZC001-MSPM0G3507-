# **[ZC001]基于64pin_MSPM0G3507的1.69寸LCD驱动程序**

ZC001--自存001

---

## **1. 引脚定义**

### **1.1 spi TFT液晶LCD屏**

| IO口 | 用途 | 说明 |
| :--: | :--: | :--: |
| PB10 | RES | GPIO |
| PB11 | DC | GPIO |
| PB14 | CS | GPIO |
| PB26 | BLK | GPIO |

### **1.2 其他**

| 外设/IO口 |   用途   | 说明 |
| :--: | :------: | :--: |
| UART0 | 调试 | 串口通信 |
| SPI1 | 通信 | 控制LCD屏 |
| DMA_CH0 | 搬运 | 与SPI1关联使用 |
| PB09 | SCLK | SPI1 |
| PB08 | PICO | SPI1 |
| PB07 | POCI | SPI1 |
| PA10 | UART0串口通信 | 开发板TX |
| PA11 | UART0串口通信 | 开发板DX |
| PB22 | 开发板LED上电测试 | GPIO |

## **2.版本说明**

1. `_s`后缀的文件为软件spi，`_h`后缀的文件为硬件spi，`_hd`后缀的文件为硬件spi+dma。
2. 目前只有在填充函数和显示图片函数中使用了DMA，其他的还没写。

## **3.注意事项**

1. Sanae_Pic图片几乎占用了单片机FLASH全部128k，比较极限。
2. spi频率为主频80MHz的一半40MHz，已经最大了。
3. LCD显示原理为`uint16_t`代表一个像素，格式为RGB565，即位数从高到低为红色5位，绿色6位，蓝色5位。可分别写入，如先写入`0xF8`再写入`0x10`则为`F810`，为程序中的RED。`0xFFFF`为白色，`0x0000`为黑色。写入前需先`setAddress`，即设定写入范围。
4. OLED显示原理为打包8个像素点，如128x64的屏幕，最后打包为128x8个`uint8_t`数据，每个数据的每一位代表一个像素点是亮是灭（1或0），如第一个数据从高到低每位分别代表第1行1列，2行1列，...，8行1列是亮是灭。
5. 串口通信有时候通信不了，可以重新插入JLink试试。
6. CCS有时候打了断点不管用，此时程序运行也会不正常，怀疑可能是跑飞了。
7. sysconfig自动生成的`ti_msp_dl_config.c`文件中可能**不会包含**`DL_SPI_enableDMATransmitEvent()`，即用SPI的TX interrupt触发DMA这一句！！（纯bug）。在driverlib的官方例程里`ti_msp_dl_config.c`文件里是有这一句的，所以可以正常运行。假如不自己在初始化时加上这一句的话DMA就不会工作！！
8. DMA在disableChannel后若想要重新enableChannel则需要再次设置源地址、目标地址及TransferSize，类似于ADC。

