# Sistemi di elaborazione
---
Un sistema di elaborazione è costituito dai seguenti componenti:
- Utenti
- Applicativi
- Programmi
- Hardware 
- Sistema operativo
In questo corso ci occuperemo in dettaglio del SO e delle sue componenti ovvero **kernel e shell**.

### Definizione di sistema operativo
Non esiste una definizione universale, ma possiamo definirlo in due approcci:
- Nella versione **top-down** il SO astrae i vari dispositivi del sistema, offrendo all'utente una macchina estesa con complessità più ad alto livello.
- A livello **bottom-up** il SO è un gestore delle risorse, ovvero un programma che controlla il funzionamento e le operazioni dei dispositivi.

### Moduli e servizi degli OS
Tra i più comuni e quelli che tratteremo di più:
- Interprete dei comandi
- Gestione dei processi
- Gestione file e [[File System]]
- Altri aspetti minori

# Terminologia e concetti base

### Kernel
E' il nucleo del sistema operativo, è un software che fornisce ai processi in esecuzione un accesso sicuro e controllato all'hardware.
Ha la responsabilità di gestire i processori, il multitasking e il sistema.
Unico programma in esecuzione tutto il tempo in cui la macchina è accesa.

Esistono diversi tipo di kernel: monolitici, microkernel, ibridi.

![[Pasted image 20221003103111.png]]

- Nei sistemi a kernel **monolitico**, il kernel è un programma molto vasto in cui si lavora sempre in kernel mode (lanciamo una syscall, entriamo in kernel mode, eseguiamo, e torniamo in user mode).
- Nei **microkernel** sono contenute solamente le funzioni più delicate che devono lavorare in modalità protetta.
- Nei **ibridi** i due modelli si mischiano in vari modi.

Esiste l'**esokernel** che

### Bootstrap
Fase di boot del sistema operativo.
Il kernel è caricato ed eseguito in memoria centrale.
Il programma di bootstrap è scritto in ROM.

### System call
Procedure di sistema che forniscono l'interfaccia ai servizi forniti dal sistema operativo stesso.
Negli OS ci sono un numero limitato di syscall, ovvero un numero che si aggira fra i 100 e 300 syscall.

Differenza fra libreria e syscall?
- Entrambe forniscono servizi all'utente.
- Per ogni syscall corrispondono una o più funzioni di alto livello.
- Le syscall sono immutabili, le funzioni sono modificabili.
- Le funzioni implementano operazioni complesse, mentre le syscall offrono servizi di base.

Quando si esegue una syscall bisogna lavorare in modalità protetta.
Il SO lavora in **dual-mode**, ovvero può temporaneamente andare in kernel mode agendo sul **bit di modo**.
Il passaggio in modalità kernel è detto **trap**.
Quando si chiama la syscall non si va all'indirizzo della funzione nello spazio utente, ma invece si esegue una trap e si passa al kernel.
All'interno del kernel il codice della syscall viene usata per accedere a una tabella che contiene il codice da eseguire.

![[Pasted image 20221003105342.png]]

Esempio di syscall 'read', stile UNIX
`int read (int fd, void *buffer, size_t nbytes)  `
Ritorna il numero di byte letti come intero.

### Login
Il processo di **autenticazione** è il termine che indica l'accesso a una macchina indicando due entità, ovvero username e password.

### Shell
E' la finestra a caratteri UNIX per eccellenza, è possibili digitarvi caratteri e usare i comandi.
NON fanno parte del sistema operativo, le shell sono applicativi a parte.
Shell di uso comune sono sh e **bash**.

### File system
è la struttura gerarchica **a grafo aciclico** in cui si organizzano le directory e i file.
In UNIX tutto il filesystem si sviluppa dalla 'root' indicata con /.
Tra le directory spicca quella 'home' in cui risiedono gli utenti.
A percorso /usr/bin... troviamo i comandi eseguini di shell.

- Filename, nome del file con regole specifiche (assenza di '/' e 'null')
- Pathname, percorso del file che vogliamo raggiungere (es. /home/maxfr/Documenti/es4.c). Può essere un percorso assoluto o relativo a seconda della working directory. 
- Il carattere '.' indica la directory corrente
- Il carattere '..' indica la directory padre
- Home directory, è la directory al cui accede un utente al momento del login
- Working directory, direttorio corrente, da cui si interpretano i path relativi.

![[Pasted image 20221003111224.png]]

### Programma
è un'entità **passiva** eseguibile che risiede su disco fisso.
Programma sequenziale: operazioni da eseguire in sequenza dove ogni istruzione inizia al termine della precedente.

Concetto di **atomicità**: operazione che non può essere interrotta da nessun processore.
Perchè non garantire sempre l'atomicità? Troppo costoso e rallentamento eccessivo della macchina.
Operazioni apparentemente atomiche come  `x++;` in realtà non lo sono.

Programma **parallelo** o **concorrente**: Più operazioni possono essere eseguite contemporaneamente, rappresentabili come un grafo delle precedenze.

### Processo
è detto 'processo' un programma in esecuzione, esso rappresente un'entità **attiva**.
Ogni processo (in UNIX) è rappresentato da un codice univoco.
La relazione fra i processi può essere rappresentata da un albero dei processi.

### Thread
anche detto 'processo leggero', un thread individua un **flusso di esecuzione** singolo all'interno dello stesso processo.
Ogni thread ha un identificatore locale al processo

### Pipe
la più vecchia forma di comunicazione fra i processi.
Una pipe è un flusso dati fra due processi, sono half-duplex.

### Deadlock
anche detto 'stallo', si verifica quando una serie di entità attende il verificarsi di un evento che è bloccato per cause diverse.
Ne esistono diversi tipi:
- Livelock, processi non effettivamente bloccati, ma di fatto non fanno nulla di utile (es. busy waiting).
- Starvation, situazione in cui a un'entità viene ripetutamente bloccato l'accesso a una risorsa.

Starvation !--> Deadlock
Deadlock--> Starvation



