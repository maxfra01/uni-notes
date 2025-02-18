# Ripassino MIPS

Fa parte della famiglia dei processori RISC.
Questo processore ha un'architettura basato su **registri general purpose** e **registri dedicati**:
- Il registro R0 contiene sempre il valore 0
- Una serie di registri per numeri interi
- Una serie di registri per numeri floating point
- Altri registri speciali

### Encoding

Le **istruzioni sono tutte codificate su 32 bit** e sono di 3 tipi: Immediate, Jump, Register.
Le operazioni di tipo **Immediate** hanno questo formato:

![[Pasted image 20231009111959.png]]

Quelle **register** hanno il seguente formato:

![[Pasted image 20231009112016.png]]

Quelle **Jump**:

![[Pasted image 20231009112029.png]]

### Instruction set

Di seguito sono riportate le load and store con esempi:

| Nome               | Esempio       | Note |
| ------------------ | ------------- | ---- |
| load double word   | ld R3, 28(R2) |      |
| load byte          | lb R3, R2     |      |
| load byte unsigned | lbu R3, R2    |      |
| load FP single     | l.s R3, R2    |      |
| load FP double     | l.d R3, R2    |      |
| store double       | sd R2, R3     |      |
| store word         | sw            |      |
| store half word    | sh            |      |
| store byte         | sb            |      |
| store FP single    | s.s           |      |
| store FP double    | s.d              |      |

# Esempio di programma MIPS

```MIPS
	.data
VecA: .byte 5
VecB: .byte 1
Result: .space 1

	.Code
	.global main
main: lb R1, VecA(R0)
	lb R2, VecB(R0)
	daddu R3, R2, R1
	sb R3, Result(R0)

	halt
```


