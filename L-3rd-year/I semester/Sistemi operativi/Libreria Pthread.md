# Pthreads
---
Per la gestione dei [[Thread]] in C la libreria più utilizzata è la POSIX Threads, e per utilizzare occorre includerla e compilare inserendola nel comando ( o l'opzione -o).

```bash
#include <pthreads.h>

gcc -Wall -o myexe -pthread file.c
```

Un thread diventa una particolare funzione che viene eseguita in maniera indipendente dal resto del programma.

Ogni thread è identificato da un TID, di tipo `pthread_t`, però il tid non è globale come il pid ma ha significato solo nel suo ambito d'uso.
I TID sono opachi, vanno confrontati con specifiche funzioni.

## Equal

```c
int pthread_equal (pthread_t tid1, pthread_t tid2);
```

Ritorna 0 se sono diversi e ritorna !=0 se sono uguali

## Self

```c
pthread_t phread_self (void);
```

Ritorna il tid del thread che l'ha chiamata.

## Create

```c
int pthread_create ( pthread_t *tid, 
const pthread_attr_t *attr,
void *(*startRoutine)(void *),
void *arg
);
```

Il primo parametro è il puntatore al tid restituito.
Attributi del thread (=NULL nel nostro caso).
Funzione di thread
Argomenti passati alla funzione di thread.

Ritorna 0 in caso di successo e un codice di errore in caso di fallimento.

## Exit
Un thread termina se si fa una return, se si fa un pthread_Exit o se riceve da un altri thread la pthread_cancel

```c
void pthread_exit (void *valuePtr);
```

Permette a un thread di terminare restituendo il suo stato di terminazione.
Il valore `valuePtr` è mantenuto dal SO fino a quando non si effettua una pthread_join.

## Join
Alla sua crezione un thread può essere dichiarato:
- joinable, ovvero che un altro thread può raccogliere il suo stato di terminazione tramite una wait
- detached, se non è joinable e dunque non è esplicitamente attendibile la sua terminazione.

Se dunque un threa effettua una join, esso rimane bloccato finche un altro non fa una exit e quindi fornisce lo stato di terminazione.

```c
int pthread_join (pthread_t tid, void **valuePtr);
```

Il tid è quello del tid atteso, mentre il puntatore da informazioni sulla terminazione del thread atteso (PTHREAD_CANCELED se è stato cancellato)

La join ritorna 0 in caso di successo o un errore in caso di fallimento (EINVAL o ESRCH).
Se il thread atteso era detached allora dovrebbe automaticamente fallire.

## Cancel

```c
int pthread_cancel(pthread_t tid);
```

Cancella il thread con il corrispondente tid, equivalente a una exit con valore PTHREAD_CANCELED.
Il thread chiamante non attende la chiusura ma va avanti.

Ritorna 0 per successo, errore per fallimento.

## Detach

```c
int pthread_detach(pthread_t tid);
```

Dichiara il thread tid come detach, dunque la memoria mantenuto per esso viene rilasciata al SO e non sarà più possibile chiamare una join su tale thread.

Ritorna 0 per successo, errore per fallimento.

