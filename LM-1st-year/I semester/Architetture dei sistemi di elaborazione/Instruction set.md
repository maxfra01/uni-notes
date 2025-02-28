# Instruction set Architecture

L'**ISA** è il modo in cui il sistema è visto da un compilatore o da un programmatore.
L'architettura è scelta in modo da bilanciare vari parametri come complessità, performance, potenza ecc...

I processori sono classificati in base alla loro **memoria interna**:
- **Stack**
- **Accumulatore**
- **Registri**, suddivisi in register-memory e register-register
Attualmente i processori sono **general-purpose register** in quanto i registri sono molto più rapidi delle memorie e per il compilatore sono molto semplici da usare.

![[Pasted image 20231003104547.png]]

Un'altra classificazione per i processori riguarda il **numero degli operandi della ALU**: normalmente il numero degli operandi varia da 0 a 3.

![[Pasted image 20231003110032.png]]

### Memory addressing
Prima distinzione su **Little Endian** e **Big Endian**, nel primo caso (come scriviamo) il LSB è a destra, viceversa nel big endian il LSB è quello più a sinistra.
Una seconda distinzione si basa sull'accesso **allineato o meno** della memoria: l'accesso allineato consente di leggere solamente a multipli del parallelismo della memoria; nel caso disallineato questo non è vero e i processori che consentono questo sono molto più complessi.

| Modo di indirizzamento      | Esempio di codice      | Commento                                                                              |
| --------------------------- | ---------------------- | ------------------------------------------------------------------------------------- |
| Register mode               | Add R4,R3              |                                                                                       |
| Immediate mode              | Add R4, #3             |                                                                                       |
| Displacement mode           | Add R4, 100(R1)        | Tramite questa notazione saltiamo di 100 posizioni in memoria rispetto al registro R1 |
| Indirect mode               | Add R4, (R1)           | Simile al precedente ma usando i puntatori                                            |
| Indexed mode                | Add R3, (R1+R2)        | La somma di R1 ed R2 da la posizione di memoria alla quale vogliamo accedere          |
| Absolute mode               | Add R3, (1001)         | Indica esplicitamente la posizione di memoria                                         |
| Memory indirect mode        | Add, R1, @(R3)         | Equivalente a \*(\*R3)                                                                |
| (post) Autoincremented mode | Add R1, (R2)+          | A seguito dell'addizione incremento il registro R2, utile per scandire vettori        |
| Autodecrement mode          | Add R1, -(R2)          | Analogo al precedente                                                                 |
| Scaled mode                 | Add R1, 100(R2) \[R3\] | Non usati                                                                                      |

# Instruction set operations

Possiamo raggruppare le operazioni eseguite dal processore in varie categorie:

![[Pasted image 20231009100824.png]]

Quando si progetta un processore occorre fare in modo che le **operazioni che vengono eseguite di frequente siano le più efficienti e quindi veloci!**

Una particolare categoria di operazioni sono le **control flow instructions**: queste operazioni modificano il flusso di esecuzione del programma in esecuzione.
- Conditional branches
- Jumps
- Procedure calls
- Procedure returns
Queste procedure richiedono un **destination address** che spesso viene codificato come un displacement dal valore corrente del program counter: questo perchè si risparmiano bit dato che spesso la destinazione target sono vicine. 

Una tipologia particolare di jump è quello indiretto:
carico una destinazione in un registro e poi dico al processore di saltare alla posizione contenuta in quel registro.

### Branches

Il processore per effettuare branches deve valutare delle condizioni e fare una scelta.
Come fa il processore a comparare due registri?
- Una prima soluzione sono dei bit riservati nella ALU che danno informazioni sul confronto fra due registri (ARM, 80x86): il dato si salva in un registro dedicato
- Nelle architetture MIPS si specificano i due registri da confrontare e quello in cui inserire la soluzione (massima flessibilità)
- Nelle architetture PARISC una unica istruzione decide il confronto ed esegue l'azione

![[Pasted image 20231009102145.png]]

### Procedure

Quando una procedura viene chiamata, occorre salvare diverse informazioni: è necessario salvare il **return address** e i **registri a cui ho acceduto** da parte del chiamante e del chiamato.

```ad-summary
title: Riassunto
- E' necessario cercare di ottimizzare le operazioni che vengono eseguite frequentemente
- Gli offset raccomandati sono almeno di 8 bit

```

### Operands

Gli operandi sono caratterizzati da un **tipo** e una **dimensione**:
- Char 1 byte
- half word 2 bytes
- word 4 bytes
- double word 8 bytes
- single prec floating point 4 bytes
- double prec floating point 8 bytes

### Encoding

Le codifiche sono molteplici e variabili:

![[Pasted image 20231009102910.png]]

Ad esempio nell'80x86 la dimensione dell'istruzione non ha lunghezza fissa: nel primo campo specifico numero di operandi e operazione, dopo seguono n campi con gli indirizzi degli operandi.
Questa strategia è **lenta ed inefficiente**, ma è molto flessibile, minima dimensione del codice

In altre architetture come il MIPS la lunghezza di un'istruzione è **fissa**, così come il numero di operazioni (opcode). In questo caso le performance sono massime, ma con grande dimensione del codice

Esiste anche una soluzione **ibrida** che ha diversi formati: questa soluzione garantisce un trade off fra dimensione del codice e performance.

```ad-note
title: Designer trade off
Un designer deve tener conto di 3 fattori quando si progetta un processore: dimensione del **codice**, dimensione dell'**instruction set** e complessità dell'**hardware**

```

Scegliere in quale registro posizionare i dati è di grande importanza per ottimizzare le operazioni. Questo è compito del compilatore e si dimostra che sono necessari in media almeno 16 registri per lavorare in modo efficiente.

Un designer per aiutare il compilatore può:
- Rendere efficiente l'operazione più frequente
- rendere corretta quella più rara
- Facilitare i trade offs tra le alternative
- Fornire primitive per i programmi
- Fornire costanti al codice e rendere disponibili operazioni che elaborino quest'ultime.
Alcune raccomandazioni sono: avere almeno 16 registri, semplicità delle operazioni e ortogonalità delle operazioni.

[[MIPS]]
[[ARM]]