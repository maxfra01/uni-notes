# Scheduling dinamico

Lo scheduling dinamico consente di **identificare le dipendenze** che sono sconosciute a tempo di compilazione. Semplifica il lavoro del compilatore e permette al processore di tollerare ritardi non predicibili. Inoltre, permette di eseguire lo stesso codice su differenti processori.

Consideriamo il seguente esempio:

```
DIV.D F0, F2, F4
ADD.D F10, F0, F8
SUB.D F12, F8, F14
```

La seconda ADD subirà uno stallo a causa della dipendenza di dato, la SUB invece dovrà aspettare che la ADD finisca, a causa dello stallo della pipeline.
Ovviamente spostando la SUB prima della DIV non fa sprecare colpi di clock: sarà compito dell'hardware di identificare queste dipendenze ed ottimizzare il codice.

### Esecuzione fuori ordine

L’esecuzione fuori ordine prevede l’esecuzione di istruzioni che non sono state ancora eseguite. Questo può però comportare problemi, in particolare introducendo:
- WAR hazards: write after read
- WAW hazards: write after write
- esecuzione imprecisa

Se l’esecuzione out‑of‑order è concessa, potrebbe diventare impossibile riuscire a fare una gestione precisa delle eccezioni. Questo potrebbe comportare che quando una eccezione è rilasciata, una istruzione precedente o successiva all’eccezione ha ancora da essere completata. In entrambi i casi potrebbe essere difficoltoso far ripartire il programma.

Per non far diventare bloccante la fase di Decode occorre **dividerla in 2 parti**: **issue** e **read**.
Nella prima parte si controllano Hazard Strutturali e si classifica l'operazione, successivamente nella seconda parte si controllano gli operandi e eventualmente si fa partire la fase di esecuzione.

# Algoritmo di Tomasulo

L'idea base dell'algoritmo è tracciare la **disponibilità degli operandi** e introdurre un meccanismo di **renaming** automatico da parte dell'hardware.

Un nuovo componente nell'hardware è la **reservation station**: sono ulteriori locazioni di memoria che ci permettono di bufferizzare le istruzioni, che attendono un certo tempo prima di essere completate (magari devono attendere la disponibilità di un operando). Inoltre:
- Sono in grado di identificare unicamente un istruzione
- Implementano il renaming in modo automatico
- Ogni stazione è legata a una certa unità funzionale

Il secondo nuovo elemento è il **Common Data Bus (CDB)**: tutti i risultati ottenuti dalle unità funzionali o dalla memoria vengono mandati al CDB che collega tutto (ad eccezione del load buffer) e permette a tutte le unità si caricare simultaneamente un dato quando è disponibile.

```ad-warning
title: Utilizzo simultaneo del CDB
Dato che il bus è condiviso è possibile che si generino degli stalli strutturali.

```

Quando le istruzioni vengono fetchate, vengono mandate all'interno delle reservation station ("sale d'attesa" per le unità funzionali vere e proprie).
Le stazioni sono anche collegate ai registri e al common data bus, quindi una stazione aspetta la disponibilità dei suoi operandi.
E' poi presente l'unità di memoria che si occupa delle operazioni di load e store.

![[Pasted image 20231106103228.png]]

### Step di esecuzione

Sono 3: **Issue, Execute e Write result**, possono tutte avere lunghezze diverse.

Nel caso si Issue, allora **le istruzioni si spostano** dall'instruction queue **verso una reservation station**, ma in questa fase potrebbe esserci uno stallo strutturale (se tutte le reservation station sono piene).

Per quanto riguarda la fase di esecuzione, dobbiamo fare delle distinzioni in base al tipo di operazioni:
- Per operazione **normali** allora non appena un risultato è disponibile si mette a disposizione sul Common Data Bus. Ovviamente l'esecuzione parte solo quando tutti gli operando sono disponibili nella reservation station (RAW eliminati).
- Per le **load e store** si parte sempre dal calcolo dell'effective address (non appena il base register è disponibile) poi successivamente:
	- Per le load si può procedere non appena la memoria è libera
	- Per le store devo prima attendere gli operando e successivamente attendere che la memoria sia libera.
- Le operazioni di **branch** rimangono complesse per l'approccio di Tomasulo, perciò non è concesso per un'istruzione andare in esecuzione se i salti prima non sono completate. Occorre risolvere il problema con la **speculazione**.

In quest'ultima fase di **Write Result**, i risultati si trovano sul CDB e tutte le unità che ne necessitano possono accedervi. In questa stessa fase le operazioni di store possono scrivere in memoria.

### Instructions identifiers

Ogni reservation station è associata a un **identificatore**: quest'ultimo viene usato dalla altre stazione per sapere chi dovrà produrre un certo dato, ovvero **identificano l'operando generato da una certa istruzione**.
Di fatto vengono usati per **rinominare i registri** e implementare il renaming.

### Campi delle reservation station

![[Pasted image 20231106105345.png]]

- Campo OP: identifica il **tipo di istruzione** (come ADD, DIV, SUB ...)
- Campi $V_{j},V_{k}$: contengono i **valori** degli operandi
- Campi $Q_{j},Q_{k}$: contengono le reservation station che produrranno gli operandi, se non disponibili
- Campo A: è usato solamente per le istruzioni di load o store, e prima contiene l'immediato, dopo l'effective address.
- Campo Busy: identifica lo **stato** della reservation station e quindi della corrispondente unità funzionale.

```ad-note
title: Assenza delle destinazioni nelle reservation station
Nelle reservation station non sono esplicitate le destinazioni delle istruzioni: i risultati vengono messi sul Common Data Bus, poi chi ne necessita può accedervi.
```

### Register status

Per il register file: ogni elemento ha associato un $Q_{i}$, che contiene il numero della reservation station che andrà a scrivere in quel determinato registro.
Se il campo è nullo, allora non ci sono istruzioni pendenti che vogliono scrivere su quel registro.

![[Pasted image 20231106111622.png]]

### Pro e contro

| Pro                                               | Contro                                         |
| ------------------------------------------------- | ---------------------------------------------- |
| Lo logica di detection degli hazard è distribuita | L'hardware ha una complessità elevata   (serve un buffer associativo per trovare le entry della stazione)       |
| Gli stalli WAW e RAW sono eliminati               | Il Common Data Bus può fare collo di bottiglia |

La gestione dei **cicli** è intrinsecamente **parallelizzata** e quindi non è necessario fare loop unrolling.

Normalmente le operazioni di load e store possono essere eseguite in qualsiasi ordine, purché non agiscano sullo stesso indirizzo di memoria, tuttavia se questo non viene rispettato allora si possono generare stalli RAW e WAR.
Per risolvere:
- Nel caso di una load, si controlla l'indirizzo e se c'è una store agente sullo stesso indirizzo, allora la load non si manda in esecuzione finchè la store non è completata
- Nel caso si store, si controlla l'indirizzo e poi non si manda la store in esecuzione finchè non si completano load e store precedenti

[[Branch Prediction Techniques]].