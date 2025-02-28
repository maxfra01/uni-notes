# Schduling della CPU
---
L'obiettivo principale della programmazione concorrente è quello di massimizzare l'uso delle risorse del sistema (la CPU è la più costosa).
Lo **Scheduler** è un'entità che deve decidere (tramite un algoritmo) a chi assegnare le risorse, ovvero a quale processo o thread assegnarle.
Dobbiamo dunque predisporre delle **funzioni di costo**.

Quando passiamo le risorse del sistema a un altro processo occorre congelare lo stato del processo corrente, passare ad eseguire l'altro e poi ripristinare le condizioni precedenti.

Esistono diversi tipi di Scheduler:
- Lo **Short term scheduler**, controlla i processi che devono subito essere eseguiti, ordine dei millisecondi
- Il **Medium term scheduler**, controlla i processi in RAM, agisce nell'ordine dei secondi
- Il **Long term scheduler**, decide i processi che stanno in disco, agisce nell'ordine dei multipli di secondi

Gli scheduler gestiscono i processi mediante **code (liste concatenate)**.

### Algoritmi

| Senza Prelazione              | Con prelazione   |
| ----------------------------- | ---------------- |
| First Come Firt Served (FCFS) | Round Robin (RR) |
| Shortest Job First (SJF)      | Shorter Remaining Time First (SRTF)                 |

### Funzioni di costo

![[Pasted image 20221212103939.png]]

### FCFS

La CPU viene assegnata con l'ordine per i quali i processi ne fanno richiesta, usa una coda FIFO.
Si utilizza la **carta di Gantt** per la pianificazione delle attività.

E' molto semplice da implementare, ma i tempi di attesa media sono lunghi e variabili, non adatto a sistemi real time, infine i processi lenti ritardano quelli brevi.

### SJF

Si suppone che per ogni processo conosca il **CPU Burst**, si ordinano i processi per tempo di durata.

![[Pasted image 20221212104655.png]]

```ad-note
Si noti che il processo P1 è immediatamente mandato in esecuzione perchè in quel momento non ci sono altri task
```

Si dimostra che questo algoritmo è ottimo, possibile però la starvation, e sopratutto il Burst Time normalmente è ignoro, perciò si possono effettuare delle stime.

### PS Priority Scheduling

Sostanzialmente a ogni processo è associata una priorità, quindi di fatto si implementa una SJF con il campo di rilevanza la priorità.
Nella pratica posso ordinare i processi tramite min/max heap.

Ottimo ma soggetto a starvation (processi a bassa priorità possono non essere mai eseguiti).
Per risolvere ciò introduco l'**aging** dei processi, più invecchiano e più aumentano la priorità.

### RR Round Robin

Versione modificata della FCFS che permette la prelazione.
IL CPU time è diviso in **quantum**, un task riceve la CPU per quel tempo e poi viene riaccodato ina una FIFO.

I tempi di attesa sono molto elevati:
- per quantum lunghi si degenera nel FCFS
- per quantum corti bisogna fare eccessivi context switching

### SRTF Short Remaining-Time First

E' una versione di SJF che permette la prelazione.
Si procede come una SJF ma quando viene sottomesso un processo, si verifica il burst time ed eventualmente si prelaziona la CPU.

![[Pasted image 20221212110614.png]]

Nuovi task brevi vengono eseguiti in modo efficiente.

### Multilevel Queue Scheduling MQS

Se i tasks possono essere raggruppate in gruppo allora si fanno più livelli di code ed ogni coda può essere gestita con un proprio algoritmo.

### Osservazioni

Anche lo Scheduler è un task in sè.
Quello con prelazione viene invocato periodicamente con un interrupt, esso verifica che fanno gli altri tasks e li organizza.

Lo scheduling può avvenire a livello di thread o di processo (negli odierni OS si schedula per thread kernel).

Tutti gli esempi sopra sono progettate a monoprocessori, ma nella realtà i sistemi sono multiprocessore, dunque abbiamo problemi di **bilanciamento del carico**, perciò abbiamo multi elaborazione **asimmetrica**, dove un master suddivide il lavoro fra gli altri.
Quella **simmetrica** per ogni processore c'è uno scheduler.

Nei sistemi Real Time deve esserci latenza bassissima!
- Soft realtime, priorità ai processi critici ma non garantita la tempistica ottima di risposta
- Hard realtime, si garantisce la tempistica di risposta.


