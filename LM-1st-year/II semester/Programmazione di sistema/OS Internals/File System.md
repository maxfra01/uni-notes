# Interfaccia

Un file può essere visto come uno **spazio di indirizzamento contiguo**, a cui è associato un tipo (file di dati, o istruzioni). L'interpretazione di un file è definita anche dall'utente (un binario può essere testo, eseguibile o sorgente).

Gli **Attributi** che descrivono il file sono:
- Il nome (che lo rappresenta o lo descrive)
- Un identificatore unico
- Un tipo (informazione necessaria al OS per gestire)
- Posizione su disco
- Dimensione
- Protezione
- Altre informazioni su permessi e tempo
- ...

![[Pasted image 20240527085729.png]]

Per modificare un file occorre una **open file table**: per gestirne uno dobbiamo eseguire l'operazione di open, che restituisce un puntatore all'ultima lettura o scrittura del file stesso.
- **Conteggio di apertura del file**: contatore del numero di volte in cui un file è aperto – per permettere la rimozione dei dati dalla tabella dei file aperti quando l'ultimo processo lo chiude.
- **Posizione del file sul disco**: cache delle informazioni di accesso ai dati.
- **Diritti di accesso**: informazioni sulla modalità di accesso per processo.

### Lock

Il meccanismo di **locking** (blocco) dei file viene utilizzato da alcuni sistemi operativi e file system per gestire l'accesso concorrente ai file.
- **Fornito da alcuni sistemi operativi e file system**: Non tutti i sistemi operativi e file system supportano questa funzionalità, ma alcuni lo fanno per migliorare la gestione dei file in contesti di accesso concorrente.
- **Simile ai lock lettore-scrittore**: I lock lettore-scrittore sono un tipo di meccanismo di sincronizzazione che permette a più lettori di accedere a una risorsa contemporaneamente, ma limita l'accesso a un solo scrittore alla volta.
- **Il lock condiviso è simile al lock lettore**: Un lock condiviso consente a più processi di accedere contemporaneamente al file in modalità di sola lettura, senza interferire l'uno con l'altro.
- **Il lock esclusivo è simile al lock scrittore**: Un lock esclusivo permette solo a un processo di accedere al file, generalmente per scrivere, impedendo così l'accesso ad altri processi finché il lock non viene rilasciato.
- **Media l'accesso a un file**: Il meccanismo di locking regola e controlla l'accesso ai file, prevenendo conflitti e garantendo la coerenza dei dati.
- **Obbligatorio o consultivo**:
	- **Obbligatorio**: In questo caso, il sistema operativo forza il rispetto dei lock. Se un file è bloccato, l'accesso viene negato a seconda dei lock già presenti e di quelli richiesti.
	- **Opzionale**: Qui i processi sono liberi di verificare lo stato dei lock e decidere autonomamente come comportarsi. Non c'è un'imposizione da parte del sistema operativo.

Questo meccanismo è essenziale per garantire che i file non vengano corrotti o modificati in modo incoerente quando vengono acceduti da più processi contemporaneamente.

### Struttura di un file

Diverse tipologie a seconda dei casi:
1. **Nessuna**:
    - Il file è una semplice sequenza di parole o byte senza una struttura specifica. Questo tipo di file è il più semplice e flessibile, ma richiede che il programma sappia come interpretare i dati.
2. **Struttura di record semplice**:
    - **Linee**: Il file è organizzato in linee di testo, come in un file di testo normale.
    - **Lunghezza fissa**: Ogni record (unità di dati) ha una lunghezza fissa, facilitando l'accesso casuale ai record.
    - **Lunghezza variabile**: I record possono avere lunghezze diverse, permettendo una maggiore flessibilità ma complicando l'accesso diretto ai record.
3. **Strutture complesse**:
    - **Documento formattato**: Il file contiene dati formattati secondo uno schema specifico, come i documenti di elaborazione testi con font, stili e layout.
    - **File di caricamento rilocabile**: File binari utilizzati per caricare programmi in memoria in modo che possano essere eseguiti, con riferimenti che possono essere risolti a diversi indirizzi di memoria.

La decisione della struttura spetta o al SO, nel caso di file di sistema o standardizzati, o a programma, per facilitare le necessità dell'applicazione.

### Tipi di accesso al file

1. **Sequenziale**: ![[Pasted image 20240527090141.png]] Un altro modo per vedere la lettura sequenziale è la seguente: ![[Pasted image 20240527090300.png]]
2. **Accesso diretto**:![[Pasted image 20240527090209.png]]

Altri metodi di accesso necessitano di introdurre un **indice**: serve per un veloce look up della memoria per accedere alle informazioni:

![[Pasted image 20240527090504.png]]

### Direttori

Nodi che permettono di raggruppare dei file: nella foto possiamo vedere una struttura a direttorio di un solo livello:

![[Pasted image 20240527090531.png]]

L'organizzazione dei direttori deve rispettare 3 condizioni in modo logico:
- **Efficienza**
- **Naming** (dipende dall'utente, infatti due utenti diversi sulla stessa macchina possono dare nomi diversi allo stesso file)
- **Grouping**

In una organizzazione di primo livello sorgono i problemi di naming e di grouping.
In una struttura a secondo livello il path sarà diverso per ogni utente, perciò si deve implementare una ricerca in modo efficiente.

![[Pasted image 20240527091227.png]]

Nel caso di una organizzazione ad **albero** si introducono i concetti di **path relativi e assoluti** e di **current directory**.

Un altra tipica organizzazione è quella **aciclica**, dove i file condivisi sono foglie dell'albero.
Tuttavia nel caso di eliminazione di un file si possono creare dangling pointer: nasce un nuovo concetto di **Link** e di operazione **resolve link** che consiste nel seguire il collegamento fino all'obiettivo.

In generale i file system a grafo sono diffusi ma come evitiamo i cicli?:
- Per costruzione
- Tramite garbage collector

Prima di utilizzare un disco occorre fare **mount**.

### Condivisione dei File

- **La condivisione dei file nei sistemi multi-utente è desiderabile**: Permette agli utenti di collaborare e accedere ai file condivisi.
- **La condivisione può essere realizzata attraverso un sistema di protezione**: Si utilizzano permessi e autorizzazioni per controllare chi può accedere e modificare i file.
- **Nei sistemi distribuiti, i file possono essere condivisi attraverso una rete**: Questo permette agli utenti di accedere ai file su sistemi remoti.
- **Il Network File System (NFS) è un metodo comune per la condivisione dei file distribuita**: È uno standard per la condivisione dei file in ambienti UNIX.
#### Sistema Multi-Utente

- **Gli ID utente identificano gli utenti, permettendo permessi e protezioni per utente**: Ogni utente ha un ID univoco che viene utilizzato per gestire i permessi di accesso ai file.
- **Gli ID di gruppo permettono agli utenti di essere in gruppi, consentendo diritti di accesso di gruppo**: Gli utenti possono essere raggruppati per semplificare la gestione dei permessi.
- **Proprietario di un file/directory**: L'utente che ha creato o possiede il file/directory.
- **Gruppo di un file/directory**: Il gruppo associato al file/directory che può avere diritti di accesso specifici.

### Condivisione dei File – Sistemi di File Remoti

- **Usa la rete per consentire l'accesso al file system tra sistemi**:
    - **Manualmente tramite programmi come FTP**: File Transfer Protocol permette il trasferimento manuale di file tra sistemi.
    - **Automaticamente e senza soluzione di continuità utilizzando file system distribuiti**: Permettono l'accesso automatico ai file remoti come se fossero locali.
    - **Semi-automaticamente tramite il World Wide Web**: Accesso ai file tramite il web.
    
- **Il modello client-server permette ai client di montare file system remoti dai server**:
    - **Il server può servire più client**: Un server può fornire accesso ai file a diversi client contemporaneamente.
    - **L'identificazione del client e dell'utente sul client può essere insicura o complicata**.
    - **NFS è il protocollo standard di condivisione file client-server per UNIX**.
    - **CIFS è il protocollo standard di Windows**.
    - **Le chiamate ai file del sistema operativo standard vengono tradotte in chiamate remote**.
    
- **Sistemi di Informazione Distribuiti (servizi di naming distribuiti) come LDAP, DNS, NIS, Active Directory implementano un accesso unificato alle informazioni necessarie per il computing remoto**.
    
### Modalità di Fallimento nella Condivisione dei File

- **Tutti i file system hanno modalità di fallimento**:
    - Ad esempio, corruzione delle strutture delle directory o altri dati non utente, chiamati metadata.
- **I file system remoti aggiungono nuove modalità di fallimento, a causa di guasti di rete, guasti del server**.
- **Il recupero dai fallimenti può coinvolgere informazioni di stato sullo stato di ogni richiesta remota**.
- **I protocolli senza stato come NFS v3 includono tutte le informazioni in ogni richiesta, permettendo un recupero facile ma meno sicuro**.
    
### Semantica di Consistenza nella Condivisione dei File

- **Specifica come più utenti possono accedere simultaneamente a un file condiviso**:
    - **Simile agli algoritmi di sincronizzazione dei processi del Capitolo 5**.
    - **Tendono ad essere meno complessi a causa dell'I/O su disco e della latenza di rete (per i file system remoti)**.

- **Andrew File System (AFS) implementa una semantica complessa di condivisione dei file remoti**.

- **Il file system Unix (UFS) implementa**:
    - Le scritture su un file aperto sono visibili immediatamente ad altri utenti dello stesso file aperto.
    - Condivisione del puntatore del file per permettere a più utenti di leggere e scrivere contemporaneamente.
    
- **AFS ha semantica di sessione**:
    - Le scritture sono visibili solo alle sessioni che iniziano dopo che il file è stato chiuso.

### Protezione

- **Il proprietario/creatore del file dovrebbe essere in grado di controllare**:
    - Cosa può essere fatto.
    - Da chi.

- **Tipi di accesso**:
    - Lettura.
    - Scrittura.
    - Esecuzione.
    - Aggiunta.
    - Cancellazione.
    - Elencazione.

### Liste di Accesso e Gruppi

- **Modalità di accesso: lettura, scrittura, esecuzione**.
    
- **Tre classi di utenti su Unix/Linux**:
    - **Proprietario del file (RWX)**: Accesso completo (lettura, scrittura, esecuzione).
    - **Gruppo del file (RWX)**: Accesso per il gruppo associato (ad esempio, lettura e scrittura ma non esecuzione).
    - **Accesso pubblico (RWX)**: Accesso per tutti gli altri utenti (ad esempio, solo lettura

- **Chiedere al manager di creare un gruppo (nome unico), ad esempio G, e aggiungere alcuni utenti al gruppo**.
    
- **Per un particolare file (ad esempio, game) o sottodirectory, definire un accesso appropriato**.
    
Questi concetti sono cruciali per la gestione sicura ed efficiente dei file nei sistemi multi-utente e distribuiti, garantendo che i dati possano essere condivisi senza compromettere la sicurezza e l'integrità delle informazioni.

# Implementazione

Il FS di fatto risiede nei dischi della memoria secondaria: fornisce un'interfaccia per vedere tutti i file, che di fatto corrisponde a una traduzione da logico a fisico.
Un singolo file è rappresentato logicamente da un **FILE CONTROL BLOCK FCB** che ne descrive tutte le sue caratteristiche.
I livelli di un File systems sono i seguenti:

![[Pasted image 20240617144727.png]]

In UNIX un FCB è chiamato **inode**:

![[Pasted image 20240617145222.png]]

### Operazioni su File System

Ci sono innanzitutto 2 strutture dati che contengono informazioni su come fare bootstrap: **Boot control block** e **Volume control block**.
La mount table da informazioni su quali file system sono attivi e dove sono agganciati.
La **system wide open file table** tiene conto dei file aperti per tutto il sistema, mentre la **per process open file table** è confinato al singolo processo

![[Pasted image 20240617145439.png]]

- Nel caso di una read, l'operazioni è chiamata in user space. Si parte dal nome del file, e si localizza su disco il FCB del relativo file e lo si porta. Dopodichè il FCB viene copiato nella system wide open file table.
- Se due processi aprono in contemporanea lo stesso file, allora ci sarà una sola copia del FCB nella system wide o.t. mentre nella per process o.t. ognuno avrà la sua istanza.

### Direttori

Due possibili implementazioni:
- Liste lineari di file O(n)
- Hash Table (migliori performance)

### Allocazioni di file: contiguo

![[Pasted image 20240617150306.png]]

### Allocazione di file: lista linkata

![[Pasted image 20240617150617.png]]

Dobbiamo supporre che ogni blocco abbia spazio per sia per i dati che per il puntatore al prossimo blocco

Una sua variante è la FAT: dato che in una linked list, se perdo un blocco perdo anche i successivi, per la FAT si è pensato di separare i dati dai puntatori.

![[Pasted image 20240617150941.png]]

### Allocazione dei file: indicizzato

Usiamo delle tabelle ad accesso diretto per accedere ai blocchi file.
Ogni file è associato a un **blocco indice**:

![[Pasted image 20240617151350.png]]

Nel caso di un file che necessiti di più indici rispetto a quelli che entrano in un blocco indice, allora ho due opzioni:
- Lista di blocchi indici: pago in complessità e performance
- Blocco indice a più livelli


### UNIX UFS

![[Pasted image 20240617151715.png]]

Il FCB (inode) contiene info e poi:
- Puntatori a blocchi dati diretti ( in genere 10, se il file è piccolo bastano loro )
- Un blocco singolo indiretto, uno doppio e uno triplo
Se il blocco singolo indiretto punta a un **blocco indice da 512** blocchi dato allora per riempire il livello doppio ho 512^2 blocchi dato e nel triplo ho 512^3.

### Gestione dello spazio libero

Tipicamente esiste o una free-list o una bitmap (vettore) parallela ai blocchi (0 occupato, 1 libero)
- La bitmap richiede spazio aggiuntivo (hanno, se ben gestite, complessità logaritmiche)
- La free list invece avrà complessità lineare per la ricerca di spazio libero contiguo.