# UML

Struttura di una classe UML:

![[Pasted image 20230610144141.png]]

# SE

Lo sviluppo del software si pone l'obiettivo di realizzare appunto software con proprietà **di processo**: come costo e tempo di progetto, e **proprietà di prodotto**: funzionalità, performance, affidibilità.

Per le proprietà di produzione abbiamo:
- I requisiti del sw, ovvero quello che dovrebbe fare
- L'architettura e il design, ovvero come organizzare le unità e i dati
- L'implementazione del sw, ovvero la scrittura del codice

Per concludere abbiamo V&V verification and validation per controllare se alla fine del lavoro si sono rispettati i 3 obiettivi riportati sopra.

### Attività di managing

Il **project management** organizza il lavoro distribuendolo e monitorandolo. Stima e controlla il budget.

Il **Configuration management** memorizza e tiene traccia di documenti, codice ecc

La **Quality assurance** definisce obiettivi, come svolgere il lavoro e monitorando i risultati

### Process models

- **Build and fix** prevede solo di scrivere codice e fixarlo eventualmente; va bene se programmo da solo e per piccoli progetti, non c'è documentazione, VV, e design

- **Waterfall** prevede che non si passi a una fase successiva se non si è completata la precedente; troppo poco flessibile, rigido

- **Incremental** prevede di rilasciare periodacemente delle build (approccio di prototipo).

- **Evolutionary** è simile all'incrementale, ma i requisiti possono cambiare anche in fasi intermedie.

![[Pasted image 20230610150107.png]]

- **Agile** ha come obiettivo primario fornire codice funzionante e in fretta ai consumer, con documentazione dove richiesta, accettando i cambiamenti in base ai feedback ricevuti.


# V&V

Un errore è uno sbaglio del programmatore.
L'errore implifica un **Fault (BUG)** che causa a sua volta un **Failure** dove il software si comporta in modo inaspettato.

L'obiettivo di VV è quello di minimizzare la presenza di fault, e massimizzare la quantità di fault rilevati al fine di correggerli.

VV in due modalità: statica e dinamica
In modalità statica non si esegue effettivamente il codice, si fa controllo di codice sorgente e si individuale dei code smell: cattive metodologie di programmazione che potrebbero portare a fallimenti.

La modalità dinamica è il testing: lo scopo di quest'ultimi è quello di trovare **i failure**, il problema è che possono solo verifica la presenza di failure, ma non la loro totale assenza. (DEBUG != TEST, sono eseguiti in momenti diversi e il primo cerca bug, il secondo failure)