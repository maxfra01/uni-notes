# Introduzione

Per ridurre ulteriormente gli effetti delle dipendenze dei dati possiamo cercare di ottenere un **CPI minore di 1**: per fare ciò occorre fare **issue di più istruzioni per ciclo di clock**.
I processori in grado di fare questa operazione sono i **superscalari** e i **VLIW** (Very Long Instruction Word)

# Multiple issue - Static scheduling

Nel Superscalar MIPS è possibile fare issue di due operazioni se e solo se una operazione è una operazione **ALU**, mentre l'altra deve essere **FP** (non load e store, che sono ALU per il calcolo dell'indirizzo).
Dobbiamo necessariamente duplicare l'unità di fetch e di decode: le due istruzioni saranno allineate per costruire il **issue packet**.

```ad-note
title: Ordine dell'issue packet
Nei vecchi processori superscalari c'era il vincolo di avere la ALU operation come prima. AL giorno d'oggi non ci sono limitazioni per l'ordine.
```

Situazione ideale:

![[Pasted image 20231113114247.png]]

Nel caso di due istruzioni (che si vogliono eseguire insieme) che appartengono a due blocchi in cache differenti, i processori spesso scelgono di fetchare solamente una istruzione.
In altri casi è possibile che il packet contenga solamente un'istruzione di branch.

E' possibile che fra due istruzione del packet si verifichi un **RAW**.

Normalmente nel MIPS la latenza di una load è di un CLK, quindi non può essere usata nello stesso colpo di clock: nei superscalari la load delay slot ( e il branch delay slot ) sono a pari a 3 istruzioni.

# Multiple Issue - Dynamic scheduling

Si può ottenere con un approccio simile a Tomasulo.
Per rendere l'implementazione più semplice le istruzione non vengono mai mandate in issue out-of-order.

E' possibile **duplicare il Common Data Bus** per migliorare le performance, andando però ad aumentare la dimensione dell'architettura.

[[Speculation]]