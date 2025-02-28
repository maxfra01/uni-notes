# Definizioni utili ITU_T
---

### Funzioni in una rete di telecomunicazioni
- Segnalazione: scambio di informazioni che riguardano l'**apertura, la chiusa e la gestione** per la rete di telecomunicazione.
- Commutazione: processo di interconnesioni di unità funzionali.
- Trasmissioni: trasferimento di segnali punto a punto.
- Gestione: tutte le operazioni restanti
- [[Protocolli a finestra]]

### Definizioni e Topologie per le reti
- Mezzo trasmissivo: mezzo fisico in grado di trasportare segnali.
- Canale: singolo mezzo trasmissivo
- Banda: quantità di dati per unità di tempo, anche detto **bit Rate**
- Capacità di un mezzo: massima velocità trasmissiva misurata in bit/s che dipende dalla tecnologia con cui vengono realizzati trasmettitore e ricevitore, ed è limitata superiormente dalla banda fisica del mezzo.
- Traffico offerto: Bit Rate che una sorgente cerca di inviare in rete
- Traffico Smaltito (**throughput**): porzione di traffico offerto che giunge correttamente alla destinazione
$$
Throughput \leq capacità \space del \space canale
$$
$$
Throughput \leq traffico \space offerto
$$
- Topologia di una rete di telecomunicazione: insieme di nodi e segmenti che fornisce un collegamento tra due o più punti per permettere la telecomunicazione tra essi
- Canale punto-punto: due soli nodi collegati agli estremi del canale che viene utilizzato in modo paritetico.
- Canale multi-punto: più nodi collegati ad un unico canale: un nodo master e numerosi slave 4.
- Canale broadcast: unico canale di comunicazione condiviso da tutti i nodi, l’informazione è inviata da un nodo e ricevuta da tutti gli altri e se i dati trasmessi contengono l’indirizzo del nodo destinazione realizzo di fatto un punto-punto.

# Topologia delle reti

### Topologia a maglia completa
Presenta un numero di canali pari a $C=\frac{N(N-1)}{2}$
Vantaggi:
- alta tolleranza ai guasti
- molti percorsi fra due nodi
Svantaggi:
- elevato numero di canali
- algoritmo di instradamento.

### Topologia ad albero
Numeri di canali pari a $C=N-1$
Vantaggi:
- basso numero di canali
- instradamento semplice
Svantaggi:
- molto vulnerabile ai guasti
- un solo percorso fra due nodi

### Topologia a stella attiva
Numero di canali pari a $C=N$
La stella 'attiva'  al centro presenta tutta la complessità della rete e l'algoritmo di instradamento.
Spesso usata in **reti locali**.
Vantaggi:
- basso numero di canali
Svantaggio:
- vulnerabile ai guasti al centro stella

Nel caso di topologia a stella 'passiva' ($C=1$)  ha il solo vantaggio di semplificare i canali, ma è sempre vulnerabile ai guasti al centro stella.
Di fatto realizza una rete con trasmissione *broadcast*.

### Topologia a maglia
Con numero di canali pari a $N-1<C<\frac{N(N-1)}{2}$
Nonostante sia la topologia con la complessità di instradamento più alta è la più utilizzata nelle reti di telefonia e in internet.

### Topologia ad anello

La topologia ad anello può essere unidirezionale o bidirezionale come segue:$$
C=\frac{N}{2} \quad C=N
$$La topologia ad anello viene molto utilizzata in reti locale e metropolitane per costruire topologie magliate (SDH reti di dorsale ottica) ed esistono uno o due percorsi possibili per ogni coppia di nodi (senso orario o antiorario). In caso di guasto l’anello bidirezionale assicura la sopravvivenza della rete a capacità dimezzata. L’anello bidirezionale offre un instradamento alternativo in caso di guasto (ed è anche il caso più semplice).

### Topologia a bus

Esiste una sola scelta possibile di percorso tra ogni coppia di nodi, viene utilizzata in reti locali e metropolitane. Bus passivo: canale su cui sono agganciate le stazioni. I nodi hanno prestazioni che dipendono dalla posizione (mentre nella topologia a stella ciò non accade).
Nel caso di bus attivo: $C=N-1$
Nel bus passivo: $C=1$

### Differenza fra topologia fisica e logica

- logica: definisce l'interconnessione tra nodi mediante canali.
- fisica: tiene conto dei mezzi trasmissivi effettivi.

![[Pasted image 20221002194438.png]]