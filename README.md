# Components


**MCP23017 Port expansion**

<img src="https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Ftse1.mm.bing.net%2Fth%3Fid%3DOIP.refT_4hqeIE-qElhNAWYCwHaCr%26pid%3DApi&f=1" alt="drawing" height="110"/> <img src="https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Ftse4.mm.bing.net%2Fth%3Fid%3DOIP.6HQeFNFjhfzpgxbkoepJJgHaEV%26pid%3DApi&f=1" alt="drawing" height="120"/><img src="https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Ftse4.mm.bing.net%2Fth%3Fid%3DOIP.6fE7VvNEaynar9eXcjTdaQHaCn%26pid%3DApi&f=1" height=155>


- provides 16-bit, general purpose parallel I/O expansion for I2C bus
- Individual pins read & write
- Interrupt support 
- Available addresses go from 0x20 to 0x27, allowing up to 8 MCP23017 on the same I2C bus.


- [Datasheet & specs](https://www.microchip.com/en-us/product/MCP23017)
- [Arduino library](https://www.arduino.cc/reference/en/libraries/mcp23017/)
- [using mcp23017 with rotary encoders](https://github.com/maxgerhardt/rotary-encoder-over-mcp23017)

*Note: MCP23**S**17 – provides SPI interface instead of I2C*

---
**MAX7219/MAX7221 8-Digit LED Display Driver**

![component](https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Ftse3.mm.bing.net%2Fth%3Fid%3DOIP.vDet4LwmxjFyfWAZVgCmNgHaEx%26pid%3DApi&f=1)

can be controlled using 3 pins, and each MAX72xx can be serialized with ogher MAX72xx
magin 3pins digits controlling more Display
- [Serially Interfaced, 8-Digit LED Display Drivers](https://datasheets.maximintegrated.com/en/ds/MAX7219-MAX7221.pdf) --> PAGE 13
- [Arduino exxample](https://www.ardumotive.com/8-digit-7seg-display-en.html)
- [Arduino exxample](https://www.instructables.com/MAX7219-7-Segment-Using-Arduino/)

---
**74HC595 8-bit shift register**

![component](https://user-images.githubusercontent.com/69033251/147548151-0b67c0d7-17cd-4fed-9026-2112d08f60f6.png)
<img src="https://haberocean.com/wp-content/uploads/2019/10/32-bit-shift-register-arduino-e1596739277671.jpg" width=300>

- It is a shift register with 8-bit serial input and 8-bit serial or 3-state parallel outputs.
- The operating voltage of this IC is from 2V to 6V.
- It can be easily cascaded through pin 9 with more IC to get more outputs.

Links
- [additional infos](https://microcontrollerslab.com/74hc595-shift-register-interfacing-arduino/)
- [example using IC for input using SN74HC165N](http://www.51hei.com/bbs/dpj-48284-1.html)
- [32-bit shift register using 75HC595](https://haberocean.com/2020/08/32-bit-shift-register-module-using-74hc595-controlled-using-arduino-uno/)

---
**4 button on 1 pin**

Using 1 Analog Pin to Read 4 Buttons

<img src="https://content.instructables.com/ORIG/FR1/H6DD/HT7P4YJM/FR1H6DDHT7P4YJM.jpg?auto=webp&frame=1&width=999&height=1024&fit=bounds&md=778fc1e5ca874763e493845743633902" width=300>

- [link here](https://www.instructables.com/Using-one-analog-pin-to-read-4-buttons-Arduino/)

---
***TP4056 LI-ION Battery charger***
<img src="https://ae01.alicdn.com/kf/HTB1NQTILxnaK1RjSZFtq6zC2VXao.jpg" width=250>
