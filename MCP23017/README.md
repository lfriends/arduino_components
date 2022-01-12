# MCP23017

![pinout](https://www.best-microcontroller-projects.com/image-files/mcp23017_breadboard.jpg?ezimgfmt=rs:450x606/rscb1/ng:webp/ngcb1)

[additional infos](https://www.best-microcontroller-projects.com/mcp23017.html)



## Pinout

The MCP23017 has internal pullups for each input pin (configurable) so there is no need to add external pull up resistors for inputs such as push buttons etc.

![pins](https://www.best-microcontroller-projects.com/image-files/mcp23017-pinout.jpg?ezimgfmt=rs:236x187/rscb1/ng:webp/ngcb1)


**Note**: In the library pins are labelled from 0 to 15 where:

- pin 0 is bit 0 of port A
- pin 7 is bit 7 of port A
- pin 8 is bit 0 of port B
- pin 15 is bit 7 of port B

| PORT B | PORT A | 
|--------|--------|
|    8   |    7   |
|    9   |    6   |
|   10   |    5   |
|   11   |    4   |
|   12   |    3   |
|   13   |    2   |
|   14   |    1   |
|   15   |    0   |
|  ...   |   ...  |

## addressing Addressing

The 23017 has three input pins to allow you to set a different address for each attached MCP23017

MCP23017 I2C address range is 32 decimal to 37 decimal or 0x20 to 0x27 for the MCP23017 (a maximum of eight MCP23017 devices can be attached to any single I2C bus).
```c++
mcp.begin();      // Default device address 0
```


## Code example  (simple - w/o interrupts)
:shipit:

```c++
#include <Adafruit_MCP23017.h>
Adafruit_MCP23017 mcp;

void setup(){
    mcp.begin();      // Default device address 0
    mcp.pinMode( 11, OUTPUT);  // Toggle LED 1
    mcp.pinMode( 8, INPUT);   // Button i/p to GND
    mcp.pullUp( 8, HIGH);     // Puled high to ~100k
 }

void loop() {
  
  if (mcp.digitalRead( 8 )) {
     mcp.digitalWrite( 11,HIGH);
  } else {
     mcp.digitalWrite( 11,LOW);
  }
  delay(500);
} 
```
## Code example  (with interrupts to avoid lag of the I2C)

Of course this a slightly contrived example as you can easily change the code for faster polling or use dynamic delays (using the millis function).
This is where interrupts become essential.

**Important**: To clear interrupts you must read back data from either INTCAP(A/B) or GPIO(A/B).

```c++
// MCP23017 Example: Interrupt operation.
//
// This code sends an interrupt from the MCP23017
// to an Arduino external interrupt pin when an
// MCP23017 input button press is detected.
// At the same time MCP LEDs are toggled on button
// release.
//
// Copyright : John Main
// Free for non commercial use.

#include <Wire.h>
#include <Adafruit_MCP23017.h>

// MCP23017 setup
#define MCP_LED1 7
#define MCP_INPUTPIN 8
#define MCP_INPUTPIN2 10
#define MCP_LEDTOG1 11
#define MCP_LEDTOG2 4
// Register bits
#define MCP_INT_MIRROR true  // Mirror inta to intb.
#define MCP_INT_ODR    false // Open drain.

// Arduino pins
#define INTPIN 3   // Interrupt on this Arduino Uno pin.

#define controlArduioInt attachInterrupt(digitalPinToInterrupt(INTPIN),isr,FALLING)

Adafruit_MCP23017 mcp;

//////////////////////////////////////////////
void setup(){

  mcp.begin();      // use default address 0

  pinMode(INTPIN,INPUT);

  mcp.pinMode(MCP_LEDTOG1, OUTPUT);  // Toggle LED 1
  mcp.pinMode(MCP_LEDTOG2, OUTPUT);  // Toggle LED 2

  mcp.pinMode(MCP_LED1, OUTPUT);     // LED output
  mcp.digitalWrite(MCP_LED1,LOW);

  // This Input pin provides the interrupt source.
  mcp.pinMode(MCP_INPUTPIN,INPUT);   // Button i/p to GND
  mcp.pullUp(MCP_INPUTPIN,HIGH);     // Puled high ~100k

  // Show a different value for interrupt capture data register = debug.
  mcp.pinMode(MCP_INPUTPIN2,INPUT);   // Button i/p to GND
  mcp.pullUp(MCP_INPUTPIN2,HIGH);     // Puled high ~100k

  // On interrupt, polariy is set HIGH/LOW (last parameter).
  mcp.setupInterrupts(MCP_INT_MIRROR, MCP_INT_ODR, LOW); // The mcp output interrupt pin.
  mcp.setupInterruptPin(MCP_INPUTPIN,CHANGE); // The mcp  action that causes an interrupt.

  mcp.setupInterruptPin(MCP_INPUTPIN2,CHANGE); // No button connected, see effect on code=none.

  mcp.digitalWrite(MCP_LED1,LOW);

  mcp.readGPIOAB(); // Initialise for interrupts.

  controlArduioInt; // Enable Arduino interrupt control.

}

//////////////////////////////////////////////
// The interrupt routine handles LED1
// This is the button press since this is the only active interrupt.
void isr(){
uint8_t p,v;
static uint16_t ledState=0;

   noInterrupts();

   // Debounce. Slow I2C: extra debounce between interrupts anyway.
   // Can not use delay() in interrupt code.
   delayMicroseconds(1000);

   // Stop interrupts from external pin.
   detachInterrupt(digitalPinToInterrupt(INTPIN));
   interrupts(); // re-start interrupts for mcp

   p = mcp.getLastInterruptPin();
   // This one resets the interrupt state as it reads from reg INTCAPA(B).
   v = mcp.getLastInterruptPinValue();

   // Here either the button has been pushed or released.
   if ( p==MCP_INPUTPIN && v == 1) { //  Test for release - pin pulled high
      if ( ledState ) {
         mcp.digitalWrite(MCP_LED1, LOW);
      } else {
         mcp.digitalWrite(MCP_LED1, HIGH);
      }

      ledState = ! ledState;
   }

   controlArduioInt;  // Reinstate interrupts from external pin.
}


//////////////////////////////////////////////
void loop(){

  delay(300);

  mcp.digitalWrite(MCP_LEDTOG1, HIGH);
  mcp.digitalWrite(MCP_LEDTOG2, LOW);

  delay(300);

  mcp.digitalWrite(MCP_LEDTOG1, LOW);
  mcp.digitalWrite(MCP_LEDTOG2, HIGH);

}
```

---
## TIP
If your MCP23017 is getting hot, then look at the overall output current from the chip by calculating the current from each pin and then adding up these results. If it is more than 150mA then reduce the current by increasing the current limiting resistors for each pin.

The above specification shows that the device is quite capable of driving current to LEDs however there are 16 outputs so the maximum output current for the whole device has to be shared by all the LEDs.

**Warning**: Maximum total current chip is 150mA, and the maximum for an individual pin is 25mA.

If you drive 16 LEDs then it will be 150mA/16 =9.38mA per GPIO pin (MAX). This also assumes that you are turning the LED on when it is driven low. Note: Choose a lower current to keep within limits.

Remember that different LED colours have a different forward voltage drop 