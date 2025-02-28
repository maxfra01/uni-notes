# Thread
---
Ogni processo (**Heavyweight process**) possiede:
- Spazio di indirizzamento prorpio
- Unico program counter

Perciò quando andiamo a creare un nuovo processo abbiamo problemi di spazio di memoria, tempi di creazione elevati, sincronizzazione e trasferimento dati, scheduling complesso ecc...

Lo standard IEEE/ANSI POSIX 1003.1c definisce il concetto di **Thread** (Lightweight process) come una sezione di un processo che viene schedulata e eseguita indipendentemente dal processo che l'ha generata.
Un thread condivide lo spazio di indirizzamento con altri thread.

I thread condividono dati quali codice, dati e risorse del sistema operativo; ogni thread possiede dati propri quali program counter, registri e stack.

![[Pasted image 20221107102223.png]]

Vantaggi rispetto ai processi:
- tempi di creazione ridotti
- condivisione dei dati in modo automatico
- costi minori per la gestione delle risorse
- maggiore scalabilità 

Svantaggi rispetto ai thread:
- non esiste protezione fra thread in quanto tutti sono eseguiti nello stesso spazio di memoria e il SO non può intervenire. Si parla di accesso ai dati condivisi **non thread safe**.
- Tutti i thread sono uguali, non esiste relazione gerarchica.


# Modelli di programmazione multi-thread
---

## Kernel-levek thread
In questo modello il kernel gestisce sia thread che processi e fornisce loro un supporto adeguato (sono presenti syscall per gestire i thread).

Il SO tiene traccia dei thread in una tabella e nel **Thread Control Block TCB** per ogni thread attivo.
Queste informazioni sono "globali" all'interno dell''intero SO.

| Vantaggi                                                           | Svantaggi                                           |
| ------------------------------------------------------------------ | --------------------------------------------------- |
| Il SO decide i processi da schedualare e come suddividere il tempo | A causa del passaggio alla kernel mode spreco tempo |
| Efficiente nelle applicazioni che si bloccano spesso               | Limitazioni sul numero di thread generati           |
| Effettivo parallelismo                                             | Costo in memoria per le TCB e informazioni          |


## User-level thread
In questo modello il kernel non è a conoscenza dei thread ed è in grado di gestire solamente processi.
I thread sono inseriti solamente nello spazio utente e sono gestiti tramite una **libreria**.

Anche in questo caso serve una tabella per memorizzare le informazioni ma le dimensioni sono notevolmente ridotte rispetto alla kernel-level (la visibilità non è grobale, ma locale al processo)

| Vantaggi                                              | Svantaggi                                                       |
| ----------------------------------------------------- | --------------------------------------------------------------- |
| Possibile implementare su ogni kernel                 | Si possono fare scelte poco efficienti                          |
| Non richiedono modifiche al SO                        | Occorre comunicare tra kernel e manager run-time                |
| Gestione efficiente (no context switch)               | Se il kernel thread si blocca tutti i thread utente si bloccano |
| Il programmatore può generare tutti i threa che vuole |                                                                 |

## Implementazione ibrida
L'idea di base sui moderni sistemi operativi è quella di avere $m$ thread utente mappati su $n$ thread kernel.

![[Pasted image 20221107104649.png]]

- L'utente decide i valori di $m,n$ 
- Il kernel gestisce solo i kernel thread ( e conosce solo quelli )

# Coesistenza di thread e processi
---
La coesistenza di questi due elementi provoca problemi relativi a segnali, alla clonazione e alla sostituzione di un processo (exec).

## Segnali
Il comportamento di un processo all'arrivo di un segnale è condiviso da tutti i thread.
Ogni segnale è consegnato a un thread singolo all'interno del processo

## Fork
In un contesto multithread la fork duplica solo il thread che l'ha richiamata. E' possibile per thread duplicati non riuscire a gestire dati privati.

## Exec
La exec sostituisce tutto il processo con il nuovo programma, non solo il thread che lo chiama.

