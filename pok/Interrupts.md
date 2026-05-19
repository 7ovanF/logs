# Interrupts

A signal that will, as its name states, interrupt whatever process is running so that a its associated **ISR** is executed.

**ISR (Interrupt Service Routine)**: the program associated with the interrupt, which will be called upon receiving that signal. Can also be called the interrupt handler.

Each ISR's address is contained by an **interrupt vector** — listed in the **interrupt vector table**, which is usually at the very beginning of the program memory. (Controllable; mentioned far below)

Here's an example of an interrupt vector table.
``` AVR
; === INTERRUPT VECTOR TABLE EXAMPLE ===
; It may be sequential like this:
.org 0x0000
	rjmp RESET
	rjmp WHATEVER_VECTOR_NO_1_IS
	
; Or, apparently, in most examples:
.org 0x0000
    rjmp RESET          ; Reset vector

.org INT0addr
    rjmp INT0_ISR       ; External Interrupt 0

.org TIMER1_OVFaddr
    rjmp TIMER1_OVF_ISR ; Timer1 Overflow Interrupt

.org USART_RXCaddr
    rjmp USART_RX_ISR   ; USART Receive Complete Interrupt
```

There are internal and external interrupts: external interrupts are triggered by signal changes on dedicated interrupt pins, where an external device is connected and is what creates the signals, while internal interrupts are triggered by things such as timers or counters.

---
### Steps:

- Processor receives an interrupt
- Finishes current instruction
- Stores the current address to the SP to be able to resume the previous process (just like RCALL does), then jump to the address of the ISR. The **I (Interrupt) bit of the SREG (Status Register)** is set to 0 to prevent interrupts while an ISR is being executed.
- Executes the whole ISR, which will end with a **`RETI` (Return Interrupt)**
- RETI executes; program counter returns to where it was, as the adress in the SP, and the I bit in the SREG is back to 1 (receives interrupts again).

> RETI is just RET (pops stack and puts to the PC), just with an added functionality of setting I in SREG to 1.

What does the I bit represent? We'll get to that in a moment.

### Interrupt Priority

If two interrupts occur at the same time, the higher priority one is served first.

The priority of each interrupt is related to the address of the interrupt in the interrupt vector. The lower the address, and in tandem, the lower the vector number, the higher the priority.

#### [I bit in SREG?](https://stackoverflow.com/a/24128928)

In the SREG, the I bit (represents the Global Interrupt Enable bit) takes the 8th bit (D7). It can be set to 1 with `SEI`, and 0 with `CLI`.

The I bit being 1 means interrupts are handled, and 0 means no interrupts are handled. Does that mean there can be no interrupts occuring during an interrupt?

Answer: **In AVR, yes!** But it varies from ISA to ISA.

**Interrupt nesting** is something that may happen if an interrupt is called while another is being handled. By default, AVR doesn't enable this. 

> An AVR ISR may manually re-enable the I bit with SEI.

Others? Larger, more complex systems usually enable interrupt nesting, since problems will occur when several interrupt signals occur at once (e.g. multiple devices interrupt at once). However, once an ISR is being executed, only higher priority calls can interrupt this ISR's execution (?).

> See also: **Priority Interrupt Controller Chips (PIC's)**

### Enabling Interrupts in Practice

Of course, you need to set the I flag to 1 in order to start receiving interrupts. You can use SEI.

But before that, you should first configure interrupts with the following registers:
#### GICR (General Interrupt Control Register)
Controls which (external) interrupt signals are accepted/recognized on runtime.
Initial value: 0 for all bits.
- Bit 7: INT1 (External Interrupt Request 1) Enable
- Bit 6: INT0 (External Interrupt Request 0) Enable
- Bit 5: INT2 (External Interrupt Request 2) Enable
- Bit 1: IVSEL (Interrupt Vector Select) 
- 0 -> Interrupt vectors are placed at the start of flash memory.
- 1 -> Interrupt vectors are moved to the beginning of the Boot Loader.
- Bit 0: IVCE (Interrupt Vector Change Enable) (let's ignore this for now...)
  
> **INT1, INT0, INT2?** They're the name of the external interrupt signals. Each have its own specific physical pin; if a signal is transmitted to that pin, its corresponding interrupt signal will be triggered. That means, to enable external interrupts, a device must be hooked to that pin.
> On ATmega8515: INT0 is linked to physical pin PD2 (which is Port D's pin number 2), INT1 to PD3, INT2 to PB2.
 
#### MCUCR (MCU Control Register)
MCU: Marvel Cinematic University (Microcontroller Unit)
This register is used to configure the behavior of the microcontroller itself. However, we will only discuss the bits relevant to interrupts.
Bits 0-3 are used to configure INT0 and INT1's behaviors. Below are the definitions:
``` Thanks, GPT!
INT0 (ISC01 ISC00)
===================
00 -> Low level
01 -> Any logical change
10 -> Falling edge
11 -> Rising edge


INT1 (ISC11 ISC10)
===================
00 -> Low level
01 -> Any logical change
10 -> Falling edge
11 -> Rising edge
```

> INT2 doesn't have its own MCUCR bits. Instead, its configs are in the **MCUCSR (MCU Control AND Status Register)**. Only bit ISC2 exists, and it can only control the following: ISC2=0 -> falling edge trigger, ISC2=1 -> rising edge trigger.
> If the PPT doesn't mention this, it shouldn't be important. 

#### TiMsk (Timer/Counter Interrupt Mask Register)
For internal interrupts.
TOIEx: Triggers when Timer/Counter overflows
OCIEx: Triggers when comparing output results in 0; needs a helper register you specify yourself.
TICIEx: When an Input is detected in a specified pin, the current value of the counter is captured into specified register.

> **(!)** Each register's contents may vary in content based on the processor. Above is the definitions for the ATmega8515. For example, the ATmega8A's GICR only has bit 7 as INT1 Enable and bit 6 as INT0 Enable, therefore only having two possible external interrupt source pins.

### Example Code
By GPT.
```AVR
.include "m8515def.inc"

; ==========================================
; INTERRUPT VECTOR TABLE
; ==========================================

.org 0x0000
    rjmp RESET

.org INT0addr
    rjmp INT0_ISR


; ==========================================
; RESET ENTRY
; ==========================================

RESET:

; ------------------------------------------
; 1. Initialize Stack Pointer
; ------------------------------------------

    ldi r16, high(RAMEND)
    out SPH, r16

    ldi r16, low(RAMEND)
    out SPL, r16

; Why?
; Interrupts use the stack automatically.
; Without a valid SP, RETI will fail.


; ------------------------------------------
; 2. Configure LED Pin
; ------------------------------------------

    sbi DDRB, 0      ; PB0 as output


; ------------------------------------------
; 3. Configure INT0 Trigger Type in MCUCR
; ------------------------------------------

; ISC01 ISC00 = 10
; Falling edge trigger

    ldi r16, (1<<ISC01)
    out MCUCR, r16


; ------------------------------------------
; 4. Enable INT0 in GICR
; ------------------------------------------

    sbi GICR, INT0

; ------------------------------------------
; 5. Set Pin to Receive Inputs (PD2)
; ------------------------------------------

	cbi DDRD, PD2

; ------------------------------------------
; 6. Enable Global Interrupts
; ------------------------------------------

    sei


; ==========================================
; MAIN LOOP
; ==========================================

MAIN:
    rjmp MAIN


; ==========================================
; ISR
; ==========================================

INT0_ISR:

; Toggle LED

    in  r16, PORTB
    ldi r17, (1<<PB0)
    eor r16, r17
    out PORTB, r16

    reti
```

Turns out, you only need to 
- Set up Stack Pointer
- Configure (GICR to enable which (or change vector table position to bootloader, apparently), MCUCR for interrupt behavior)
- DDR of PDx to take inputs (0)
- SEI!
- Done.
You just need to hook the signal transmitter device to PD2 for INT0 signal input.
#### Collection of new instructions
Operations on I/O Registers
- IN
- OUT
- SBI (Set Bit in I/O Register). `SBI A, b` where A is the register and b is the bit number
- CBI (Clear Bit)