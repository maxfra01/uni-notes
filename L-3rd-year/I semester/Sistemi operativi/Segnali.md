# Definizioni
---
Un **Segnale** è un interrupt software, una notifica inviata dal kernel o da un processo, verso un altro processo per avvisare quest'ultimo del verificarsi di un dato evento.

Servono prinicipalmente per la gestione di eventi **asincroni** e per la comunicazione fra processi.

Grazie a POSIX i segnali sono standardizzati e affidabili, iniziano tutti con "SIG" e sono definiti nel file "signal.h", rintracciabile con `whereis signal.h`.

![[Pasted image 20221027122141.png]]

# Gestione dei segnali
---
Tipicamente consideriamo 3 fasi:

| Generazione —>                                                                       | Consegna     —>                                                                                            | Gestione del segnale                                            |
| ---------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------- |
| Il segnale si genera quando il processo o il kernel effettuano l'evento desiderato | Il tempo di vita del segnale è dalla generazione alla consegna, se non viene conseganto risulta pendente | Il segnale viene ricevuto e abbiamo 3 diversi modi di procedere |  

Per la gestione del segnale possiamo:
- impostare un comportamento di default 
- ignorare il segnale
- gestirlo (effettuando il "catch") attraverso il **signal handler**

Le syscall che permettono ciò sono: `signal`, `kill`, `raise`, `pause`, `alarm`

## Syscall Signal

```
#include "signal.h"

void (*signal (int sig, void (*func)(int))) (int);
```

Questa syscall permette di istanziare un handler di segnale:
- il parametro `sig` indica il segnale da intercettare
- la `func` specifica l'indirizzo della funzione da invocare se si presenta il segnale, essa non ritorna nulla, ed ha un solo parametro int.

In caso di successo la signal ritorna il puntatore al signal handler, nel caso di errore ritorna SIG_ERR.

> parte da completare


## Syscall kill

```
#include "signal.h"

int kill(pid_t pid, int sig);
```

Questa syscall permette di inviare il segnale `sig` al/ai processi identificati da `pid`.
Attenzione: occorre avere i diritti, ad esempio un processo utente può mandare segnali solo a processi con lo stesso UID.
Il superuser può fare come vuole :)

![[Pasted image 20221027135355.png]]

Valore di ritorno: $0$ in caso di successo, $-1$ in caso di insuccesso.

>Inviare il segnale $0$ ovvero `sig=0` equivale a verificare se un dato processo esiste:
>Il segnale 0 indica di non fare nulla; se dunque ritorna 0 significa che il processo ha ricevuto il segnale e non fatto nulla, se ritorna -1 significa che quel processo non esiste.


## Syscall Raise

```
#include "signal.h"

int raise(int sig);
```

La Raise permette a un processo di mandarsi da solo un segnale.
Di fatto equivale a `kill(getpid(),sig);`


## Syscall Pause

```
#include "unistd.h"

int pause(void);
```

La pause sospende il processo corrente fino alla ricezione di un nuovo segnale.
La funzione ritorna $-1$ solo quando un gestore di segnali termina la sua esecuzione.

## Syscall Alarm

```
#include "unistd.h"

unsigned int alarm(unsigned int seconds);
```

La syscall permette di generare un countdown di `seconds` secondi, alla fine del quale viene generato (dal kernel) il segnale di SIGALRM.

Se si chiama alarm mentre ce n'è una in esecuzione che non ha terminato il countdown, questo ricomincia con il nuovo valore.

Se si chiama la funzione con seconds=0, la chiamata precedente semplicemente termina.

Valore di ritorno:
- Il numero di secondi rimasti prima dell’invio del segnale se ci sono state chiamate precedenti
- Il valore $0$ se non c'erano chiamate precedenti


# Limiti dei segnali
---
Esistono 3 problematiche principali legate ai segnali:
- La memoria dei segnali pendenti è limitata
- Richiedono funzioni rientranti
- Creano **Race conditions**

### Memoria dei segnali pendenti
Si può avere un solo segnale pendente per ogni tipo (inviato ma non consegnato).

### Funzioni rientranti
Normalmente la gestione del segnale è divisa nelle 3 fasi sopra citate: il kernel sa dove viene interrotto il normale flusso di istruzioni, mentre il signal handler non ne è a conoscenza.
Questo da origine a un problema, ovvero che il signal handler potrebbe eseguire operazioni incompatibili con il flusso originale (interrompere una malloc e farne un'altra diversa).

Sono nate perciò una famiglia di funzioni dette **rientranti (reentrant)** che possono essere interrotte senza problemi (read, write, sleep, wait).

```ad-note
La maggior parte delle funzioni I/O standard di C non sono rientranti, ad esempio  una chiamata interrotta a printf può dare risultati inaspettati.
```

### Race Conditions
Il comportamento tra processi che lavorano su dati comuni dipende dall'ordine di esecuzioni, in sostanza esitono problemi di precedenza temporali.

La programmazione concorrente è soggetta particolarmente a problemi del genere.