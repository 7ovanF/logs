# Timer
Timers can be used to generate time delays in a program, or simply count. You can configure it so that an interrupt occurs when a certain goal has been reached.

There are two 8-bit and one 16-bit timers in AVR. Well... **for ATmega8515, there is only Timer 0 (8-bit timer) and Timer 1 (16-bit timer).**

The **Timer/Counter Interrupt Flag Register** is where AVR records that a timer event has happened. Events are specified below.

These are the definitions of each TIFR bit.

| Bit     | Meaning                   |
| ------- | ------------------------- |
| `TOV0`  | Timer0 overflow flag      |
| `OCF0`  | Timer0 compare match flag |
| `TOV1`  | Timer1 overflow flag      |
| `OCF1A` | Timer1 compare A flag     |
| `OCF1B` | Timer1 compare B flag     |
| `ICF1`  | Timer1 input capture flag |
> To address the bit position, you can `LDI r16, 1<<TOV0`

As you can see, Timer0 and Timer1 have different functionality.
### Common Timer Interrupt Events

#### Overflow
When the TCNT Counter Register 8-bit (255 to 0)

#### Compare Match

#### Input Capture

### Prescaler? No, he's only 8 months old.