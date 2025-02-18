# Configuration Management

Durante l'intero sviluppo del progetto occorre mantenere aggiornati i documenti e il codice: numerose persone accedono ai documenti durante lo sviluppo di software. Inoltre è necessario fornire specifiche che determinino chi può avere accesso ai documenti, chi può modificarli, chi è stato l'ultimo a modificarli...
Un secondo motivo per fare CM è quello delle dipendenze: i documenti che andiamo a sviluppare possiedono dipendenze, che vanno rese note a tutti i membri del team ed essere accessibili.

### Configurations Management System (CMS)

- I **Configuration item** (CI) sono unità che rientrano nella gestione della configurazione (file, documenti, specifiche)
- Una **Configurazione** è un insieme di CI, che devono essere consistenti fra di loro
- Un **Repository** è un luogo fisico/logico dove sono posizionati i CI
- Il **Versioning** è la capacità di navigare avanti e indietro fra versioni di CI o di configurazioni
- Il **Change control** è la capacità di mettere/togliere diritti di eseguire azioni sui CI
- Il **Check-out** indica che un CI è stato acceduto da un certo utente (operazione di lettura es. pull)
- Il **Check-in** indica che un CI non è più accessibile, ed è stato modificato (operazione di scrittura es. commit)

![[Pasted image 20240325174832.png]]

Due possibili modelli per il **controllo delle modifiche**:
- **Lock modify unlock**: un primo utente prende il controllo di un CI, nessun altro utente può accedere a quel CI finchè la modifica non è terminata (no parallelizzazione)
- **Copy modify merge**: in parallelo più utenti fanno un check-out di un CI, e lavorano in parallelo. Questo può portare a conflitti in fase di merge

Non tutti i file devono essere "versionati" (vedi .gitignore)


I software che si occupano di versioning sono i **CMS** (Configuration management system) sono in grado di memorizzare e versioning di file, fare revert, confrontare file, tracciare chi ha modificato cosa...

Possono essere di 3 tipi: locali, centralizzati o **distribuiti**.
Possiamo ancora distinguere i CMS in base a come memorizzano i "commit":
- **Deltas**: per un CI iniziale si memorizza l'intero file, per le versioni successive solamente i cambiamenti (meno spazio usato, ma la ricostruzione richiede tempo)
- **Full copies**: Per ogni CI si salva sempre un'intera copia per ogni versione (più spazio usato, ma ricostruzione in tempo zero)

Possiamo ulteriormente fare una distinzione sui CMS:
- Alcuni CMS interpretano il commit come un cambiamento di versione del CI modificato (SVN)
- Altri CMS (git) fanno in modo che un commit definisca un nuovo **snapshot**, composto da tutti i CI nel progetto, con la loro versione. Se un CI non cambia versione si usa un link al precedente.

# Git 

**Git** è un software di versioning **distribuito**, che fa uso di snapshots come modello di configurazione. Supporta operazioni locali che garantiscono velocità e inoltre garantisce **Integrità** e **additività** (tutto è controllato prima di essere modificato, l'informazione è sempre aggiunta e mai eliminata).

### Workflow client side

La working directory è la cartella dove stiamo lavorando localmente: i file sono **untracked** (ovvero non monitorate da git) e possono essere resi **staged** tramite `git add <nome file>`. Quando un file è "staged" si trova nella **working copy (staging area)**, dove i file sono pronti per essere sottoposti a commit, ma non ancora versionati.
Tramite l'operazione di commit possiamo spostare i file dalla staging area verso il repository locale: infine con la push andiamo a spostare i file locali su repo remoto.

Il commit è un'operazione **atomica**: o fallisce per intero, o ha successo per intero.

![[Pasted image 20240325181343.png]]

I file possono essere in diversi stadi:

![[Pasted image 20240325182316.png]]

### Comandi Git

Per impostare l'account (una volta sola) usare:

```
git config -global user.name <my-name>
git config -global user.email <my-email>
```

Per inizializzare un repo dobbiamo fornirgli un'origine, ovvero il repo remoto:

```
git init
git remore add origin http://server.com/project.git
```

Per il resto dei comandi basta vedere [Git cheat-sheet](https://education.github.com/git-cheat-sheet-education.pdf).

### Branching 

Per ogni commit git salva:
- Un puntatore allo snapshot con il contenuto staged
- Un puntatore al vecchio commit da cui siamo partiti per modificare i file
Quando faccio staging dei file allora si fa un checksum su tutti e si salva la versione di quei file (blobs).

![[Pasted image 20240325185420.png]]

Di fatto in git, un branch è un semplice puntatore che punta a un commit: esiste il puntatore HEAD che si sposta automaticamente se aggiungo dei commit, oppure si può modificare manualmente cambiando branch.

Per fondere branch, possiamo usare **merge** o **rebase**, la differenza sta nel fatto che merge crea un nuovo commit fondendo il branch con quello principale, mentre il rebase modifica la storia del repo rendendolo lineare, andando di fatto ad eliminare la branch.

Rebase può anche essere usato per compattare fra loro numerosi commit consecutivi.

Per lavorare al meglio su git si usa un paradigma di **fork and pull**, dopo aver fatto una fork occorre aprire una pull (merge) request.
