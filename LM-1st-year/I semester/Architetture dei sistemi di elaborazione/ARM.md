Lavoriamo in modalità **bare metal**, ovvero senza sistema operativo, senza system call, senza scheduler ecc...
# Architettura 

![[Pasted image 20231114111902.png]]

Nell'unità di controllo si trova l'unità di **decode**. L'architettura dell'ARM in questione è ti tipo Harvard dove codice e dati sono in memorie separate.

Gli indirizzi sono su **32 bit**, di cui per un'operazione ad esempio di ADD abbiamo: 5 bit di OPcode, 4 bit indirizzare un registro, 4 bit per l'altro registro e 19 bit restanti per gli immediati.

| Reg-reg | Reg-imm | Load/store |
| ------- | ------- | ---------- |
| ![[Pasted image 20231120101511.png]]        | ![[Pasted image 20231120102016.png]]        |  ![[Pasted image 20231120102030.png]]          |

### Operazioni reg-reg

- Dall'address register è possibile incrementare il PC ( che in ARM è un general purpose register).
- I due operandi sono letti dalle sorgenti Rn ed Rm
- La ALU elabora gli operandi
- Il risultato è scritto in Rd
- Una nuova istruzione è fetchata e si riprende

### Operazioni reg-imm

- In questo caso uno dei due operandi arriva direttamente dall'istruzione

### Load/store

- Ci mette due clock cycle per completare una di queste due operazioni, in particolare uno per la preparazione (effective address), e uno per scrivere/leggere dalla memoria.

### Branch 

- Si usa il barrel shifter per fare shift di 2 posizioni, il valore si aggiunge al PC
- Poi la pipeline si svuota

Nel caso di un jump and link occorre un ciclo di clock extra per salvare l'indirizzo di ritorno all'interno di R14.

# Cortex-M3

![[Pasted image 20231120104157.png]]

La pipeline del M3 ha solamente 3 stadi: fetch, decode, execute.

![[Pasted image 20231120104207.png]]

La pipeline in caso di branch impiega 3 colpi di clock per completare l'istruzione.
Le load (come detto prima) impiegano due colpi di clock, così come le store.

Dal punto di vista del programmatore il CORTEX M3 ha:
- **18 registri da 32 bit ciascuno**
- **Gestione delle eccezioni** efficiente
- **Gestione della potenza**
- **Gestione debug**
- **Forte support per OS**, quindi il processore può funzionare in modalità User o supervisor

I registri del CORTEX vanno da r0 a r12 per usi general purpose, poi abbiamo altri 3 registri (che in teoria sono general purpose), ma in realtà:
- R13 serve per lo **stack pointer**
- R14 serve per il **link**
- R15 serve per il **Program counter**
Esiste un ulteriore registro, detto **master stack pointer** (MSP) utile per l'integrazione con il sistema operativo.

### Program status register

![[Pasted image 20231120105814.png]]

Diviso in 3 parti: **Application program status register** (APSR), **Execution program status register** (EPSR) e **Interrupt Program status register** (IPSR).

I primi 5 bit servono per dare informazioni riguardo alla ALU (Negative, Zero, oVerflow ..., Q -> **sticky** flag)

Il **T bit** specifica se il processore deve interpretare le istruzioni come se fosse prese dal Thumb Instruction set o dal normale instruction set della ARM. Thumb ha meno istruzioni, è meno potente e le istruzioni sono su 16 bit. (esiste anche un superset di Thumb, ovvero Thumb2).

### Operating modes

Due diverse modalità: **thread mode** e **handler mode**
Due livelli di accesso: **user level** e **privileged level**.
La modalità handler è sempre privilegiata.

### AMBA bus

Utilizzeremo un tipo di bus detto **AMBA**: comprende un **Advanced High Performance Bus (AHB)**, un **Advanced system bus (ASB)** e un **Advanced peripheral bus (APB)**.

![[Pasted image 20231120111047.png]]

### Clock

Spesso necessitiamo di un clock ad alta frequenza per connettere le componenti ad alte prestazioni, mentre per moduli di I/O serve avere un clock a frequenza minore.
Il clock è generato da un phase lock loop, dopodichè è presente un divisore per portare al sistema il **PCLK** (peripheral clock), e un collegamento diretto per portare il **CCLK** (CPU clock).

![[Pasted image 20231120111545.png]]

### Power management

Ci sono diverse modalità di sleep (**idle**): 
- sleep now, basati su interrupt o azioni utente
- sleep on exit, automatico 
- deep sleep, dove si fermano anche memorie, debugger, ecc...
Nella pratica si ferma il CLK per congelare il sistema.
Per il risveglio abbiamo moduli esterni detti **Wake-up interrupt controller** (WIC).

# Conditional fields

![[Pasted image 20231120123042.png]]

Per usare questi flag basta apporre all'istruzione le due lettere della condizione:

```asm
ADDEQ r0,r1,r2 //solo se il flag Z è settato allora si esegue l'operazione
```

I flag non vengono aggiornati da tutte le operazioni ma solamente per quelle che terminano con una S aggiuntiva:

```
ADDS r0,r1,r2 //al termine si aggiornano i flag di sistema
```

Le istruzioni che modificano i flag sono:
- Le operazioni con la S finale
- Le istruzione di comparazione come CMP, TST
- Le scritture dirette nel registro 

# Instruction set

Istruzioni di comparazione:

```
CMP r0, #12 
CMN r0, #10
```
La CMP (compare positive) fra la **sottrazione** fra i due operandi e poi va ad aggiornare i flag.
La CMN (compare negative) **somma** gli operandi e aggiorna i flag.

**TST e TEQ** testano rispettivamente AND logico e EOR logico, andando poi ad aggiornare i flag eccetto V.

Poi sono presenti le solite ADD, SUB, ADC (add con carry), per le altre vedere slide.

# Direttive

Clausole fornite all'assemblatore per permettere la programmazione in maniera più efficiente.
Normalmente una linea di codice ha il seguente formato: 

```
{label} {operation} {;comment}
```

In particolare le operazioni possono essere sia istruzioni (o pseudo) che **direttive**.

| Direttiva     | Uso                                                              |
| ------------- | ---------------------------------------------------------------- |
| AREA          | Definisce un blocco di codice o di dati                          |
| RN            | Rename, permette di associare un registro a un nome              |
| SPACE         | Usata per inizializzare zone di memoria                          |
| ALIGN         | Forza gli allineamenti delle aree di memoria                     |
| ENTRY         | Dichiara un punto di ingresso al programma                       |
| END           | Specifica dove finisce il file sorgente                          |
| EQU           | Definisce una costante associata a un nome                       |
| DCB, DCW, DCD | 'Define constant' byte word o double, usate per allocare memoria |
| LTORG         | Usati per gestire la memorizzazione di immediati di grandi dimensioni                                                                 |


### Area 

Bisogna specificare all'IDE come gestire le diverse parti del programma: codice, dati e altri blocchi.
Per la parte di codice occorre che contenga la direttiva AREA nelle prime linee di codice:

```
AREA sectionName {,attr} {,attr}
```

Nel caso di compilatore C si utilizza: `|.text|` 
Alcuni tipi di aree che si usano di solito sono:
- CODE: contiene il codice macchina
- DATA: contiene i dati
- READONLY: contiene dati che si allocano solamente in una read-only memory
- READWRITE: equivalente a sopra, allocata in una memoria read-write
- ALIGN: allinea la sezione a multipli di $2^{expr}$ byte

### RN

L'uso di **Rename** è il seguente:

```
name RN registerIndex
```

Ad esempio posso scrivere `coeff1 RN r7`: questo porta a una maggiore leggibilità del codice.

### EQU

La direttiva di **EQU** ha lo stesso formato della rename, dove al posto del registro c'è un valore numero: `name EQU number`. 
E' possibile esprimere i numeri in 3 metodi differnti:
- Decimale (base omessa)
- Esadecimale (0x...)
- Qualsiasi altra base (n_...) dove n è la base e ... il numero in quella base

### DC

In questo caso per **dichiarare delle costanti** possono usare il seguente formato:

```
{label} DCxx expr{,expr}
```

Al posto di xx si decide la quantità di memoria allocata:
- DCB, dichiaro un byte
- DCW, dichiaro una half word
- DCWU, dichiaro una half word unaligned
- DCD, dichiaro una word
- DCDU, dichiaro una word unaligned
expr è una espressione numera nel range deciso, oppure una stringa (solo nel DCB)

![[Pasted image 20231121092311.png]]

Nell'esempio sopra vediamo che nella dichiarazione di half word saltiamo il byte 5, in quanto l'allineamento naturale di questa memoria è multiplo di 2. Inoltre le zone di memoria più "basse" corrispondono ai bit meno significativi.

### Align

Questa direttiva permette di allineare la posizione corrente con l'indirizzo nella forma $n \cdot \text{expr} + \text{offset}$, ovvero allineata a multipli di expr più un offset.

```
ALIGN {expr {, offset}}
```

![[Pasted image 20231121093311.png]]

### Space

Simile al MIPS, specifica il numero di byte settati a 0 da allocare nella memoria:

```
{label} SPACE expr
```

Dove $\text{expr}$ è il numero di byte da riservare

# Branch

Per i salti **unconditional** ci sono 4 possibili istruzioni possibili:
- Branch , `B label`
- Branch indirect, `BX Rn`
- Branch and link, `BL label`
- Branch and link indirect, `BLX Rn`

Un programma stand alone non deve continuare all'infinito, perchè il programma avrebbe esiti non predicibili: dobbiamo perciò inserire un loop infinito su cui si blocca (dopo sostituiremo con idle): 

```
stop B stop
```

Le istruzioni `B` ha 24 bit di immediato: possiamo saltare a una distanza massima di circa $\pm 16$MB, nel caso invece di `BX` abbiamo la possibilità di saltare di 32 bit ovvero di 4 GB.

Possiamo modificare direttamente il PC per simulare un salto incondizionato:

```
LDR Rd, =label
MOV PC, Rd

#oppure

LDR PC, =label
```

In ogni caso usare una MOV per fare un salto incondizionato è sconsigliato (si genera un warning).

Per realizzare **salti conditional** posso usare i flag da apporre alle istruzioni di branch:

![[Pasted image 20231121112020.png]]

# Subroutine

Per chiamare una **subroutine** usiamo l'istruzione (una delle due):

```
BL <label>

BLX <Rn>
```

Nel **Link register** è contenuto l'indirizzo di ritorno, quindi per usare le funzione rientranti devo, alla fine della procedura, rimettere nel PC il contenuto di LR : `MOV PC, LR`

Se ho più chiamate a funzioni nested, all'interno di LR rimane solo l'indirizzo di ritorno dall'ultima funzione chiamata: per ovviare a questo dobbiamo salvare preventivamente LR all'interno dello **stack**.
Ovviamente nello stack possiamo anche salvare tutti i registri di cui abbiamo bisogno.

Supponiamo quindi avere una funzione dove usiamo r2,r4,r5:

```
sub PROC
	PUSH {r2,r4,r5,LR}
	...corpo della procedura
	POP {r2,r4,r5, PC}
```

# Supervisor Calls (SVC)

Nell'architettura ARM è possibile usare l'istruzione assembly `SVC` per **sollevare un interrupt** software.
Ogni eccezione ha tre stati possibili: Inattiva, Attiva, Pendente.

Quando un eccezione è processata, vengono salvati in una porzione dello stack (**stack frame**) i registri r0-r3,r12,LR,PC,xPSR.

Per usare l'istruzione possiamo usare la seguente sintassi:

```
{label} SVC immediate
```

Dove l'immediato è un valore su 8 bit (0-255).

![[Pasted image 20231128100942.png]]

