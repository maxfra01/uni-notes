[[Introduzione]] 

# Struttura di una memoria Cache

Le memorie **Cache** sono di piccole dimensioni, e sono interposte fra la memoria principale e il processore.
Questo tipo di memorie servono ad aiutare il processore a reperire più velocemente i dati dalla memoria principale (si evita il collo di bottiglia, in quanto accedere alla memoria principale è costoso).

### Livelli di memoria 

Normalmente l'architettura classica di memoria e processore (notiamo l'ordine di grandezze delle frequenze di accesso ai vari tipi di memoria):

![[Pasted image 20231030102138.png]]

```ad-note
title: SRAM vs DRAM
Le memorie presenti nei SoC sono tipicamente SRAM (composta da Flip Flop), mentre in altri casi la memoria è DRAM (composta da condensatori): è separata del processore, perchè ha bisogno di più circuiteria per il rinfresco.
```

### Località dei riferimenti

I vantaggi di avere una memoria cache sono legati a due principi.

- **Temporal locality**: Se a un tempo $t$ il programma accede a una certa cella di memoria, allora è molto probabile che si accederà alla stessa cella in un tempo $t +\Delta t$
- **Spatial locality**: Se a un tempo $t$ il programma fa un accesso a una cella $x$ allora è molto probabile che in un tempo $t +\Delta t$ si farà accesso a una cella $x \pm e$

### Performance

Per definire una **performance** dobbiamo prima dare delle definizioni: quando cerchiamo di acquisire un dato e questo si trova in cache allora abbiamo un **cache hit**. Il concetto opposto è detto **miss**.
Chiamiamo $h$ il **cache hit ratio**, $C$ il tempo di accesso alla cache e $M$ il tempo di accesso in memoria quando il dato non sta in cache (miss).
Definiamo il tempo media di accesso in memoria come:
$$
t_{ave} = h \cdot C +(1-h) \cdot M
$$
Valori normale per $h$ sono intorno a $0.9$

### Architettura della cache

![[Pasted image 20231030103859.png]]

Una prima parte che si occupa di controllo e logica della memoria è detta **Cache controller**.
La matrice della memoria ha una struttura divisi in **Data array** e **Directory array**.
Il campo **Tag** serve per capire se ho avuto un hit o un miss: contiene di fatto l'indirizzo della RAM a cui avrei fatto accesso in memoria principale.
Il **Validity bit** è un bit che indica se il dato è corretto e può ancora essere usato per ottenere una hit (in sistemi multiprocessori, se un altro core scrive il dato allora non sarà valido per gli altri).

La cache è organizzata in **linee**.
Ogni linea contiene un **blocco di memoria** (composto da un multiplo di word).
Il **Tag** contiene informazioni su quale blocco è memorizzato nella linea.

La logica della cache contiene la logica per:
- Intercettare gli indirizzi a cui vuole fare accesso il processore (bus)
- Controllare se all'interno della cache c'è un blocco a cui il processore vuole accedere
- Caricare i blocchi se necessario

Gli indirizzi generati dalla CPU si compongono di 3 campi: il primo campo **TAG** serve appunto per il confronto con il tag della cache. **INDEX** conterrà informazioni sul numero della linea della cache con cui fare il confronto. **OFFSET** andrà a selezionare la word all'interno del blocco memorizzato in cache.

![[Pasted image 20231030105153.png]]

1. Si legge l'INDEX per sapere a quale linea di cache accedere
2. Si confronta il TAG nel CPU address e il tag della cache
3. Se sono uguali e il validity bit è a 1 allora estraggo il blocco dalla cache
4. Uso il campo OFFSET per accedere a una particolare word del blocco

### Posizione della cache

Normalmente le cache sono posizionate fra Il bus e la CPU, e non fra CPU e main memory.

![[Pasted image 20231030105839.png]]

In questo modo il carico sul bus è ridotto, e inoltre questa soluzione è compatibile con sistemi multiprocessore.

In certi casi c'è distinzione fra **Data cache e Instruction cache**: compatibile con architettura Harvard (memoria di codice e di dato separate).

### Parametri di una cache

**Dimensione di cache, dimensione del blocco, mapping, algoritmi di replacing, meccanismi di update di memoria principale**.

Scegliere la dimensione della cache è fondamentale per costi e performance.
All'aumentare della dimensioni aumentano i costi, le performance del sistema, ma la cache stessa diventa più lenta: ad oggi non si va sopra alcuni MBs.

### Mapping

E' il meccanismo per il quale sappiamo **in quale linea di una cache un certo blocco di memoria è caricato**.

Un primo modo per il mapping è il **direct mapping**: dato il numero del blocco in memoria principale si fa la seguente operazione:
$$
k=i \, \, mod \space N
$$
Dove $N$ è il numero di linee della cache, mentre $i$ è il numero del blocco in memoria.
Per calcolare in modo semplice $k$ si considerano i $\log_{2}(N)$ bit **meno significativi di $i$**.

Il **Set associative Mapping** consiste nel dividere le linee di cache in $S$ sottoinsiemi, contenenti ciascuno $W$ linee ($W$-ways). Per calcolare il blocco $k$ si fa la seguente operazione:
$$
k = i \, \, mod \, \, S
$$
Il **Fully associative mapping** è la soluzione più flessibile: ogni blocco può finire in ogni linea di cache

### Algoritmi di Replacing

Questi algoritmi definiscono quali linee devono essere usate per memorizzare dei blocchi di memoria.
Le strategie sono: **LRU (Least Recently Used)**, **FIFO**, **LFU (Least frequently Used)**, **random**.

![[Pasted image 20231030115539.png]]

### Memory Update

Quando eseguiamo un'operazione di scrittura di un dato in cache allora dobbiamo aggiornare il valore del dato anche in memoria principale: dobbiamo avvalerci di due soluzioni, ovvero **Write Back** e **Write through**.

Per il meccanismo di write-back introduciamo il **dirty bit** che tiene conto se quella linea è stata modificata: quando la linea è rimossa dalla cache e il dirty bit è settato a 1, allora vado a correggere il valore del dato in memoria principale. (utile per il single core)

Il Write through consiste invece nello scrivere anche in memoria principale ogni volta che modifico una linea di cache. (utile nel caso di multicore)

### Coerenza delle cache

Considerano un'architettura con più di una CPU e più di una cache allora si introducono dei problemi di coerenza.
Si introduce quindi il **validity bit**, che se settato a 0 fa produrre un miss in cache (la linea non è valida).
All'avvio di un sistema, tutte le linee hanno il validity bit a 0.

# Esempio di dimensionamento

- 64 Kbyte per i dati
- direct mapping
- blocchi di 4 byte
- 1 block per entry page
- Indirizzi da 32 bit

