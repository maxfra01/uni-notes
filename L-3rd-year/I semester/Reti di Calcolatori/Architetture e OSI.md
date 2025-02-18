# Modello a strati
---

### Protocollo
Descrizione formale delle procedure adottate per assicurare la comunicazione fra due o più oggetti dello stesso livello gerarchico.

Ogni protocollo prevede lo scambio di messaggi al fine di definire:
- **tipologia**, come richieste o risposte
- **sintassi** dei messaggi e/o pacchetti
- **semantica**, ovvero il significo dei vari campi di un pacchetto
- **temporizzazione** del messaggio, se sincrono o no

### Architettura di rete
Si intendono l'insieme di specifiche riguardanti:
- il processo di comunicazione
- le relazioni fra le entità coivolte
- le funzioni necessarie 
- le modalità organizzative

Oggigiorni è riconosciuto universalmente il modello **OSI** definito da ISO, ma ciò non significa che tutte le architetutture di rete rispettino il seguente modello:
In particolare nel modello OSI a ogni strato è associato uno o più protocolli speicifici.

Una rete è composta da più sistemi collegati tra loro tramite mezzi trasmissivi.
Caratterizziamo un sistema come un qualcosa diviso in strati.
Ogni strato realizza delle funzioni tramite delle **entità**.
Il principio di un'architettura a strati permette a ogni later di lavorare con le risorse fornite dai layer inferiori, al fine di fornire risorse e migliorare/integrare informazioni per quello superiore.

![[Pasted image 20221010145716.png]]

E' importarte chiarire che per l'N+1-esimo stato il funzionamento degli N strati inferiori è visto come una 'black box' dunque è sconosciuto a priori.

Distinguiamo ora due tipi di servizi offerti dagli strati:
- Servizi **Connection-oriented**: Si stabilisce preventivamente un accordo fra rete e interlocutori, si scambiano i dati e si libera al rete (ricorda il circuito virtuale)
- Servizi **Connectionless**: I dati sono messi in rete senza un accordo e sono trattati in modo indipendente.

I servizi sono offerti agli strati superiori tramite **SAP** (service access point).
Se consideriamo sistemi diversi, allora l'N-esimo strato di A comunica con l'N-esimo strato di B tramite un N-esimo protocollo.
Le **connessioni** sono relazioni fra SAP di diversi sistemi (tutti sulla stessa entità).

![[Pasted image 20221010150519.png]]

### Comunicazione fra sistemi

Ricordiamo la distinzione fra PCI, SDU, PDU, e allo strato i-esimo abbiamo:$$
PDU_{i}=PDI_{i}+ SDU_{i}
$$Poniamo che il sistema A voglia inviare dati al sistema B: allora ogni strato in discesa aggiungerà informazioni al pacchetto da inviare in intestazione.
Le intestazioni possono essere **in testa e in coda**.
Considerando N strati allora il PDU avrà N intestazioni diverse, una per ogni strato.
In ricezione ogni strato potrà elaborare la sua relativa porzione di informazione di controllo

![[Pasted image 20221010151248.png]]

E' possibile che uno strato non sia in grado di elaborare più di una certa quantità di dati, quindi è possibile avere una segmentazione delle informazioni in più pacchetti.
Ciò però non conviene, in quanto otterremo più intestazioni, occorre riassemblare i dati e se ne perdo uno perdo di significato il resto.

# Modello OSI

![[Pasted image 20221010152300.png]]

### Livello fisico
[[OSI 1 - Strato Fisico]]
E' lo strato che si occupa di trasferire le cifre binarie sul mezzo trasmissivo.
Dunque fornisce specifiche per i mezzi meccanici, fisici, funzionali e le procedure per attivare e disattivare la connessione.
Definisce ad esempio codifiche di linea, connettori, livelli di tensione ecc....

### Livello Collegamento
[[OSI 2 - Strato Collegamento]]
Fornisce i mezzi e le procedure per trasferire informazioni fra il livello rete e per fronteggiare problemi al layer fisico.
Tra le funzioni fondamentali abbiamo:
- delimitazione delle unità dati
- rilevazione e recupero degli errori
- controllo di flusso

### Livello rete
[[OSI 3 - Strato Rete]]
Fornisce i mezzi per instaurare, mantenere e abbattere le connessioni di rete tra entità di strato trasporto
Tra le funzioni fondamentali:
- instradamento
- controllo di flusso e congestione
- tariffazione

### Livello Trasporto
[[OSI 4 - Strato Trasporto]]
Colma le carenze di qualità di servizio delleconnessioni di strato rete
Funzioni fondamentali:
- Controllo di errore
- controllo di sequenza
- controllo di flusso
- esegue multiplazione e demultiplazione diconnessioni
- Esegue la segmentazione dei dati in pacchetti e la loro ricomposizione a destinazione

### Livello Sessione
Assicura alle entità di presentazione unaconnessione di sessione
- organizza il colloquio tra le entità dipresentazione
- struttura e sincronizza lo scambio di dati inmodo da poterlo sospendere, riprendere terminare ordinatamente
- maschera le interruzioni del servizio trasporto
- Spesso integrato nelle funzioni dei livelli superiori

### Livello Presentazione
Risolve i problemi di compatibilità per quantoriguarda la rappresentazione dei dati da trasferire.
- risolve i problemi relativi alla trasformazione della sintassi dei dati
- può fornire servizi di cifratura delle informazioni
- Spesso integrato nelle funzioni del livello superiore

### Livello Applicazione
[[OSI 7 - Strato Applicazione]]
Fornisce ai processi applicativi i mezzi peraccedere all’ambiente OSI.
Esempio di processi sono: trasferimento di file, posta elettronica e terminale virtuale.
