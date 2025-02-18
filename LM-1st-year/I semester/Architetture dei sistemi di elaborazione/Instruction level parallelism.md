Abbiamo definito il concetto di **ILP** in [[Introduzione]]

# Approcci a ILP

- **Approccio dinamico**: si fa affidamento sull'**hardware** per implementare ILP
- **Approccio statico**: si fa affidamento sul **software** per implementare ILP

### Dynamic

Sono la **maggioranza** del mercato dei processori, con prodotti come:
-  Intel Core Series
- ARM Cortex-A9
- Athlon
-  MIPS R10000/12000
- Sun UltraSPARC III
- PowerPC 603, G3, G4
- Alpha 21264.

### Static

Usate principalmente in architetture **Embedded**, come ARM Cortex-A8. Anche Intel ha usato questo approccio in architetture Intel IA-64 e Itanium

# Tecniche per ILP

Il primo tipo di istruzioni che andremo ad ottimizzare sono **blocchi**, pezzi di codice senza branch out e branch in.

### Rescheduling

una prima tecnica di ottimizzazione è il **rescheduling**, che opera su un blocco di istruzioni andando a **modificare l'ordine di esecuzione** (facendo attenzione a eventuali dipendenze).
Considerano il seguente pezzo di codice notiamo che possiamo spostare alcune istruzioni per rimuovere stalli, senza influenzare il funzionamento del programma:

![[Pasted image 20231024092235.png]]

Ad esempio per evitare i primi due stalli posso allontanare la load di RC e la ADD corrispondente, andando a caricare ad esempio un registro successivo:

![[Pasted image 20231024092331.png]]

Abbiamo avuto un miglioramento sul blocco di 2 colpi di clock (14 contro 12).

### Loop unrolling

Consideriamo un `for` loop con ogni iterazione indipendente dalle altre come il seguente:

```C
for (i=0; i<1000; i++)
	x[i] = x[i]+ y[i];
```

In questo caso la tecnica che **loop unrolling** consiste nel diminuire il numero di esecuzioni del ciclo, andando esplicitamente ad eseguire l'istruzione nel corpo del ciclo più volte.
Ad esempio per l'esempio precedente abbiamo:

```C
for (i=0; i<1000/4; i++){
	x[i] = x[i] + y[i];
	x[i+1] = x[i+1] + y[i+1];
	x[i+2] = x[i+2] + y[i+2];
	x[i+3] = x[i+3] + y[i+3];
}
```

Usando questa tecnica la dimensione del codice aumenta, ma così facendo il numero di iterazioni è minore, inoltre andando ad aumentare il corpo del loop c'è maggior possibilità che un rescheduling elimini gli stalli.

### Single instruction stream, multiple data streams (SIMD)

Consiste nell'operare sui vettori come fossero insiemi di dati e non scalari.
E' un meccanismo ampiamente usato nelle GPU.
Considerando questo esempio:

```C
for (i=0; i<1000; i++)
	x[i] = x[i]+ y[i];
```

Si può tradurre in:
- Caricare il vettore `x` dalla memoria
- Caricare il vettore `y` dalla memoria
- Sommare i due vettori `x+y`
- Memorizzare il vettore risultato

# Dipendenze

[[Pipelining]]
Se due istruzioni sono indipendenti allora possono essere eseguite in parallelo senza stalli. In caso contrario, se sono dipendenti allora dovranno essere eseguiti sequenzialmente.
Perciò il problema di ottimizzazione si riduce al **cercare anticipatamente le dipendenze**:
- Data dependencies
- Control dependencies
- Name dependencies
Le dipendenze portano ad hazards: RAW, WAW, WAR.

### Dipendenze di dato

Un'istruzione *i* è data dipendente da *j* se una delle due condizioni sottostanti è verificata:
- *i* produce un risultato usato in *j*
- *j* è data dipendente con *k*, e *k* è data dipendente con *i*

```MIPS
Loop: L.D F0, 0(R1)
	  ADD.D F4, F0, F2
	  S.D F4, 0(R1)
```

Quando c'è una dipendenza allora è possibile che ci siano rischi (**hazards**).

### Dipendenze di memoria

Individuare dipendenza fra registri è molto semplice, ma al contrario quelle di memoria risultano più complesse: questo perchè a differenza dal codice gli accessi in memoria variano run-time.

### Dipendenze di nome

Questo tipo di dipendenza si verificano quando due istruzioni fanno riferimento allo stesso registro ma non c'è flusso di dati associato a quel registro.
Due tipi:
- **Anti-dipendenza**: *j* scrive un registro che *i* deve leggere, ma *i* viene eseguito prima.
- **Output dipendenza**: sia *i* che *j* scrivono il loro output sullo stesso registro.

```
Loop:
	L.D F0, 0(R1)        # OUT-dep
	ADD.D F4, F0, F2     # ANTI-dep
	S.D F4, 0(R1)        # ANTI-dep
	L.D F0, -8(R1)       # OUT-dep
	ADD.D F4, F0, F2
	S.D F4, -8(R1)
	L.D F0, -16(R1)
```

Questo tipo di dipendenza si risolve con la tecnica di **renaming**.

### Renaming

Si basa sul cambiare registro che causa la dipendenza di nome, e può essere fatto in modo statico o dinamico, rispettivamente dal compilatore e dal processore.
Un metodo simile ma più complicato è disponibile per risolvere lo stesso problema ma in memoria.
Nel caso statico occorre identificare prima tutte le dipendenze di nome (richiede di analizzare a fondo il codice, considerando anche tutti i salti).

### Dipendenze di controllo

Una dipendenza di controllo si verifica **quando un'istruzione dipende da un salto**.

![[Pasted image 20231024101954.png]]

Un'istruzione controllo dipendente da un branch non può essere spostata prima del branch.