# Collegamenti seriali sincroni
---
Distinguiamo inizialmente due modi di interconnettere moduli: in parallelo o seriale:
| Parallelo                            | Seriale                              |
| ------------------------------------ | ------------------------------------ |
| ![[Pasted image 20230502180107.png]] | ![[Pasted image 20230502180124.png]] |

Non è sempre detto che il parallelismo porti a maggiori velocità (causa skew) dunque si va su connessioni seriali (meno cavi, costi minori).
Ad esempio SATA sta per Serial ATA, PCI express è seriale.

### Struttura del collegamento seriale

![[Pasted image 20230502180300.png]]

In ognuno delle interfacce abbiamo dei componenti che serializzano i dati (ad esempio registri PISO) mentre nella ricezione abbiamo ad esempio registri SIPO.
Abbiamo inoltre bisogno di mandare il clock dalla sorgente alla destinazione.
E' anche possibile usare lo stesso bus per mandare dati e clock.
| Pro                                            | Contro                           |
| ---------------------------------------------- | -------------------------------- |
| Pochi conduttori, dunque costo ridotto         | Grande latenza                   |
| Routing semplice, unico                        | Sincronizzazione a livello ciclo |
| Riduzione dei consumi (unico driver)           | Serve controllo di flusso                                 |
| Migliore per grandi distanze e grandi velocità |                                  |


### Bit o simboli

Il segnale trasmesso è una sequenza di simboli, dove ogni simbolo **rappresenta uno o più bit**.
Parametri tipici sono ad esempio Bit Rate (bit/s) , Baud Rate (simboli/s), Efficienza (bit/simboli).
E' poi possibile codificare usante **RZ** o **NRZ**.


### Struttura base

![[Pasted image 20230502181256.png]]

E' importante che i due clock siano sincronizzati, nel senso che devono avere **stessa frequenza** e una relazione fra le fasi note.
E' dunque necessaria una **fase di sincronizzazione** del clock, e se la garantiamo possiamo procedere al trasferimento.

E' possibile calcolare il **tempo di bit** $T_{BIT}$come la differenza fra due fronti di salita del clock al trasmettitore.
Possiamo dunque, considerando di trasmettere $n$ bit, calcolare la **latenza** come:
$$
\textrm{Latenza} = n \cdot T_{BIT} 
$$

Supponiamo che il clock tx e rx vadano periodicamente risincronizzati, e voglio effettuare questa operazione prima di una trasmissione.
Per fare questa operazione devo analizzare un diagramma **ad occhio**:

![[Pasted image 20230502182441.png]]

Qui è possibile osservare tutti segnali che vedo sul collegamento seriale.
Occorre osservare se all'interno dell' "occhio" c'è abbastanza spazio da essere alta (sull'asse y) abbastanza per contenere il rumore massimo e minimo.
L'apertura (sull'asse x) la consideriamo togliendo a destra e sinistra il tempo di setup e di hold.
La porzione di occhio che ci resta è la massima variazione del clock che può fare in quel punto $\Delta t_{CK}$ che assicura il corretto trasferimento delle operazioni.

La **risincronizzazione** è legata a $\frac{t_{CK}}{\Delta t_{CK}}$


### Modem sincrono

Se invece scegliamo di usare una multiplazione inserendo nel simbolo anche l'informazione di temporizzazione, occorre usare un componente, detto **Modem** che effettua modulazione e demodulazione.

![[Pasted image 20230502183303.png]]


### Sincronismo e asincronismo

Nei collegamenti seriali **sincroni** sono necessarie le fase di risincronizzazione.
Esistono dei tempi di bit in cui dunque non si trasmette nulla.
Nel caso di collegamenti seriali asincroni invece non è necessario.


# Collegamenti seriali asincroni
---
La trasmissione in questo caso inizia in qualsiasi momento ed è riconosciuta da un simbolo di START:

![[Pasted image 20230502183615.png]]

Il CLK del ricevitore è sincronizzato sul fronte di discesa dello START bit.
Lo STOP bit invece segnala la fine del carattere (insieme di bit).
Normalmente la linea a riposo è allo stato alto.


# UART
---
E' lo **standard Universal Asynchronous Receiver/Transmitter**.
Dal lato trasmettitore c'è il modello con registro PISO (in RX c'è il SIPO).
SI usano start bit e stop bit: la risincronizzazione è fatta in base allo start bit.

![[Pasted image 20230502184118.png]]

