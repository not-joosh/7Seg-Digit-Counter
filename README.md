# 7Seg-Digit-Counter
7SEG Counter | Interfaced with 8086 Microprocessor, 8086 Assembly, 8253 PIT, 8252 PPI, Proteus Simulation
## Description
This assembly code project simulates a 7-Segment digit counter from 0 to 9. It uses the 8253 (Mode 0 & 1 second delay) with the PPI 8252. 

### Proteus Simulation (Hardware with Software)

![Ratificar_SingleDigit7SegCounter](https://github.com/not-joosh/7Seg-Digit-Counter/assets/105687297/24395a56-12f8-412c-ae0a-8c6c17f4419c)


### 8086 Assembly Software Script 

```assembly
;====================================================================
; Main.asm file generated by New Project wizard
;
; Created:   Sun Nov 8 2023
; Processor: 8086
; Compiler:  MASM32
; AUTHOR:    JOSH BOBIER RATIFICAR
;====================================================================
DATA SEGMENT   
    ; 8255
    PORTA EQU 0C0H  		; PORTA ADDRESS                                       
    PORTB EQU 0C2H  		; PORTB ADDRESS                                     
    PORTC EQU 0C4H  		; PORTC ADDRESS    
    COMREG EQU 0C6H  		; COMMAND REGISTER   
    
    ; 8253
    CLOCK0 EQU 0C8H 		; COUNTER
    T_COMREG EQU 0CEH		; COUNTER COMMAND REGISTER	     
    
    ; VARIABLES	
    NUM DB 3FH, 06H, 5BH, 4FH, 66H, 6DH, 7DH, 07H, 7FH, 6FH 
    DIGIT DB 0d                                                                  
DATA ENDS
CODE SEGMENT PUBLIC 'CODE'
    ASSUME CS:CODE, DS:DATA

START:
    INIT:
    ; TURNING OFF LED
    MOV DX, PORTA
    MOV AL, 00000000B
    OUT DX, AL
    ;SETUP THE 8253
    XOR AL, AL
    MOV AL, 00111000B  ; CWORD
    OUT T_COMREG, AL         
    CALL CLOCK_RESET
    
    ;SETUP THE 8255 
    XOR AL, AL
    MOV AL, 10001001B ; COMMAND BYTE A 
    OUT COMREG, AL
    
    MAIN:        
        ; DISPLAYING NUMBER 
        CALL DISPLAY_NUM   
	
	; CHECK IF DONE COUNTING, IF SO, INCREMENT COUNTER
        MOV DX, PORTC
        IN AL, DX
        CMP AL, 0
        JE TRY_INC
        JMP NEXT_PHASE
        TRY_INC:                            
            CALL NUM_INC  
        NEXT_PHASE:
    JMP MAIN 
DISPLAY_NUM PROC 
    XOR AX, AX   
    LEA SI, NUM  
    MOV DX, PORTA
    MOV AL, [DIGIT]   
    ADD SI, AX
    MOV AL, [SI]
    OUT DX, AL   
    XOR AX, AX
    RET
DISPLAY_NUM ENDP
NUM_INC PROC            
    INC [DIGIT]
    MOV AL, [DIGIT]
    CMP AL, 10d    
    JE RESET_COUNTER  
    JMP END_INC
    RESET_COUNTER:
    MOV [DIGIT], 0d
    END_INC:
	CALL CLOCK_RESET
        RET
NUM_INC ENDP    

CLOCK_RESET PROC
    XOR AX, AX
    MOV DX, CLOCK0
    MOV AL, 0D0H
    OUT DX, AL   
    MOV AL, 07H
    OUT DX, AL
    RET
CLOCK_RESET ENDP 
CODE ENDS
END START

```
---

### USEFUL 8255 NOTES:
<img width="577" alt="8255_1" src="https://github.com/not-joosh/7Seg-Digit-Counter/assets/105687297/a767581f-4618-4153-aa5c-455fcdb0dd16">
<img width="577" alt="8255_2" src="https://github.com/not-joosh/7Seg-Digit-Counter/assets/105687297/010f02c6-1b47-4ede-9795-40116b9c273e">
<img width="577" alt="8255_3" src="https://github.com/not-joosh/7Seg-Digit-Counter/assets/105687297/02ffec56-d3c3-43d2-8d28-15fcdd9837bc">

---

### USEFUL 8253 NOTES:
<img width="430" alt="8253" src="https://github.com/not-joosh/7Seg-Digit-Counter/assets/105687297/1b81c3a2-8c08-4cc2-81b3-1a7d3bf014e4">

--- 
### References 
- (8255 Screenshots)
  - Univeristy of San Carlos
- (8253 Notes)
  - https://datasheetspdf.com/pdf-file/45347/IntelCorporation/8253/1 
  - https://www.slideserve.com/woodse/8253-54-timer-and-music-powerpoint-ppt-presentation
      
