# IPC InterProcess Communication
---
I [[Processi]] sono **Indipendenti** se non possono essere influenzati da altri e non influenzano altri processi.
I processi **cooperanti** scambiano e condividono i dati, e richiedono opportuna sincronizzazione.

La comunicazione è basata su memoria condivisa e scambio di messaggi.

## Memoria condivisa
Basata su condivisione di una zona di memoria.
Di base l'SO impedisce a un processo di accedere a memoria di un altro, dunque abbiamo due possibili strategia:
- **condivisione di File**, tramite puntatore, nome, o descrittore
- File **Mappati** (non visti nel corso) ovvero ogni processo associa una regione della propria memoria centrale a un file.

## Modello di comunicazione

Prima opzione con scambio di messaggi, occorre dunque instaurare un canale e utilizzare una syscall.

Il canale di comunicazione ha diverse categorie:
- naming: Comunicazione diretta, specificando il destinatario oppure comunicazione indiretta tramite casella di posta comune ad ex.
- Sincronizzazione: sia l'invio che la ricezione può essere sincrona o asincrona
- Capacità: La coda utilizzata per la comunicazione è di lunghezza pari a 0, limitata o infinita.

Le metodologie di canali di comunicazione più comuni previste da UNIX sono: **Half-duplex pipes** e i **Semafori**.


# Pipe
---
Una pipe consiste in un **flusso di dati** tra due processi.
Sono gestite in maniera simile a un file e individuate da due descrittori, uno per ciascun estremo.

![[Pasted image 20221103163204.png]]

Dato il canale **half-duplex** non è possibile da parte di p1 e p2 di scrivere allo stesso tempo.

E' possibile far comunicare due processi con un **parente comune**.

## Syscall pipe

```c
#include "unistd.h"

int pipe (int fileDescr[2]):
```

Normalmente dopo una chiamata a pipe, si crea una pipe al processo stesso, uno in scrittura e uno in lettura.
Per  mettere in comunicazione due processi si fa normalmente una fork.
Il figlio eredita i descrittori del file e si ottiene il seguente risultato.

![[Pasted image 20221103163952.png]]

Uno dei due descrittori può essere tranquillamente chiuso per ottenere una comunicazione da padre a figlio.

Le pipe si manipolano con le syscall read e write, si utilizza tipicamente un unico scrittore e un unico lettore.

La `write` è atomica di fatto, ma può essere interrotta nel caso di una pipe quasi piena, scrivendo solo parzialmente dei dati.
In presenza di più lettori risulta indeterminato chi legge.

La `read` è bloccante se la pipe è vuota, se invece la pipe è chiusa ritorna 0

# Pipe da shell e redirezione
---
A livello di Shell una pipe può essere creata con il carattere '|' e quest'ultima risiede completamente in memoria, e di fatto è più veloce che la condivisione di file.

Di fatto si viene a creare un collegamento fra lo stdout del primo comando e lo stdin del secondo e così via...

```bash
ls -laR .c* | wc
```

Il termine redirezione indica l’associazione dei dispositivi standard a unità logiche diverse, normalmente all'avvio delle shell abbiamo:
- stdin ha un file descriptor 0
- stdout 1
- stderr 2

```ad-hint
Nel file `/dev/null` è possibile scrivere byte senza toccare la memoria, oppure è possibile leggere una serie di 0.
``` 

## Redirezione dello stdin

```bash
comando < file
```

In questo modo il comando prenderà lo stdin da file.

## Redirezione dello stdout

```bash
comando > file
comando >> file
```

Nel primo caso lo stdout cancella il contenuto del file, mentre con >> questo non accade. Notiamo che possiamo anche scrivere `1>` ma l'uno si omette.

Lo stderr è uguale a stdout

## Redirezione multipla
E' possibile redirezionare allo stesso tempo stdout e stder con il comando:

```bash
comando &> file
comando &>> file
```

Oppure in maniera separata con

```bash
comando 1> file1 2> file2
```
