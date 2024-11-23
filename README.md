# Temprature-controlled-fan

## Introduction
Our project deals with the design and implementation of a temperature-controlled fan system. The principle involved here is that the speed of the fan will vary with ambient temperature for energy efficiency. It consists of an AT89C52 microcontroller, a temperature sensor LM35, and a 16x2 LCD displaying the temperature reading. Pulse Width Modulation is used for dynamic control of the fan speed.
---

## Getting Started

### Prerequisites

Before you begin, ensure you have the following:
- **Software**:
  - Proteus: PCB Design and Circuit Simulator Software
    
- **Hardware Requirements** :
  - Specific hardware requirements – AT89C52 Microcontroller & ADC0804
  

---

### Installation

To set up and run the prototype locally:

1. **Clone the repository**:
   ```bash
    git clone https://github.com/Nishit1006/Temprature-controlled-fan.git
    cd Temprature-controlled-fan

   
1. **Code To Programme AT89C52**:
```bash
    ORG 0000H
    LJMP MAIN

    ORG 000BH
    LJMP INTERRUPT

    ORG 0030H
    MAIN:	
    MOV P0, #0FFH
    SETB P1.5
    MOV A,#38H
    ACALL COMMAND
    ACALL DELAY
    MOV A,#0EH
    ACALL COMMAND
    ACALL DELAY
    MOV A,#01H
    ACALL COMMAND
    ACALL DELAY
    MOV A,#080H
    ACALL COMMAND
    ACALL DELAY
    LJMP AGAIN1

    COMMAND:
    MOV P2,A
    CLR P3.1
    CLR P3.0
    SETB P3.2
    ACALL DELAY
    CLR P3.2
    RET

    DELAY:
    MOV R3,#0FFH
    AGAIN : DJNZ R3,AGAIN
    RET

    AGAIN1:
    MOV A,#' '
    ACALL LCDWRITE
    ACALL DELAY
    MOV A,#'M'
    ACALL LCDWRITE
    ACALL DELAY
    MOV A,#'I'
    ACALL LCDWRITE
    ACALL DELAY
    MOV A,#'C'
    ACALL LCDWRITE
    ACALL DELAY
    MOV A,#'R'
    ACALL LCDWRITE
    ACALL DELAY
    MOV A,#'O'
    ACALL LCDWRITE
    ACALL DELAY
    MOV A,#'C'
    ACALL LCDWRITE
    ACALL DELAY
    MOV A,#'O'
    ACALL LCDWRITE
    ACALL DELAY
    MOV A,#'N'
    ACALL LCDWRITE
    ACALL DELAY
    MOV A,#'T'
    ACALL LCDWRITE
    ACALL DELAY
    MOV A,#'R'
    ACALL LCDWRITE
    ACALL DELAY
    MOV A,#'O'
    ACALL LCDWRITE
    ACALL DELAY
    MOV A,#'L'
    ACALL LCDWRITE
    ACALL DELAY
    MOV A,#'L'
    ACALL LCDWRITE
    ACALL DELAY
    MOV A,#'E'
    ACALL LCDWRITE
    ACALL DELAY
    MOV A,#'R'
    ACALL LCDWRITE
    ACALL DELAY
    MOV A,#0C1H
    ACALL COMMAND
    ACALL DELAY
    MOV A,#'P'
    ACALL LCDWRITE
    ACALL DELAY
    MOV A,#'R'
    ACALL LCDWRITE
    ACALL DELAY
    MOV A,#'O'
    ACALL LCDWRITE
    ACALL DELAY
    MOV A,#'J'
    ACALL LCDWRITE
    ACALL DELAY
    MOV A,#'E'
    ACALL LCDWRITE
    ACALL DELAY
    MOV A,#'C'
    ACALL LCDWRITE
    ACALL DELAY
    MOV A,#'T'
    ACALL LCDWRITE
    ACALL DELAY
    ACALL DELAY1
    MOV A,#01H
    ACALL COMMAND
    ACALL DELAY
    MOV A,#' '
    ACALL LCDWRITE
    ACALL DELAY
    MOV A,#'T'
    ACALL LCDWRITE
    ACALL DELAY
    MOV A,#'E'
    ACALL LCDWRITE
    ACALL DELAY
    MOV A,#'M'
    ACALL LCDWRITE
    ACALL DELAY
    MOV A,#'P'
    ACALL LCDWRITE
    ACALL DELAY
    MOV A,#' '
    ACALL LCDWRITE
    ACALL DELAY
    MOV A,#'C'
    ACALL LCDWRITE
    ACALL DELAY
    MOV A,#'O'
    ACALL LCDWRITE
    ACALL DELAY
    MOV A,#'N'
    ACALL LCDWRITE
    ACALL DELAY
    MOV A,#'T'
    ACALL LCDWRITE
    ACALL DELAY
    MOV A,#'R'
    ACALL LCDWRITE
    ACALL DELAY
    MOV A,#'O'
    ACALL LCDWRITE
    ACALL DELAY
    MOV A,#'L'
    ACALL LCDWRITE
    ACALL DELAY
    MOV A,#' '
    ACALL LCDWRITE
    ACALL DELAY
    MOV A,#'F'
    ACALL LCDWRITE
    ACALL DELAY
    MOV A,#'A'
    ACALL LCDWRITE
    ACALL DELAY
    MOV A,#'N'
    ACALL LCDWRITE
    ACALL DELAY
    LJMP AGAIN2

    LCDWRITE:
    MOV P2,A
    SETB P3.1
    CLR P3.0
    SETB P3.2
    ACALL DELAY
    CLR P3.2
    RET

    DELAY1:                
    MOV R3,#0FFH
    HERE1: MOV R5,#0FFH
    HERE2: MOV 75H,#02FH
    HERE3: DJNZ 75H,HERE3
    HERE4: DJNZ R5,HERE2
    DJNZ R3,HERE1
    RET

    AGAIN2:
    SETB P3.5
    SETB P3.3
    CLR P3.4
    SETB P3.4
    HERE5: JB P3.5,HERE5
    CLR P3.3
    MOV A,#0C1H
    ACALL COMMAND
    ACALL DELAY
    MOV TMOD,#02H
    MOV IE,#82H
    MOV R1,P0
    MOV A,R1
    MOV R4,A
    ACALL COMPARE
    MOV A,R4
    LCALL CONVERSION
    LCALL LCDWRITETMP
    ACALL DELAY1
    LJMP MAIN

    COMPARE:
    CLR C
    CJNE R1,#35,GAIN
    GAIN: JNC GAIN1
    CLR C
    CJNE R1,#25,GAIN2
    GAIN2: JNC GAIN3
    CLR TR0
    LJMP GAIN4                              
    GAIN1: ACALL GREATER
    LJMP GAIN4
    GAIN3: ACALL LOWER
    GAIN4: CLR C
    RET

    GREATER:  
    CLR TR0
    MOV R2,#0AAH
    MOV TH0,#0FFH
    SETB TR0
    RET

    LOWER:               
    CLR TR0
    MOV R2,#0AAH
    MOV TH0,#1FH
    SETB TR0
    RET

    CONVERSION:
    MOV B,#10
    DIV AB
    MOV R7,B
    MOV B,#10
    DIV AB
    MOV R6,B
    MOV A,R6
    ADD A,#30H
    MOV R6,A
    MOV A,R7
    ADD A,#30H
    MOV R7,A
    RET

    LCDWRITETMP:
    MOV A,R6
    ACALL LCDWRITE
    ACALL DELAY
    MOV A,R7
    ACALL LCDWRITE
    ACALL DELAY
    MOV A,#'C'
    ACALL LCDWRITE
    ACALL DELAY
    RET

    INTERRUPT:
    CPL P1.5
    CLR TR0
    MOV 76H,R2
    HERER: DJNZ 76H,HERER
    SETB TR0
    CPL P1.5
    RETI     

    END
