# File
---
Memorizzano informazioni a lungo termine indipendentemente da alimentazione, terminazione di [[Processi]] ecc...
Logicamente un file è visto come:
- Insieme di informazione correlate tramite un sistema di **codifica**.
- Spazio di indirizzamento continuo

Esistono diverse codifiche più o meno adatte a ciò che si vule fare, ad esempio ASCII, UNICODE ecc...

Un file in pratica è costituito da una serie di byte scritti l'uno dopo l'altro ma sostanzialmente distinguiamo:
- File di testo, come i sorgenti (.c, .cpp)
- File binari (eseguibili, .xlsl)
Notiamo che il kernel Linux **non fa** questo tipo di distinzione.


### File di testo
I file sono **Line Oriented** cioè è possibile andare a capo per migliorare il formato, ma il carattere 'a capo' è differenza a seconda dell'OS che si usa.

### File binari
Possono include ogni possibile sequenza di 8 bit, dunque la gestione del file è molto difficile, non è possibile usare un editor standard.
In compenso però sono facili da modificare, e facile posizionarsi su una porzione di file e sono compatti.

### Serializzazione
Processo di traduzione di una struttura (ad esempio una struct in C) in un formato memorizzabile.
L'entità ora può essere trasmessa e se il ricevitore prende l'accordo sulla serializzazione esso può ricostruire l'entità i maniera identica.


# Input/Output UNIX
---

Si utilizzerà la libreria POSIX che permette l'IO secondo solo 5 funzioni:
- open
- read
- write
- lseek
- close
Ognuna di queste funzioni corrisponde a una **system call**.
Si dice quindi **unbuffered I/O** perchè ad ogni chiamata di read o write corrisponde a una system call.

**FILE DESCRIPTOR** è un intero non negativo (a differenza del C dove è un puntatore) e normalmente lo STDIN è 0, STDOUT è 1 e STRERR è 2.
Per usarli includiamo nel codice ``unistd.h

### OPEN

```c
int open (const char *path, int flags, mode_t mode);
```

In caso di successo ritorna il descrittore del file, o -1 nel caso di errore (che va sempre controllato!)
- path indica il file da aprire
- mode specifica i diritti di accesso (   S_I[RWX]USR $\implies$ rwx — —   )
- flags indica molteplici opzioni (O_RDONLY, O_WRONLY, O_RDWR...)

```ad-note
La open accetta sia 2 o 3 parametri alla chiamata, in quanto il campo mode è opzionale
```

### READ

```c
int read (int fd, void *buf, size_t nbytes);
```

Legge dal file ``fd`` un numero di bytes pari a `nbytes` e li memorizza in `buf` 
Il valore di ritono è
- numero di byte letti in caso di successo
- -1 in caso di errore
- 0 in caso di EOF

### WRITE

```c
int write(int fd, void *buf, size_t nbytes);
```

Scrive `nbytes` contenuti in `buf` nel file con descrittore `fd`
Valori di ritorno:
- normalmente `nbytes` ovvero il numero di byte scritti
- -1 in caso di errore

### LSEEK

```c
off_t lseek (int fd, off_t offset, int whence);
```

Ogni file ha associata una posizione corrente del file offset, e questa posizione è quella che indica il punto di partenza per la prossima operazione di read/write.
La funzione lseek assegna il nuovo valore `offset` al file offset.

Whence è un parametro che dice da dove interpretare l'offset:
- SEEK_SET, allora valuto dall'inizio
- SEEK_CUR, dalla posizione corrente
- SEEK_END , valutato dalla fine del file

Valori di ritorno:
- il nuovo offset in caso di successo
- -1 in caso di errore

### CLOSE

```c
int close(int fd);
```

Chiude il file con descrittore `fd` 
Valori di ritorno:
- 0 per successo
- -1 per errore


# Direttori
---
Un direttorio è un nodo(di albero) o vertice (di grafo) contenente informazioni su elementi in esso contenuti.
La struttura di una directory dipende da:
- Efficienza
- Naming (semplicità per l'utente di identificare i file)
- Grouping (raggruppare file di categorie simili)

### Direttori a un livello

![[Pasted image 20221013221716.png]]

Tutti i file del filesystem sono contenuti all'interno dello stesso direttorio, i file sono differenziati solo dal nome.
- entry: nome ed altre info sui file
- dati: sono individuati dall'entry tramite puntatore

Prestazioni:
- efficienza, semplice ed efficacie
- naming, i nomi devono essere univoci, scomodo
- grouping, complesso, per utenti multipli impossibile

### Direttori a due livelli

![[Pasted image 20221013222021.png]]

Ogni utente ha la sua home directory
Prestazioni:
- efficienza, semplice agendo su utenti singoli
- naming, possibile avere file con nomi uguali ma utenti diversi, occorre specificare il path
- grouping, semplice per diversi utenti, scomodo per singolo

### Direttori ad albero

![[Pasted image 20221013222210.png]]

Generalizzazione dei precedenti, possono esistere per ogni nodo direttori e sotto-direttori.

Prestazioni:
- efficienza, ricerche legate alla complessità dell'albero
- naming, bello esteso
- grouping, bello esteso

### Direttori a grafo aciclico

I direttori ad albero non permettono la condivisione di file, perciò soluzioni come copie di file vanno a generare problemi di spazio e versione della copia.
Nasce quindi il concetto di grafo ciclico che si basa sulla presenza di link.

![[Pasted image 20221013222521.png]]

La complessità diventa alta a seconda dei link e dei meccanismi per gestirli.
Inoltre la presenza di link potrebbe generare cicli.

### Direttori a grafo ciclico

![[Pasted image 20221013222916.png]]

Complessità massima, un elemento potrebbe autorefenziarsi e non essere rilevati o ineliminabili.

# Allocazione
---
Per **allocazione** intendiamo le tecniche di utilizzo del blocchi dei dischi per la memorizzazione dei file.
Esistono diverse metodologie:
- Continua (contiguous)
- Concatenata (linked)
- Indicizzata (indexed)

### Allocazione continua

![[Pasted image 20221013230005.png]]

Ogni file occuopa un insieme continuo di blocchi, e per ciascuno di essi si specificano l'indirizzo di partenza e la lunghezza del file.
Vantaggi:
- E' una strategia molto semplice, che permette facili accessi sequenziali.
Svantaggi:
- Occorre decidere come scegliere lo spazio per allocare
- Difetti in ogni algoritmo di allocazione quindi c'è spreco di memoria (**frammentazione esterna**)
- I file non possono espandersi più di tanto perchè lo spazio finisce

### Allocazione linkata

![[Pasted image 20221013225951.png]]

Ogni file è allocato gestendo una lista di concatenzaione di blocchi.
Per ogni file abbiamo il puntatore alla testa e quello della coda.
Vantaggi:
- Risolve i problemi dell'allocazione continua
Svantaggi:
- Ogni lettura implica un accesso sequenzale ai blocchi
- Memorizzazione dei puntatori
- Risulta efficiente solo per accessi sequenziali.

Introduciamo il concetto di **FAT (File Allocation Table)**, ovvero una variante del metodo di allocazione concatenato.
In questa tecnica i puntatori ai blocchi successivi non si trovano nel blocco stesso, bensì sono tutti raggruppati appunto nella FAT.
Questo però comporta una serie di svantaggi:
- lentezza dovuto ai continui accessi in memoria e in FAT
- affidabilità critica, se perdo la FAT sono fregato!
- dimensione della fat critica

### Allocazione indicizzata
Puntatori organizzati in una tabella di puntatori detta **Blocco indice, i-node**, ogni file dunque ha la sua tabella (che sostanzialmente è un vettore)
L'i-esimo elemento di questo vettore individua l'i-esimo blocco del file.

![[Pasted image 20221013225934.png]]

Per ogni file è dunque necessario allocare anche il blocco indice, ma le sue dimensioni possono essere critiche se troppo grandi o troppo piccole.
Nei sistemi UNIX si usa la variante **combinata** dell'allocazione indicizzata.

Ogni file ha associato un i-node contenente 15 puntatori:
- i primi 12 sono diretti a blocchi dei file
- 13,14,15 contengono puntatori indiretti con livello di indirizzamento crescente.

