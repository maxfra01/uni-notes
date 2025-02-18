# Sezioni critiche
---
Nell'ambito della programmazione concorrente occorre evitare corse critiche o risolvere l'assenza di funzioni non rientranti, e per fare ciò occorre **sincronizzare opportunamente i processi o threads**.

Consideriamo il "too much milk problem", una coda FIFO o uno stack LIFO.
Definiamo le **sezioni o regioni critiche** quelle parti di codice, comuni a più processi o threads che vanno ad agire sugli stessi oggetti.
Alternativamente una SC o RC è una sezione di codice nella quale più di un processo o thread **competono per l'uso di risorse comuni**.

Occorre implementare un **protocollo di accesso per la mutua esclusione**.
Dunque andiamo a definire una sezione di accesso in cui si effettua la **prenotazione** alla sezione critica, e una sezione di **rilascio** delle risorse.

 ```ad-note
 E' importante che le sezioni non critiche non siano protette, altrimenti questo va a scapito del parallelismo
 ```

Ogni soluzione alle Sezioni critiche deve rispettare 4 caratteristiche:
- Si deve garantire la **mutua esclusione**
- Si vuole garantire il **progresso** dunque voglio evitare deadlock
- Si vuole garantire l'assenza di **starvation**, dunque attesa finita
- Si vuole garantire **simmetria** quindi nessun processo deve essere avvantaggiato ( a meno di necessità )

### Soluzioni delle sezioni critiche

- Soluzioni di tipo **software**, dove la correttezza è garantita dalla scrittura di codice del programmatore
- Soluzioni di tipo **hardware**, basate sulle architetture che garantiscono l'atomicità (a livello assembler)
- Soluzioni **ad hoc**, dove si garantisce la correttezza a carico dal sistema operativo (ad es. i semafori)


# Soluzioni Software
---
Sono basate sull'utilizzo di **variabili globali**, e analizzeremo il caso con soli due P/T (processi o threads) perchè con più di due è difficile la generalizzazione.

### Soluzione 1

Esiste una variabile globale del tipo: `int flag[2]={FALSE,FALSE}`

![[Pasted image 20221128104415.png]]

Ma in questo casa la mutua esclusione non è assicurata in quanto la gestione della variabile di lock è a carico di due sezioni a se stanti.
Inoltre questa soluzione effettua un **busy waiting** e quindi spreca cpu time.

### Soluzione 2

![[Pasted image 20221128104618.png]]

Soluzione simmetrica a quella prima, ma in questo caso non si garantisce il progresso.

### Soluzione 3

Una variabile globale definisce il turno del processo che può procedere.

![[Pasted image 20221128104659.png]]

Questa soluzione non garantisce l'attesa definita, in quanto per cambiare turno bisogna che uno dei due processi si risolva, ma se uno sta in SC per sempre allora l'altro va in starvation.

### Soluzione 4

![[Pasted image 20221128104837.png]]

Si utilizzano entrambe le variabili globali viste prima, il flag viene assagnato a se stessi e il turno è assegnato all'altro.
1. La mutua esclusione è garantita in quanto non possono entrare insieme (variabile `turn` univoca) e nemmeno in momenti diversi (variabile `flag`)
2. Progresso, unico pericolo è ciclare per sempre nel while interno ma per il flag ciò non è possibile in quanto se uno non vuole entrare allora l'altro ciclerà, se uno vuole entrare il ciclo si interrompe.
3. Attesa finita: starvation non è possibile
4. Codice simmetrica

Dunque **questa soluzione 4 è corretta** ma rimane il problema del cpu time, in quanto la soluzione si basa sul busy waiting e spin lock.
In generale le sw sono limitati a soli due P/T e sono complessi per il programmatore


# Soluzioni Hardware
---
- Soluzioni che permettono la **prelazione**
- Che non permettono la **prelazione**

Un sistema **senza diritto di prelazione** è tale per cui:
- I P/T in esecuzione nella CPU non possono essere interrotti

```ad-note
In un sistema monoprocessore senza diritto di prelazione non ci si pone il problema delle sezioni critiche ma tali sistemi sono insicuri e non adatti alla programmazione real-time.
```

Nei sistemi monoprocessori con diritto di prelazione posso momentaneamente bloccare questo diritto con un **interrupt**, gestito dal processo stesso (pericoloso ed insicuro).

L'unica strategia vera è un'estensione software ma a livello hardware, basata su **lucchetti di protezioni, lock**.
Il progettista garantisce l'atomicità di alcune operazioni che vengono usate per manipolare tali lock.
- Funzione **test-and-set** che setta e restituisce una variabile di lock globale

![[Pasted image 20221128110842.png]]

In ogni caso il lock viene messo a TRUE e si restituisce il valore di entrata del lock.
Ciò garantisce la mutua esclusione, ma si deve implementare in hardware la test and set. Svantaggi: cpu time ancora sprecato

- Funzione **swap**

![[Pasted image 20221128111117.png]]

Le funzione lock e swap non garantiscono solamente l'attesa finita, ovvero la non starvation.
Per soddisfare tutti i 4 criteri si usa una variante della test and set:

![[Pasted image 20221128111444.png]]

Le soluzioni hardware sono molto utili anche su multiprocessore, sono estendibili su n processi ma sono complesse da implementare in HW.


# Semafori
---
La soluzione offerta dagli OS non è basata su busy waiting e utilizza i semafori.
Un semaforo S è una variabile intera condivisa, protetta dal sistema operativo che si usa per inviare e ricevere segnali (non quelli visti a lezione).
L'atomicità in questo caso è garantita dal SO.

Manipolazione dei semafori a livello di pseudocodice:

| Primitive     | Significato                                                                                 |
| ------------- | ------------------------------------------------------------------------------------------- |
| `init(S,k);`  | Definisce e inizializza il semaforo al valore k                                             |
| `wait(S);`    | Permette (nella sezione di ingresso) di ottenere l'accesso della SC protetta dal semaforo S |
| `signal(S);`  | Permette (nell'uscita) di uscire dalla SC protetta dal semaforo S                           |
| `destroy(S);` | Cancella (free) del semafoto S                                                                                            |

### Init

Funzione che inizializza il semafoto S al valore k (intero)

```c
init (S, k){
	alloc S (global var);
	S=k;
}
```

Il valore k è anche il massimo valore che può assumere.
Distinguiamo fra:
- Semafori binari: k oscilla fra 0 e 1 (detti anche "MUTEX")
- Semafoti con conteggio: k indicherà sia il valore iniziale che quello massimo.

### Wait

Se il valore di S è negativo o nullo si blocca il processo chiamante (cioè la risorsa non è disponibile).

```c
wait(S){
	while (S<=0);
	S--;
}
```

In ogni caso si decrementa il semaforo!
Si noti che solo concettualmente si utilizza un busy waiting e quindi spreco della cpu, nella pratica si implementa una coda.

### Signal

Operazione opposta della wait ovvero in ogni caso si prova a incrementare il semaforo.

```c
signal (S){
	S++;
}
```

### Destroy

Non fa altro che liberare le risorse destinate al semaforo.

```c
destroy (S){
	free(S);
}
```

```ad-warning
Occorre prestare attenzione se voglio usare un solo semaforo per più processi, in quanto uno può essere molto più veloce dell'altro e quindi catturare la signal due volte.
```


# Implementazioni in C come struct
---
Una prima possibilità è quello di implementare i semafori come una struttura C.
E' una scelta non usata nella pratica perchè ci sono librerie apposta.

```c
typedef struct semaphore_s{
	int cnt;
	process_t *head;
}semaphore_t;

init(semaphore_t *S, int k){
	alloc S;
	S->cnt=k;
	S->head=NULL;
}

wait(semaphore_t *S){
	S->cnt=S->cnt-1;
	if (S->cnt < 0){
		push P to S->head;
		block P;
	}
}

signal(semaphore_t *S){
	S->cnt++;
	if(S->cnt<=0){
		pop P from S->head;
		wakeup P;
	}
}

destroy(semaphore_t *S){
	while (S->cnt <=0){
		free P from S->head;
		S->cnt++;
	}
}
```

Altre possibilità sono rappresentate da librerie che sono ormai standardizzate da tempo.

# Implementazione con Pipe
---
Usata molto per sincronizzare **processi** si basano sulla lettura bloccante su una pipe, facendo attenzione a non riempire la pipe (anche se improbabile).
Le regole sulla pipe sono già viste in [[Comunicazione fra processi]].

L'idea generale è quella di usare la write per simulare la signal, mentre la read bloccante emula al wait

```c
#include "unistd.h"

void semaphore_wait (int *S){
	char ctr;
	if(read (S[0],&ctr, sizeof(char)) != 1){
		printf("Errore");
		exit(-1);
	}
	return;
}

void semaphore_signal(int *S){
	char ctr= 'X';
	if(write(S[1],&ctr,sizeof(char)) != 1){
		printf("Errore");
		exit(-1);
	}
	return;
}
```

# Implementazione POSIX
---
Ci limitiamo ai semafori senza nome che sono implementati nella memoria interna del processo.
Per usare i semafori occorre includere la libreria `<semaphore.h>`
I semafori sono del tipo **sem_t** mentre tutte le primitive sono del tipo **sem_\***.

```c
int sem_init(sem_t *sem, int pshared, unsigned int value);

int sem_wait(sem_t *sem);

int sem_trywait(sem_t *sem); //versione non bloccante della wait

int sem_post(sem_t *sem);

int sem_getvalue(sem_t *sem, int *valP); //visualizza il valore del semaforo

int sem_destroy(sem_t *sem)
```


# Implementazione MUTEX
---
Si trovano in `<pthread.h>`
Le funzione che fornisce sono: init, lock, trylock, unlock, destroy

```ad-note
comodi ed efficienti
–PDM
```


# Problemi tipici
---
Un primo esempio è detto **del produttore e del consumatore**, all'interno del quale si ha un buffer di tipo circolare in cui memorizzo informazioni (compito del produttore) e le utilizzo (compito del consumatore).

In una situazione sequenziale enqueue e dequeue non sono mai insieme, ma sempre in sequenza, mentre in parallelo è possibile distinguire in due casi:
- Un **solo produttore** e un **solo consumatore**, dunque usiamo due semafori che contano rispettivamente gli elementi "pieni" e i "vuoti", di conseguenza aspettiamo su un determinato semaforo quando ce n'è bisogno.  ![[Pasted image 20221205110951.png]]
- Nel caso di P produttori e C consumatori dobbiamo proteggere l'estremità della coda su cui si scrive da accessi multipli in contemporanea, dunque aggiungo due mutex in più per garantire la mutua esclusione fra P e C ![[Pasted image 20221205111527.png]]


Un secondo famoso problema è quello **dei readers & writers**, dove c'è una base dati condivisa fra processi.
I Readers sono in concorrenza fra di loro mentre mentre i writers sono in mutua esclusione sia con altri writers che con i readers.
In una prima versione diamo precedenza ai reader e in un'altra diamo precedenza ai writers.

Dando precedenza ai reader usiamo dei mutex per garantire la mutua esclusione. 
![[Pasted image 20221205112002.png]]

In questa versione i writers sono soggetti a **starvation** perchè potrebbero attendere per sempre.
Nella seconda versione si da **precedenza ai writer** e dunque sono i reader che potrebbero andare in starvation.

![[Pasted image 20221205112954.png]]


Il problema del **tunnel a senso alternato** non è altro che un estensione del caso precedente con due insieme di reader.
Ci serviamo di due semafori, uno per ciascun senso di marcia, due variabili globali di conteggio e un semaforo globale di busy.

![[Pasted image 20221205113401.png]]

Ultimo problemi tipico è quello dei **5 filosofi**, dove ci sono 5 filosofi intorno a un tavolo con davanti a loro del riso e solamente 5 bacchette cinesi.
I filosofi o pensano o mangiano ma ciò implica che deve acquisire entrambe le bacchette, una a dx e una a sx.

Due modelli di risoluzione:
- nel primo si usa un mutex per proteggere la risorsa "cibo" dunque solo un filosofo mangia per volta ma non si massimizza la concorrenza (logicamente due possono mangiare contemporaneamente).
- Nel secondo usiamo un mutex per ogni bastancino e prima di mangiare un filosofo deve ottenere entrambi i bastoncini ( puo provocare deadlock )

Una terza soluzione è quella di inserire uno stato del filosofo in cui "vorrebbe mangiare":

![[Pasted image 20221205120823.png]]
