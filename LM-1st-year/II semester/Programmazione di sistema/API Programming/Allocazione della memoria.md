# Programmi e relativa esecuzione

Un programma è un insieme di istruzioni macchina, dati, valori di configurazione e controllo.
Per essere eseguito occorre caricare il programma in una parte di **memoria accessibile al processore**.

```ad-note
title: Loader
All'interno dei sistemi operativi più grandi (ovvero quelli comuni su pc) i file eseguibili, ovvero i programmi, sono portati dal disco in memoria RAM da un modulo chiamato **loader**.

```

Una volta il memoria il programma è eseguito: preleva dalla memoria le operazioni e le esegue tramite il modello base **fetch, decode, execute**.
Per accedere a una specifica parte della memoria viene usato un **indirizzo**, ovvero un offset dall'indirizzo base.

### Spazio di indirizzamento

Un programma è eseguito all'interno dello **spazio di indirizzamento**, ovvero il sottoinsieme di RAM accessibili dal processore per quel programma.
Possiamo immaginare lo spazio di indirizzamento come un vettore di byte contigui (nella realtà non è così).

```ad-warning
title: Segmentation fault
Se un programma prova a scrivere o leggere una cella di memoria fuori dal suo spazio di indirizzamento il sistema operativo uccide il programma, lanciando l'errore di SEGMENTATION FAULT.

```

Potenzialmente il numero di celle nello spazio di indirizzamento **dipende dal processore**, ma realmente viene limitato dalla RAM disponibile.
Ovviamente gli indirizzi nel codice sono **virtuali**: uno specifico blocco hardware **MMU** (Memory management unit) traduce gli indirizzi virtuali in fisici.

Usando linguaggi di programmazione ad alto livello rendono questi meccanismi invisibili, tuttavia è richiesto che il linguaggio introduca delle sovrastrutture e da ciò deriva l'introduzione di vincoli di cui bisogna essere coscienti.

E' organizzato nel seguente modo:
- **Codice eseguibile**
- **Costanti**
- **Variabili globali**
- **[[#Stack]]**
- **[[#Heap]]**
### Modello di esecuzione

Il **modello di esecuzione** è diverso da linguaggio a linguaggio, ma è compito del **compilatore** implementare tale modello: per generare le istruzioni il compilatore fa uso di **runtime library**.

![[Pasted image 20240304132219.png]]

Queste librerie di esecuzioni permettono di offrire agli applicativi i meccanismi base per il loro funzionamento (ad esempio una `fopen` per aprire file).
Sono composte da due tipi di funzioni:
- Quelle invisibili al programmatore (per il controllo stack, delle aree di memoria ecc...)
- Quelle per offrire funzionalità standard (malloc...)

Nel linguaggio C/C++ il modello ha le seguenti proprietà:
- I programmi sono pensati come fossero gli unici utilizzatori dell'elaboratore, i conflitti sono evitati grazie al SO
- Il programma assume di poter accedere a qualsiasi indirizzo di memoria nello spazio di indirizzamento
- Non ci sono limiti al numero di istruzioni da eseguire, ne sul tempo per eseguirle
- Sono presenti strutture dati per gestire chiamate annidate e uso della memoria

### Stack

E' un blocco di memoria allocato automaticamente all'avvio del programma. Contiene la storia delle chiamate a funzioni, dei parametri e dei valori di ritorno.

Ha una dimensione finita, quindi pone un limite alle chiamate ricorsive. Nel caso in cui la spazio dello stack dovesse esaurirsi allora il programma termina con l'errore **stack overflow**.

Quando chiamiamo una funzione:
1. Lasciamo uno spazio per il valore da ritornare
2. Salviamo nello stack i parametri passati alla funzione
3. Entriamo nella funzione
4. Salviamo immediatamente l'indirizzo di ritorno
5. Salviamo nello stack eventuali variabili locali alla funzione
6. Eseguiamo la funzione e salviamo nello spazio lasciato il valore di ritorno.

![[Pasted image 20240304134936.png]]

All'uscita della funzione lo stack si contrarrà lascio solo l'informazione del main e il suo valore di ritorno. Notiamo inoltre che lo spazio iniziale dello stack è riservato al valore di ritorno del main.

### Heap

Tutte le volte in cui un dato ha un ciclo di vita non collegato al tempo di esecuzione
della funzione in cui è stato creato o la cui dimensione è cospicua oppure non nota
in fase di compilazione, occorre richiederne la memorizzazione in una struttura a
parte: lo **heap**.

I dati all'interno dell'heap sono accessibili **solo tramite puntatore**.
E' compito del programmatore liberare la memoria, altrimenti si verifica un **memory leak** che porta il SO a uccidere il processo.
Occorre fare attenzione a rilasciare le risorse **solamente una volta** per non cancellare cose scritte da altri processi.

![[Pasted image 20240304141811.png]]

Si deve prestare attenzione: dopo la liberazione dello heap il puntatore rimane utilizzabile, ma porta a una zona di memoria di cui non sappiamo nulla.

### Variabili

Le variabili **globali** hanno un indirizzo **fisso**, determinato dal compilatore e dal linker, così sono sempre disponibili in qualsiasi punto del codice.
Il ciclo di vita coincide con il tempo di esecuzione del programma.

Le variabili **locali** iniziano ad esistere dalla riga in cui sono definite, hanno un indirizzo relativo alla cima dello stack pointer: in particolare hanno un valore inziale casuale perchè dipende da cosa c'era prima nello stack a quella posizione.
Il loro ciclo di vita è confinato al loro **scope**.

Le variabili **dinamiche** sono accessibili solo tramite puntatori e il loro ciclo di vita è controllato dal programmatore tramite free/malloc. Vengono chiamati rispettivamente il costruttore (in modo esplicito dal programmatore o dal compilatore) e il distruttore (dal compilatore) per rilasciare le risorse.
E' necessaria la presenza di funzioni di libreria e il supporto del SO per implementare correttamente questi meccanismi.

# Puntatori

Sono strumenti che permettono di indirizzare le aree di memoria, e sono molto utili per il passaggio di parametri alla funzione (specialmente se si tratta di parametri di una certa grandezza). Inoltre implementano in modo comodo numerose strutture dati e l'uso di memoria dinamica.
Il loro svantaggio è che potrebbero generare undefined behavior:
- Contiene un indirizzo valido?
- Quanto è grosso il blocco puntato?
- Fino a quando è garantito l’accesso?
- Se ne può modificare il contenuto?
- Occorre rilasciarlo?
- Lo si può rilasciare o altri conoscono lo stesso indirizzo?
- Viene usato come modo per esprimere l'opzionalità del dato?

Una loro prima utilità è quella di **ritornare più risultati da una funzione**:

```c
bool f(int* result){
	*result = get_some_data();
	if (*resutl > 0) return true;
	return false;
}
```

Oppure possiamo usare i puntatori per** accedere a blocchi di dati** come stringhe, array o matrici:

```c
const char* ptr = "Quel ramo del lago di Como...";
//conta gli spazi
int n=0;
//usa l'aritmetica dei puntatori
for (int i=0; *(ptr+i)!=0; i++) {
//usa il puntatore come fosse un array
	if ( isSpace(ptr[i]) ) n++;
}
```

Posso usare i puntatori per **accedere dinamicamente a dati** memorizzati all'interno dell'**heap**:

```c
int* ptr= new int[size];
consume_data(ptr, size);
```

Ultimo utilizzo è quello di **implementare strutture dati complesse**:

```c
struct simple_list {
	int data;
	struct simple_list *next;
};

struct simple_list *head;
```

### Responsabilità del programmatore

- Limitare gli **accessi a un blocco** sia nello spazio (non accedere ai blocchi adiacenti) e nel tempo (non accedere a dati puntati dopo il loro ciclo di vita).
- Non assegnare a puntatori valori corrispondenti ad indirizzi non mappati 
- Rilasciare sempre la memoria dinamica allocata (o si generano memory leak)

### Dangling pointer

Il problema del dangling pointer si verifica se si prova ad accedere a una dato tramite puntatore al di fuori del suo scope:

![[Pasted image 20240305140252.png]]

### Memory leakage

![[Pasted image 20240305140638.png]]

Se non libero lo spazio dello heap posso saturare lo heap, inoltre avendo perso il puntatore non posso sapere dove sta la stringa scritta.

### Double free

Se capita di avere due puntatori che fanno riferimento alla stessa zona, occorre ricordarsi di fare la free solamente una volta, altrimenti potrei cancellare dati.

![[Pasted image 20240305140838.png]]

### Gestione avanzata

Definiamo il concetto di **possessore**, che indica colui che ha il dovere di garantire il rilascio (ovvero ne è responsabile). Se però ho una copia del seguente puntatore, chi è il possessore del blocco dati?
Il vincolo di rilasciare un blocco è ambiguo a causa dei linguaggi come C/C++, perchè non esistono i concetti di possessione.
Inoltre in strutture dati complessi è comune appoggiarsi a blocchi dati allocati dinamicamente, chiamate **dipendenze**.
Ancora una volta i linguaggi come C/C++ non hanno supporti per queste problematiche.

Per far fronte a queste problematiche si può ricorrere all'uso di **linguaggi memory safe**, come appunto [[Rust]].