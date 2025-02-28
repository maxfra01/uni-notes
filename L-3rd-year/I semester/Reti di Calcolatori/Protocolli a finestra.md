# Definizioni 
---
Sono protocolli che compaiono in quasi tutte le reti telematiche, in ambito della [[Trasmissione nelle reti]], hanno lo scopo di:
- Recuperare gli errori di trasmissione
- controllo del flusso di trasmissione
- controllo di sequenza

I bit di parità fanno parte dell'intestazione della PDU, dunque nella PDI.
A seconda della quantità di bit di parità si può procedere in due modi:
- **FEC (Forward Error Connection)**: i molti bit di parità si usano per cercare di correggere gli errori, senza la ritrasmissione del pacchetto.
- **ARK(Automatic Repeat reQuest)**: i pochi bit di parità cercano di rilevare errori e chiedere la ritrasmissione.

Nel corso ci occuperemo di ARQ di cui esistono 3 tecnica differenti.
- Stop and wait
- Go back N
- Selective repeat

# Stop and wait
---
Passi del processo di Stop and wait:
- Il TX fa una copia della PDU da inviare (per una possibile ritrasmissione). 
- Inizializzazione del TX e del RX sui parametri del protocollo, ovvero abbiamo V(T)=0 e V(R)=0.
- Successivamente invia la PDU al RX e fa partire un orologio (timer) detto tempo di **timeout**. In questo lasso di tempo attende una risposta alla PDU che viene inviata (cioè attende l’ACK). Se il timeout scade prima dell’arrivo della conferma, ripete la trasmissione facendo ripartire il timeout.

Quando il RX riceve la PDU allora:
- RX controlla la correttezza della PDU
- Se è corretta allora fa il controllo in sequenza (ovvero se è proprio quella che si aspettava)
- Manda al TX l'ACK.
- La PDU è passata ai livelli superiori

![[Pasted image 20221012110144.png]]

Se arriva un pacchetto al ricevitore con il codice di parità sbagliato, **il ricevitore non fa nulla!** 

**Round trip time (RTT)**: intervallo temporale dall'invio del primo bit del pacchetto alla ricezione dell'ultimo bit del relativo ACK.
Come gestire la numerazione dei pacchetti?

### ALTERNATING BIT PROTOCOL 
Metodo per il quale si alterna la numerazione fra 0 e 1.

Nel caso di ricezione errata della PDU allora devo tenere conto che la regolazione del timeout è delicata, se troppo lunga perdo efficienza, se troppo corto (e magari l'ACK è un po' in ritardo) riinvio il pacchetto anche se era arrivato.

Nel caso di perdita dell'ACK di un pacchetto allora dopo un po' il TX prova a rinviare la PDU, ma il RX che ha già incrementato il numero di sequenza finirà per ricevere il pacchetto sbagliato.
A questo punto RX rinvia la conferma, che corrisponde al pacchetto che si aspetta.

Nel caso di canali non sequenziali si possono verificare eventi di perdita e/o duplicazione di PDU. Per ovviare a ciò dobbiamo aumentare i bit per numerazione e introdurre il concetto di **TTL (Time To Leave)**.

# Go Back N
---
Il metodo di stop n wait è soggetto a elevati ritardi per l'attesa dell'ack di ogni pacchetto.
La soluzione a ciò consiste nel **trasmettere più PDU prima di fermarsi in attesa delle conferme**.

**Finestra di trasmissione** $W_{T}$: quantità massima di PDU che è possibile inviare in rete senza aver ricevuto alcun riscontro.
La dimensione di quest'ultima dipende dalla memoria allocabile in rete.
Inoltre $W_{T}$ rappresenta anche il numeor massimo di PDU presenti contemporanemente sul canale o in rete.

**Finestra di ricezione $W_{R}$**: quantità massima di PDU che un ricevitore è disposto ad accettare e memorizzare.
Anche questa dipende dalla memoria allocabile in ricezione.

Come funzion il Go Back N?
Il trasmettitore copia fino ad N = $W_{T}$ PDU che vuole inviare e le trasmette.
Attiva un solo orologio per le N PDU.
Attende gli ACK.
Per ogni ACK non duplicato che riceve shifta la finestra si trasmissione in avanti quanti sono i pacchetti confermati.
Se scade il timeout, ripete la trasmissione delle PDU non confermate.

Dal lato RX invece si controlla la correttezza della PDU, se lo è si controlla il numero di sequenza e si manda l'ACK.
Inoltre se la PDU risulta essere la prima della sequenza si consegna la SDU relativa a livelli superiori.

- $W_{T}>1$ in trasmissione
- $W_{R}=1$ in ricezione (come lo stop n wait)

### Come decidere la sematica dell'ACK?
Dato che può variare, il TX e RX si mettono preventivamente d'accordo.
- ACK cumulativo, si manda il numero di sequenza che mi aspetto ("Ho ricevuto tutto fino ad n escluso, dunque ora mandami n")
- ACK selettivo, si manda indietro il numero del pacchetto ricevuto ("Ok bene ho ricevuto il pacchetto n")
- NAK (Negative acknowledgment), non si manda un ack ma si manda un negative(n) quando voglio la ritrasmissione del pacchetto ennesimo.
- PiggyBacking, tecnica possibile nei flussi bidirezionali di pacchetti che consiste nel mettere l'ACK nella PDU che viaggia in direzione opposta

![[Pasted image 20221018142916.png]]

La numerazione delle PDU dovrebbe essere univoca, eppure si usa un metodo ciclico. (k bit di numerazione, ciclo dopo $2^k$ pacchetti)

![[Pasted image 20221018143159.png]]

### Go back n VS stop n wait
Nel go back N il TX è più complesso, deve gestire orologio, algortimi e memoria, il RX è identico
La finestra di trasmissione deve avere misure specifiche ($W_{T}<2^k$)


# Selective Repeat
---
Il limite del go back n è l'accettare solamente PDU in sequenza corretta, mentre il Selective repeat accetta tutte le PDU corrette, anche non in sequenza.
In sostanza sia la finestra di trasmissione e quella di ricezione hanno dimensione maggiore di 1 (dimensioni uguali).

Come funziona TX e RX?
Da parte del TX non c'è variazione rispetto al go back n.
Il ricevitore controlla, e se è in sequenza consegna al livello superiore.
Altrimenti se è dentro $W_{R}$ la memorizza, se è fuori dalla finestra la scarta.
Invia un ACK relativo all'ultima PDU ricevuta in sequenza.

### Quando conviene questa tecnica?
- nel caso di perdita singola, avremo prestazioni simili al go back n.
- Vantaggioso nel caso di perdite multiple, perchè basta che ne arrivi una copia al RX
- Vantaggioso se RTT < tempo di trasmissione della finestra
- Vantaggioso se applico ACK selettivi

### Osservazione
Vale sempre la relazione:
$$
W_{T}+W_{R}\leq 2^k
$$
dove $k$ sono i bit di memorizzazione
Throughput della comunicazione con selective repeat:
$$
\theta=\eta C
$$
Dove $C$ è la capacità del canale e $\eta$ rappresenta l'efficienza
