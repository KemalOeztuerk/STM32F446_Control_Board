# STM32F446_Control_Board

A 2-layer control board built around the **STM32F446RET6**, intended for UAV and small robotics applications. Designed in KiCad.

The board provides a CAN interface, a hardware-inverted SBUS receiver input, PWM outputs for actuators, quadrature encoder inputs for closed-loop feedback, analog inputs, and standard debug/expansion headers.

---

## Features

| Block | Details |
|---|---|
| MCU | STM32F446RET6 — Cortex-M4F @ 180 MHz, 512 KB Flash, 128 KB SRAM, LQFP64 |
| CAN | SN65HVD230 transceiver, 3.3 V, with bus termination option |
| SBUS input | Hardware inverter on the receiver line — no software inversion or timer tricks needed |
| PWM outputs | 4x, timer-driven (servo / ESC compatible) |
| Encoders | 2x quadrature channels on hardware timer encoder mode |
| ADC | 4x analog inputs |
| Headers | SPI, UART, SWD |
| Layers | 2 |

## Repository layout

```
f44ret6.kicad_pro     KiCad project
f44ret6.kicad_sch     Schematic
f44ret6.kicad_pcb     PCB layout
sch.pdf               Schematic export (PDF)
```

Open `f44ret6.kicad_pro` in **KiCad 7 or newer**.

## Hardware notes

### Power

The board is powered from a single supply rail feeding a 3.3 V regulator. Check the schematic for the accepted input range before connecting a battery directly.

### SBUS

SBUS is an inverted UART running at 100000 baud, 8E2. The inversion is handled in hardware on this board, so the receiver line can be connected straight to a standard USART RX pin. In firmware, configure the USART as:

- Baud rate: `100000`
- Word length: 9 bits (8 data + parity)
- Parity: even
- Stop bits: 2

### CAN

The SN65HVD230 is a 3.3 V transceiver with a slope-control pin. Termination (120 Ω) should only be populated on the two physical ends of the bus.
