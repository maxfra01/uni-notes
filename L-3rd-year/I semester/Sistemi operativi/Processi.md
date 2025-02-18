# Introduzione ai processi
---
Un processo è l'astrazione di un programma in esecuzione, esso rappresenta un'entità attiva.

I processi si distinguono in **sequenziali e concorrenti**:
- nel primo caso le azioni sono eseguite una dopo l'altra, inoltre hanno un comportamento deterministico, ovvero lo stesso input produce sempre lo stesso output
- Nel caso parallelo più istruzioni possono essere eseguire nello stesso istante, il comportamento non è deterministico. Si parla di concorrenza fittizia(illusoria, nei mono-core) o reale (multi-core).

Ad esempio nella fase di bootstrap vengono attivati numerosi processi, che sono terminati allo shut-down.

Normalmente, da lato utente, è possibile:
- identificare e controllare un processo esistente
- creare un processo
- attendere, sincronizzare e terminare processi in esecuzione

# Identificazione di un processo
---
Ogni processo è identificato univocamente da un **PID (Process Identifier)**.
Quest'ultimo è rappresentato da un intero non negativo.
Anche se è univoco i sistemi UNIX li riutilizza, e possono essere abbinati dal processo per generare oggetti unici.
(file simili ma per distinguerli uso il PID)

Il comando `getpid()` è usato per ritornare il PID del processo chiamante.

Alcuni PID sono riservati, ma tra i due più noti abbiamo:
- $0$ per lo schedulatore dei processi (**swapper**) che viene eseguito a livello kernel.
- $1$ per il processo di `init 1` invocato alla fine del bootstrap, che non termina mai, ed **eredita tutti i processi che rimangono orfani.**

Segue tabella dei comandi per il PID sui processi:

| Comando           | Valore di ritorno                     |
| ----------------- | ------------------------------------- |
| `pid_t getpid()`  | PID del processo chiamante            |
| `pid_t getppid()` | PID del padre del processo chiamante  |
| `uid_t getuid()`  | ID dell'utente del processo chiamante |
| `gid_t getgid()`  | ID del gruppo del processo chiamante  |

Non esiste il comando per ottenere il PID di un processo figlio (ce ne possono essere moltissimi).
In particolare gli ultimi due comandi hanno un corrispettivo **effective** ovvero il processo può cambiare identità assumendo un EUID o EGID.


# Creazione di un processo
---
Nei sistemi UNIX un nuovo processo viene creato dalla system call **fork**. La funzione procede per clonazione del processo padre, ed è una funzione che viene chiamata una volta ma ritorna due volte.

```c
#include <unistd.h>

pid_t fork(void);
```

La fork genera un processo detto **processo figlio**, che è la copia esatta del processo padre tranne per il PID, che è il valore che ritorna la fork:
- il padre riceve l'ID del figlio, così sa chi è
- il figlio riceve il valore $0$, il padre si identifica con `getppid()`
- $-1$ nel caso di errore, se non è possibile allocare un nuovo processo (limite di processi in esecuzione raggiunto)

![[Pasted image 20221017115129.png]]

Nei **CFG (Control Flow Graph)** il triangolo indica la fork, e per convenzione indichiamo a destra il processo figlio e a sinistra il padre.

Ogni nuovo processo è una nuova entry nella tabella dei processi:

| **Padre e figlio condividono:** | **Padre e figlio hanno differenti:**|
| -------------------------- | ------------------------------- |
| Codice sorgente C          | Valori ritornati dalla fork     |
| Descrittori dei file       | PID                             |
| User ID, Group ID          | Spazio dati, heap e stack       |
| Root e working directory   | -                                |
| Risorse del sistema        |  -                               |
| Segnali                    |   -                              |

Nei moderni OS, si usa la tecnica **copy-on-write** e la memoria è duplicata solo per lo stretto necessario.

### Esempio
Si scriva un programma concorrente in grado si generare un figlio, far terminare il processo padre prima del figlio e poi viceversa.
Inoltre per ogni processo che termina stampiamo il PID corrente e del padre.
(useremo la syscall sleep anche se non si dovrebbe mai usare)

Si suppone di avere TC e TF, tempi di attesa di padre e figlio

```c
pid=fork();
if(pid==0){
//siamo nel figlio
	sleep(TC);
	printf("Child, PID: %d", pid);
	printf("PID: %d, my parent: %d", getpid(), getppid());
} 
else{
	sleep(TF);
	printf("Father, PID: %d", pid);
	printf("PID: %d, my parent: %d", getpid(),
}
```

Se lanciamo un programma, notiamo che il padre di questo processo è la shell che l'ha chiamata, vedere stato in shell tramite `ps`.

# Terminazione di un processo
---
Esistono 5 differenti modi  **STANDARD** per terminare un processo:
- Eseguire una `return;` della funzione principale
- Eseguire una `exit;`
- Eseguire una `_exit` o `_Exit`
- Richiamare una `return;` dal main dell'ultimo thread del processo
- Richiamare `pthread_exit` dall'ultimo thread del processo

Esistono 3 modi **ANOMALI** per terminare un processo:
- Usare la funzione `abort;`
- Ricevere un segnale `signal`
- Cancellare l'ultimo thread del processo

Per comprendere a pieno vedere [[Thread]].

Quando un processo termina, il kernel invia il segnale **SIGCHLD** al padre (evento asincrono), il quale può decidere se gestire la terminazione del figlio o ignorarla (scelta di default).

Nel caso si scelga la gestione della terminazione, allora essa può essere sincrona o asincrona.
Nel caso sincrono uso le syscall `wait` e `waitpid`

![[Pasted image 20221020163445.png]]

### wait 

```c
#include <sys/wait.h>

pid_t wait (int *statLoc);
```

Comportamento della wait:
- Se il processo non ha figli, la wait ritornerà un errore pari a $-1$.
- Se il processi ha figli, ma sono tutti in esecuzione allora la wait blocca il processo. Quando uno qualunque dei figli termina allora si sblocca il processo padre e si ritorna al chiamante il PID del figlio e lo stato.
- Se uno dei figli ha già terminato allora la wait ritornerà il PID del figlio e lo stato di questa terminazione.

Il sistema operativo tiene in memoria lo stato e il PID di ogni processo terminato finchè non sono recuperati dal padre.
Il parametro `statLoc` indica lo stato di terminazione del processo figlio terminato. E' un puntatore a intero, e le macro sono specificate nella libreria.

(WIFEXITED è vero se la terminazione è stata corretta)
(WEXITSTATUS cattura gli 8 LSB del parametro passato a exit)


### waitpid

```c
#include "sys/wait.h"

pid_t waitpid(pid_t pid, int *statLoc, int options);
```

Comportamento al variare di pid:
- $-1$ aspetta un qualunque figlio, come wait
- si aspetta il figlio con il PID se $>0$.

Se options ha WNOHANG, allora il chiamante non si ferma se il figlio con il PID non è ancora terminano

# Altre definizioni
---

### Processi Zombie
Un processo terminato per il quale il padre non ha ancora eseguito una wait si dice **zombie**.
Occorre fare la wait per alleggerire il sistema operativo.

### Processi orfani
Se un processo padre termina prima del figlio, allora il figlio diventa un processo **orfano**.
Normalmente i processi orfani vengono ereditati da **init o custom init**.
Dato che init non termina mai, i processi orfani non possono diventare zombie