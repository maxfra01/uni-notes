# DRAM - Dynamic RAM
---

![[Pasted image 20230404161050.png]]

Il valore di 0 o 1 è immagazzinata come carica sul condensatore che si traduce in tensione alta o bassa quando vado ad effettuare la lettura.

![[Pasted image 20230404161156.png]]

Il **Sense amplifier** occorre per amplificare il valore logico di 0 o 1.

Quando si vuole leggere una cella di memoria, prima c'è il segnale di equalizzazione, dopodichè si attiva una **wordline** che attiva tutti i transistori collegati alla linea i-esima.

![[Pasted image 20230404161735.png]]

Quando la wordline viene attivata il valore della **Bitline** inizia a distorcersi,e  quando il Sense amplifier si attiva il segnale viene amplificato verso il valore logico corretto.

![[Pasted image 20230404161918.png]]

### Supercelle DRAM

Immaginiamo di implementare supercelle, che contengono una certa quantità di bit:

![[Pasted image 20230404162120.png]]

Normalmente per notazione abbiamo D supercelle con W bit ciascuna.
Per **Leggere una supercella** inviamo al controllore di memoria un indirizzo in due parti: la prima parte identifica la riga, e questa riga viene trasferita su un buffer di riga (**RAS, read address strobe**).
Successivamente c'è la fase di **CAS** dove il controllore manda l'indice per accedere alla cella corretta nel buffer.

il valore viene dunque trasferito sul bus e mandato a destinazione. 

### Segnali in una DRAM Asincrona

![[Pasted image 20230404162529.png]]

Ras low e Cas low indicano che sono attivi bassi, poi abbiamo il segnale di write enable e output enable.
Il canale A monodirezionale è usato per fornire gli indirizzi alla DRAM.
Il Bus D è bidirezionale half-duplex, perciò non posso leggere e scrivere contemporaneamente.
Notiamo l'assenza di un clock, perciò le azioni vengono intraprese a seguito della modifica di un valore di questi segnali.

### Chip e Moduli tipici

![[Pasted image 20230404163428.png]]

Considerando il parallelismo di 1 byte (8 bit) abbiamo 127 milioni di celle da 1 byte, quindi occorrono 27 bit per indirizzare appunto queste supercelle.
Di questi 27 in particolare: 2 individuano i memory field (le grandi aree in cui la memoria è suddivisa), 14 per la fase di RAS e 11 per quella di CAS.

Questa struttura è propria di un chip, essi vengono poi combinati per formare moduli di dimensione maggiore.


### Evoluzione delle DRAM

Ad oggi le memorie DRAM sono di tipo sincrono, e implementano la tecnologia Fast Page, dove si accede a dati in sequenza indicando una volta la riga e più volte la colonna (ras,cas,cas,cas...)

Sono principalmente tutte memorie di tipo sincrono e inoltre sono **DDR, double data rate** ovvero sono sensibili a entranbi i fronti di clock.
(DDR5 significa double data rate di 5a generazione).


### SDRAM - Synchronous DRAM

Le DRAM sincrone hanno la seguente struttura: al posto dei memory field abbiamo la nozione di "banchi":

![[Pasted image 20230404164411.png]]

Gli indirizzi BA0-BA1 servono per identificare a quale banco di RAM accedere, mentre A0-A11 servono per la fase di RAS e CAS.
E' presente una logica di controllo che ricevuti segnali dall'esterno ne produce di nuovi per regolare la memoria. (la logica gestisce anche le fasi di refresh per ripristinare lo stato del segnale).

Ad ogni banco di memoria è applicato un sense amplifier.

### Temporizzazione SDRAM

Un primo parametro è la **CAS latency (CL)** che corrisponde al ritardo che intercorre fra l'attivazione del READ a quando il dato è effettivamente disponibile in uscita.
E' possibile, a volte, programmare la CL in cicli di clock.

Il **Burst Lenght (BL)** è il numero di colonne alle quali si può accedere tramite un segnale di READ o WRITE.
Spesso è programmabile e pari a potenza di 2: 1,2,4...

A causa della tecnologia **DDR Double data rate**, è possibile avere CL frazionarie, in quanto l'unità di misura effettiva è il semiperiodo di clock.




# SRAM - Static RAM
---
Le RAM statiche non necessitano del Refresh, in quanto il dato non può essere perso.
La struttura di una cella elementare è detta a **6 transistor**:

![[Pasted image 20230404170439.png]]

I due inverter sono collegati in una configurazione bistabile (latch) e poi ci sono i due transistor di accesso.
Quando la WordLine WL è zero nessuno può accedere al dato nel latch, mentre quando è attiva posso o leggere o scrivere:
- Per scrivere forzo BL a 1, mentre BLc a 0, o viceversa; aspettando un tempo per stabilizzarsi avrò scritto correttamente il valore nel latch.
- Per leggere forzo le due bitline alla tensione di alimentazione Vdd: a seconda del dato memorizzato uno dei due lati si scaricherà e dunque attivando il sense amplifier avrò letto il dato.

### Struttura di una SRAM

![[Pasted image 20230404171133.png]]

Il decoder attiverà una sola riga, le cui celle saranno poi pilotate dalle due bitline. E' presente un sense amplifier per ogni colonna e un Write driver per gestire la scrittura.
In dettaglio:

![[Pasted image 20230404171339.png]]

Le due bitline sono sempre precaricate e Vdd prima di leggere o scrivere.
Per portare giù una delle due bitline (fase di scrittura) entra in gioco il writer driver che ne azzera una delle due.
L'equalizzatore ha il compito di garantire che le due tensioni siano effettivamente uguali, in modo da non sbilanciare il sense amplifier.

### Sense amplifier

![[Pasted image 20230404171821.png]]

Inizialmente le due bitline sono caricate a Vdd, ma quando effettuo una lettura una delle inizia ad abbassarsi (l'altra dovrebbe rimanere costante, ma si abbassa di poco per alcune dispersioni), a questo punto il sense amplifier porta a 0 o 1 le due bitline.

Inoltre è presente un **SE sense enable** che va attivato per il corretto funzione dell'amplifier.

### SRAM Asincrone

La struttura di una memoria SRAM asincrona è la seguente:

![[Pasted image 20230404172210.png]]

Per comandare le operazioni si usa la transizione dei segnali WRITE enable o OUTPUT enable.
Ques'utlimo segnale decide il flusso dei dati sul bus D.
Inoltre in questa configurazione i segnali sono attivi sul fronte di discesa!

```ad-info
Le SRAM in sistemi embedded sono tipicamente SINCRONE
```

### SRAM Dual-Port

E' possibile ottenere le operazioni di READ e WRITE in maniera simultanea al costo di modificare la cella (con una dual-port a 8 transistor):

![[Pasted image 20230404173356.png]]

Abbiamo in aggiunta una coppia di transitori in accesso, per appunto sdoppiare le due bitline in modo da eseguire le due operazioni.

Questo meccanismo di aggiunta di coppie di transistor è estendibile fino a un paio di ulteriori porte di accesso (ad esempio per 2 letture e una scrittura simultanea).
Non è possibile aggiungere altri transistor perchè potrebbero compromettere il dato nel latch.

Una particolare applicazione delle celle dual port sono le memorie FIFO.

### Confronto fra SRAM e DRAM
| **SRAM**                                | **DRAM**                |
| ----------------------------------- | ------------------- |
| più condensatori dunque più costosa | Meno costosa        |
| Velocità di lettura migliore        | Lettura lenta       |
| Lettura non distruttiva             | Lettura distruttiva |
| No refresh                          | Occorre Refresh     |
| Consumo statico molto basso         |                     |
| Fabbricazione standard              | Schemi di indirizzamento non banali                    |




# CAM - Content Addressable Memory
---
L'idea di una CAM è sostanzialmente il contrario di una memoria RAM: in quest'ultima dato in pasto un indirizzo otteniamo il contenuto di una cella.
Nella CAM **a seguito di un dato in input, otteniamo l'indirizzo di quest'ultimo in output**.

Uno schema concettuale di una CAM è il seguente:

![[Pasted image 20230404180420.png]]

All'interno delle celle di una CAM sono presente celle di SRAM, inoltre necessito di un meccanismo di ricerca.
L'encoder ricevuta una riga attiva su $k$, ne fa la codifica su $\log_{2}k$ bit.
In particolare si può impostare la priorità del decoder per restituire il primo dato trovato o l'ultimo.

Si parla di **Ternary CAM, TCAM** quando è possibile cercare 3 tipi di simboli, ovvero oltre 0 e 1 posso specificare il **Don't care X** che viene ignorato dalla ricerca.

Un esempio di applicazione delle TCAM è il **packet inspection and forwarding**, ovvero tramite la TCAM si cerca la porta di uscita di un pacchetto.
E' il meccanismo di forwarding che usano ad esempio i router!

Un altro esempio è nelle memerie Cache, per verificare se un dato è presente o meno in cache.

![[Pasted image 20230404181652.png]]




