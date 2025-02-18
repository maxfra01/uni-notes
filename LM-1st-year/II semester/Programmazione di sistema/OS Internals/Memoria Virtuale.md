# Virtualizzazione

Spesso non è necessario che l'intero programma sia in [[Memoria principale]]: è possibile che sezioni del programma non servano o non vengano mai chiamate.
L'idea è quindi caricare solamente quello che serve: si parla di **partial loading**.
Così facendo ho più spazio in memoria, posso caricare più processi, posso velocizzare il caricamento dei processi e ho un maggior parallelismo.

La **memoria virtuale** è una separazione più netta fra la memoria logica e fisica. Grazie a questo meccanismo abbiamo diversi benefici:
- Solamente la parte di programma necessaria è caricata in memoria
- Lo spazio di indirizzamento logico può essere più grande di quello fisico
- Permette la condivisione dello spazio di indirizzamento
- Aumenta l'efficienza della creazione dei processi
- Posso aumentare il numero di programmi in esecuzione in parallelo
- Meno I/O da fare per caricare o swappare i processi

Lo **spazio di indirizzamento virtuale** è una vista logica degli indirizzi in memoria.

La memoria virtuale può essere implementata in due modi:
- **Demand paging**
- Demand segmentation

Lo scenario che consideriamo è il seguente:

![[Pasted image 20240315103443.png]]

Nella memoria logica abbiamo diverse convenzioni usate: il codice, e le variabili globali sono negli **indirizzi bassi** (a partire da zero), al di sopra di questa struttura abbiamo lo **stack** e lo **heap** (tipicamente lo stack cresce verso il basso, mentre lo heap cresce verso l'alto).
Lo spazio in mezzo è utile per l'uso di librerie condivise.

# Demand paging

Teoricamente si può portare tutto il programma in memoria al **load time**. In realtà posso decidere di fare entrare **in memoria principale solamente le pagine che mi servono** (ad esempio all'inizio posso portare solamente la prima pagina del main): abbiamo ridotto l'I/O necessario, la risposta in partenza è più rapida e si minimizza l'uso della memoria.
Se ho bisogno di una pagina ho due scenari (una volta ottenuto il riferimento alla pagina):
- Se il riferimento non è valido faccio abort del programma
- Se non si trova in memoria la carico.

Come posso **determinare le pagine** di cui ho bisogno?
Ho bisogno di inserire nel meccanismo di esecuzione una nuova funzionalità della MMU che permetta di portare in memoria le pagine se necessarie.
Ciò che mi permette di fare questa nuova operazione è il **validity bit**:
- Nel caso 1 significa che la pagina si trova in memoria
- Nel caso 0 significa che la pagina non si trova in memoria
Nel caso 0 devo distinguere due casistiche: la pagina può davvero non trovarsi in memoria (va dunque caricata) oppure non è proprio valida. In ogni caso scateno il **page fault**.

Gestire un **page fault** è costoso, dunque va **ridotto** il più possibile.

Lo scenario di un page fault è il seguente:
1. Si genera il page fault
2. Il sistema operativo cerca in un'altra tabella e decide
	- Se la reference è invalida, allora si fa abort
	- Oppure si deduce che la pagina non è in memoria
3. Il sistema operativo cerca un frame libero
4. Si fa lo swap tra il disco e il frame
5. Si setta il validation bit a 1
6. Si riparte dall'istruzione che ha causato il page fault

![[Pasted image 20240315105825.png]]

Altri possibili scenari:
- **Pure demand paging**: si parte con nessuna pagina caricata in memoria, poi ad ogni necessità si genera un page fault e si portano le pagine in memoria
- **Multiple page faults**: un'istruzione può accedere a pagine multiple per completare un'istruzione quindi si genererebbero multipli page faults (il problema è mitigato dalla località dei riferimenti)
- **Swap space**: è una memoria secondaria che viene in aiuto nella procedura di swapping

### Free frame list

La maggior parte dei sistemi operativi mantiene una **lista di frame liberi**: è un pool di frame che soddisfa la richiesta (non è un problema di fit, perciò prendo il primo frame in lista).
Ogni frame libero (appunto perchè libero) contiene il puntatore al prossimo blocco libero: basta sapere dove parte la lista e si possono scorrere tutti i frame liberi.

Talvolta sono necessari dei frame azzerati: si utilizza una tecnica chiamata **zero-fill-on-demand**.

Quando un sistema fa il boot, metto tutto lo spazio disponibile nella free frame list.

### Page fault dettagliato

1. Si scatena la trap
2. Si salvano i registri utente e lo stato del processo in esecuzione
3. Si determina che la trap è un page fault
4. Controlla che l'accesso alla pagina invalida sia legale, e determina la posizione della pagina su disco
5. Fa un issue per una read da disco al frame libero (I/O)
6. Mentre si aspetta il trasferimento la CPU viene concessa ad altri processi
7. Un interrupt informa che la copia è completata (I/O terminato)
8. Salva i registri e lo stato per gli altri utenti
9. Determina che l'input era per I/O da disco
10. Si corregge la entry in memoria 
11. Si attende la CPU
12. Si riprende il processo 

Per valutare le **performance** del demand paging definiamo il **Page fault rating**: $0\leq p \leq 1$
$$
\text{EAT} = (1-p) \cdot \text{memory access} + p\cdot (\text{page fault overhead + swap page out + swap page in})
$$
L'operazione è costosa:
- Possiamo cercare di ridurre la possibilità di page fault
- Possiamo velocizzare l'operazione di gestione del page fault

### Page fault optimizations

Una prima opzione è avere un disco dedicato ad alte prestazioni per eseguire solamente operazioni di swap. Simile a questa strategia è avere una partizione apposita per fare swap, che dev'essere più veloce del disco.

Altra possibilità: per tutto ciò che deve arrivare ad un eseguibile (tipicamente dati in sola lettura) prendiamo dal disco ma evitiamo di salvare (no page out) e scartiamo le pagine.
Esistono però parti di programmi che non possono essere scartati (come lo stack o l'heap).

### Copy on write

Nei sistemi UNIX, dopo una fork è creato un processo figlio che condivide lo spazio dati del processo padre. La tecnica di **copy-on-write** permette di creare dati appositi per un processo solamente se il processo stesso li modifica.
Questo porta alla velocizzazione della creazioni di un processo figlio.

# Page Replacing

Se durante un page fault non esistono free frame: **page replacement**, ovvero cerco una pagina che attualmente si trova in memoria, per buttarla fuori e generare un free frame.
Come scegliere però la pagina da buttare fuori? Se trovo una pagina che non mi serve con dirty bit a 0, significa che posso velocizzare l'operazione perchè non devo salvarla.

Page replacement
1. Trovare la pagina su disco
2. Se c'è un frame libero usalo
3. Se non c'è un free frame usare un algoritmo di determinazione pagine per trovare la pagina vittima (che sarà tolta dalla memoria)
4. Eventualmente se il dirty bit è a 1 si scrive la vittima su disco
5. Si porta la pagina desiderata in memoria, si aggiorna la page table e si riprende il programma

### Algoritmi di page replacing

Algoritmi il cui obiettivo è minimizzare la possibilità di page fault.
Per prima cosa eseguiamo il programma e memorizziamo la sequenza di accessi a pagine da parte del processo: abbiamo quindi una stringa di numeri (numero di pagina), simile alla seguente:
$$
7,0,1,2,0,3,0,4,2,3,0,3,0,3,2,1,2,0,1,7,0,1
$$
Supponiamo di avere dunque una **stringa di riferimento** $w$ per ogni processo che ho fatto girare: la probabilità empirica di una page fault è la seguente:
$$
f(A,m) = \sum_{\forall w}p(w) \frac{F(A,m,w)}{len(w)}
$$
Dove $A$ è l'algoritmo di page replacement in esame, $m$ il numero di frame disponibili e $F(A,m,w)$ il numero di page fault generati da una certa stringa $w$, usando un algoritmo $A$ su un sistema con $m$ page frames.

L'algortitmo **ottimo** da usare sarebbe quello che permette di rimuovere le pagine a cui non verrà fatto accesso per più tempo, ma non è ovviamente possibile (non si può predirre il futuro).
### FIFO Algorithm

![[Pasted image 20240325123346.png]]

Strategia molto semplice; è soggetta però all'anomalia di **Belady**, ovvero aumentando il numero di frame liberi porta a più page fault.

### LRU - Least Recently Used

La strategia base è rimuovere la pagina che non viene usata da più tempo (si guarda il passato della memoria).

![[Pasted image 20240325124150.png]]

La domanda base è come implementare questo algoritmo:
- **Counter** implementation: ad ogni pagina vado ad associare un tempo di accesso/contatore che devo controllare ogni volta che si verifica un page replacement; inoltre devo aggiornare questo contatore quando faccio l'accesso. Questa tecnica però è molto costosa 
- Stack implementation: Si tiene traccia di una coda prioritaria basata sugli accessi. In fondo allo stack sarà presente la pagina a cui si è fatto accesso più nel passato, mentre all'inizio c'è la più recente. Per fare questo serve una struttura dati che sia O(1), doppio linkata e vanno sempre aggiornati i puntatori.

Qualunque implementazione si scelga occorre in ogni caso dell'**hardware dedicato** e comunque si presenterebbe un costo elevato. Si cerca allora di produrre algoritmi che siano "quasi" LRU.

# Allocazione dei frame

Ogni processo ha bisogno di un numero **minimo di frame** quanti sono necessari per eseguire un istruzione. Il **numero massimo** corrisponde ai frame totali del sistema.
Per gestire l'allocazione dei frame abbiamo due alternative: **Fixed** e **Priority** allocation

Nel caso di fixed allocation abbiamo due strategia:
- Allocazione identica fra processi (100 frame, 5 processi, dunque 20 frame a processo)
- Allocazione proporzionale: si allocano i frame in modo proporzionale alla grandezza del processo

Quando si fa page replacement dobbiamo definire due politiche:
- Se un processo necessita di un frame, allora posso scegliere la vittima anche in frame destinati ad altri: questa tecnica è detta **Global replacement**
- Se un processo può scegliere solo dai frame riservati a lui, allora si parla di **local replacement**. 

Se manteniamo una lista di free frames, possiamo implementare il global replacement andando però a definire una soglia superiore o inferiore, oltre le quali il kernel interrompe la richiesta di frame liberi.

Molti sistema hanno la seguente struttura, Detta **NUMA**:

![[Pasted image 20240325134125.png]]

Allora si fa in modo di avvicinare i frame liberi alla CPU interessata.

# Trashing

Se un processo non ha abbastanza pagine, il page fault ha una probabilità molto alta: se questo accade allora otteniamo troppi context switch, poco utilizzo della CPU e poche reali esecuzione di istruzioni.
Se numerosi processi sono in esecuzione allora è possibile che si ottengano troppe richieste di paginazione e di conseguenza un degrado delle prestazioni (**trashing**).

![[Pasted image 20240325134523.png]]

### Page fault frequency

Si definisce una **frequenza di page fault** ragionevole (accettabile) e si usa una policy di replacement di tipo locale. Se il rate effettivo di page fault è più alto do un frame in più al processo, se è più basso allora tolgo un frame al processo.

Un possibile algoritmo prevede il seguente funzionamento:
- Definisco $\tau$ come l'intervallo di tempo dall'ultimo page fault
- Definisco una certa soglia $C$
- Se $\tau < C$ si aggiunge un nuovo frame al processo
- Se $\tau \geq C$ si tolgono dal processo tutte le pagine con reference bit a 0 
- Dopo metti a 0 il reference bit di tutte le pagine nel processo

![[Pasted image 20240325141241.png]]

Riassunto: al posto di assegnare un frame fisso al processo, diano un numero di frame dinamico.
# Allocazione memoria kernel

Lasciando perdere come si allocano i frame, concentriamoci sulla memoria del kernel.
Il Kernel necessita di una porzione di memoria utilizzata in modo speciale (nel caso di IO e di Page Table si richiede una porzione di memoria contigua).
Due alternative:
- buddy system
- slab allocator

### Buddy system

Sistema di allocazione **contigua** che cerca di sfruttare comunque i vantaggi della paginazione: si alloca solamente in base alle potenze di 2 (accetto frammentazione interna) arrotondando per eccesso.
Quando richiedo allocazione di poco spazio, il chunk libero viene diviso in due (potenza di 2 dell'ordine inferiore).
ES. 256 KB chunk, richiedo allocazione di 21KB, allora otterrò due chunk da 128KB.

![[Pasted image 20240415115632.png]]

### Slab allocator

Sistema di allocazione contiguo che alloca a multiple di pagine: una **slab** è una o più pagine contigue.

![[Pasted image 20240415115643.png]]

# Prepaging

Per diminuire il numero di page fault all'avvio di un processo, si usa una strategia di **preallocazione** di pagine per un data processo (prima di essere avviato).
Assumiamo che $s$ pagine siano preparate e $a$ il numero di pagine usate effettivamente usate ($a$ è una percentuale).
