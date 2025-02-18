# Introduzione

Una importante limitazione del [[Dynamic Scheduling Technique]] è la presenza di un salto: infatti dobbiamo necessariamente aspettare che il salto sia eseguito per continuare.

Se un processore supporta il **branch prediction** nel dynamic scheduling allora le istruzioni vengono fetchate e mandate in issue come se la predizione fosse giusta.
Se un processore supporta la **speculazione hardware-based** allora quelle stesse istruzioni vengono anche **eseguite**.

Così facendo il processore implementa un **data flow execution** dove il processore esegue le operazioni non appena gli operandi sono disponibili.

# Architettura

SI utilizza l'architettura di Tomasulo, con due importanti accorgimenti:
- Dobbiamo permettere il completamento dell'operazione e il bypassing ad altre istruzioni
- Dobbiamo aggiungere un ulteriore stadio (**instruction commit**): facciamo attendere le istruzioni 'future' per il completamento di quelle precedenti: si parla quindi di **in-order commitment**.

### ROB, ReOrder Buffer

E' una speciale struttura dati che **contiene i risultati delle istruzioni di cui non è ancora stato fatto il commit**.
Normalmente con Tomasulo, i risultati sono letti dal register file, mentre in presenza di speculazioni i dati possono essere letti:
- Dal ROB se il commit di quelle istruzioni non è stato effettuato
- Dal register file, altrimenti

La struttura della ROB è equivalente a una tabella di cui **ogni entry ha il seguente formato**:
- Instruction **type**: branch, store, register
- **Destination**: contiene il registro destinazione o un indirizzo di memoria
- **Value**: Contiene il valore quando l'istruzione è completa ma non 'committata'
- **Ready**: indica quando l'istruzione è stata completata (Non completata, completata, committata)

![[Pasted image 20231113102834.png]]

# Instruction execution steps

La fase di execute è divisa in: **Issue, Execute, Write Result e Commit**.

### Issue

Durante questa fase si estrae un'istruzione dalla IQ solamente se c'è spazio all'interno di una relativa reservation station e anche spazio libero nel ReOrder Buffer.
Gli operandi sono mandati alle reservation station se si trovano nei registri o nel ReOrder Buffer.

### Execute

Non appena tutti gli operandi sono disponibili alla reservation station allor all'istruzione viene eseguita.
I risultati possono essere presi anche dal CDM, inoltre grazie a questo meccanismo si evitano i RAW.

### Commit

Il ReOrder buffer è ordinato in base al codice.
Non appena un'istruzione arriva alla testa del buffer:
- Se è un mispredicted branch tutto il ROB viene flushato
- Altrimenti il risultato è scritto in un registro o in memoria nel caso di una store
- In entrambi i casi quella entry è dichiarata libera
Questi meccanismi necessitano che il ROB sia strutturato come un **buffer circolare**.

### Eccezioni

Le eccezioni non vengono eseguite appena sono sollevate, ma entrano nel ROB.
Quando l'istruzione viene committata allora l'eccezione viene eseguita e il ROB flushato per intero.
Nel caso di un'istruzione (che genera un eccezione) flushato dal ROB, allora quell'eccezione si può ignorare.