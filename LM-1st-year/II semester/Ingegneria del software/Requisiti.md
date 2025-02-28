# Requirement engineering

Si intende la **definizione delle proprietà del prodotto** prima di cominciarne lo sviluppo: è importante sottolineare che nel dialogo con un cliente,  i requisiti non sono quasi mai chiari né tantomeno realizzabili.

Le proprietà di un software si distinguono in funzionali e non funzionali.

![[Pasted image 20240308225547.png]]

La fase iniziale dei requisiti è una discussione informale con un cliente che descrive ciò di cui ha bisogno: l'obiettivo è quello di collezionare requisiti che siano **completi e consistenti**:
- Per completi intendiamo esaustivi
- Per consistenti intendiamo che non ci siano contraddizioni

E' utopistico pensare di generare tali requisiti per motivi come: omissioni, incorrettezze,  ambiguità, troppi dettagli e ridondanze.

Definiamo ora alcune tecniche per la **formalizzazione** dei requisiti:
### Stakeholders

Uno stakeholder è un individuo, un gruppo o un'organizzazione che **ha interesse o influenza su un progetto**, un'azienda o un'organizzazione. Gli stakeholder possono includere dipendenti, clienti, fornitori, investitori, autorità di regolamentazione.

Elencare gli stakeholders è fondamentale per capire i vari punti di vista nello sviluppo del progetto.

### Stories e personas

1. **User Stories (Storie degli Utenti):** Le storie degli utenti sono brevi descrizioni delle funzionalità del software, scritte dal punto di vista degli utenti finali. Solitamente seguono uno schema semplice: "Come\[tipo di utente\], voglio \[funzionalità\] per \[motivo\]". Ad esempio, "Come utente registrato, voglio poter modificare il mio profilo per mantenere le informazioni aggiornate". Le storie degli utenti servono a comunicare in modo chiaro e conciso le esigenze degli utenti al team di sviluppo e forniscono un modo efficace per suddividere il lavoro in compiti gestibili e focalizzati sull'esperienza utente.
    
2. **Personas:** Le personas sono archetipi o rappresentazioni fittizie di utenti reali. Sono create attraverso la raccolta di dati demografici, comportamentali e psicografici degli utenti target. Le personas aiutano il team di sviluppo a comprendere meglio le esigenze, i desideri e le motivazioni degli utenti finali, consentendo loro di progettare e sviluppare un software che soddisfi efficacemente le esigenze di un'ampia varietà di utenti. Ad esempio, potrebbe esserci una persona chiamata "Alice" che è una professionista occupata alla ricerca di un'applicazione mobile che semplifichi la gestione delle sue attività quotidiane. Comprendere le necessità di Alice aiuta il team a concentrarsi sulle funzionalità e sull'esperienza utente che sono più importanti per lei.

### Context diagram

Un diagramma di **contesto** (context diagram) è un tipo di diagramma che rappresenta visivamente l'ambiente esterno di un sistema software e le interazioni principali che il sistema ha con esso. Questo diagramma è spesso utilizzato durante le fasi iniziali del processo di progettazione del software per fornire una visione ad alto livello del sistema e del suo contesto.

Le entità fuori dall'applicazione sono detti **attori**.
Le **interfacce di comunicazione** sono frecce o  linee tra il sistema principale e le entità esterne indicano le interfacce di comunicazione o i flussi di dati tra di essi.

### Requisiti funzionali

Occorre **separare** e **nominare** tutti i requisiti funzionali, inoltre è buona norma l'uso di numerazione gerarchica: questo è utile in fase di sviluppo e testing.

La ISO/IEC 25010 si basa sui seguenti sette attributi di qualità del software:

1. **Funzionalità (Functionality):** Capacità del software di fornire funzionalità che soddisfino i requisiti specificati e gli obiettivi dell'utente.
2. **Affidabilità (Reliability):** Capacità del software di eseguire le sue funzioni richieste in modo affidabile, garantendo la precisione e la consistenza dei risultati.
3. **Usabilità (Usability):** Facilità con cui gli utenti possono utilizzare il software per raggiungere i propri obiettivi, includendo aspetti come l'intuitività dell'interfaccia utente e l'efficienza nelle operazioni. 
4. **Efficienza (Efficiency):** Capacità del software di utilizzare in modo efficiente le risorse disponibili, come la memoria, la CPU e la larghezza di banda di rete, durante l'esecuzione delle operazioni.
5. **Manutenibilità (Maintainability):** Facilità con cui il software può essere modificato, corretto o adattato in risposta a cambiamenti nei requisiti o nell'ambiente operativo.
6. **Portabilità (Portability):** Facilità con cui il software può essere trasferito da un ambiente di sviluppo o di esecuzione all'altro, senza necessità di modifiche sostanziali.
7. **Efficienza (Efficiency):** Capacità del software di eseguire correttamente le funzioni richieste nel tempo previsto e con risorse accettabili.

Inoltre definiamo anche:

1. **Sicurezza (Security):** Si riferisce alla capacità del software di proteggere i dati, i sistemi e le risorse da accessi non autorizzati, modifiche non autorizzate e altri attacchi informatici. Questo include la protezione dei dati sensibili, l'autenticazione degli utenti, il controllo degli accessi e la gestione delle vulnerabilità.
2. **Sicurezza operativa (Safety):** Si riferisce alla capacità del software di garantire che le operazioni siano eseguite in modo sicuro e senza rischi per le persone, le proprietà o l'ambiente circostante. Questo è particolarmente importante nei sistemi critici per la sicurezza, come i sistemi di controllo industriale e i sistemi medici.
3. **Dependability:** Si riferisce alla capacità del software di essere affidabile, disponibile, resiliente e mantenibile nel tempo. Include anche la capacità di ripristinare rapidamente il funzionamento normale dopo un guasto o un'interruzione.

### Requisiti non funzionali

Spesso sono più stringenti dei requisiti funzionali, e riguardano requisiti aggiuntivi (efficienza, tempo di risposta), organizzativi ed esterni.

Devono essere **misurabili**, altrimenti sono del tutto inutili: ad esempio un requisito del tipo "un sistema deve essere facile da usare e minimizzare gli errori" è inutile. Una variante sensata potrebbe essere "dopo 2 ore di training il numero di errori non supera i 2 per giorno".
Di fatto ciò che **non è testabile non è utile**.

Nel caso di **conflitti tra requisiti RF**, gli stakeholders chiave devono prendere una decisione (ad esempio efficienza vs sicurezza): questa è una decisione aziendale perciò i programmatori non possono scegliere.

E' importante definire anche i **requisiti di dominio** (in che ambito è posto il prodotto in uso) e definire un **glossario** per favorire la corretta comunicazione: in particolare per il glossario si può usare il linguaggio [[UML]]:

![[Pasted image 20240309125127.png]]

# Scenario e Use cases

Uno **scenario** è un esempio di interazione con il software in sviluppo: è composto da una serie di precisi passi (eventi). In questo contesto è necessario considerare il **tempo**.
Considerando l'esempio del sistema di vendita possiamo immaginare uno scenario come il seguente:

| Step | Descrizione                             |
| ---- | --------------------------------------- |
| 1    | Start sales transaction                 |
| 2    | Read bar code X                         |
| 3    | Retrieve name and price given barcode X |
| 4    | Repeat 2 and 3 for all products         |
| 5    | Compute total T                         |
| 6    | Manage payment cash amount T            |
| 7    | Deduce stock amount of product          |
| 8    | Print receipt                           |
| 9    | End transaction                         |

Possiamo aggiungere a uno scenario dei **pre e post conditions**: se pensiamo al sistema di vendita possiamo aggiungere come pre-condition il fatto che l'utente sia loggato.
Dobbiamo inoltre prevedere scenari alternativi (con eventuali errori) come il rifiuto di una carta di credito, oppure un metodo di pagamento particolare.

Un **use case** è un insieme di scenari comuni che hanno un obiettivo simile in comune: nel caso della vendita abbiamo ad esempio la gestione delle vendite:
- Scenario1: vendita di 2 oggetti
- Scenario2: vendita di n oggetti con pagamento elettronico
- Scenario3: vendita di n oggetti con transazione rifiutata o prodotto non valido.

![[Pasted image 20240309125610.png]]

Il context diagram va a unire i requisiti funzionali descritti nei passi precedenti con gli attori che interagiscono con il sistema, tenendo in considerazione il **tempo**.

### Elementi chiave

Gli elementi chiave che dobbiamo tenere in considerazione sono principalmente:
- gli **attori** che interagiscono con il sistema
- **il sistema** (ancora visto come una black box, non serve scendere in dettagli implementativi o grafici)
- L'**obiettivo** che l'utente vuole raggiungere interagendo con il sistema
### Relazioni

Definiamo in genere le relazioni fra un attore e un use case:

![[Pasted image 20240309130138.png]]
![[Pasted image 20240309130149.png]]
![[Pasted image 20240309130207.png]]

### Template narrative

| Use Case               | Titolo caso d'uso                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| ---------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Scope                  | Sistema da progettare (o una parte)                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| Level                  | User-goal                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| Intention in contact   | Perchè l'utentenusa il sistema e cosa vuole ottenere                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| Primary actor          | Utente principale                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Support actor          | Attore di supporto, se presente                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| Stakeholders' interest | Interesse specifico di uno SH in questo caso d'uso                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| Precondition           | Condizione che deve verificarsi perchè il caso d'uso sia eseguito                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Minimum guarantees     | Condizione che si verifica indipendentemente dal successo o fallimento del caso d'uso                                                                                                                                                                                                                                                                                                                                                                                                                             |
| Success guarantees     | Condizione che si verifica se il caso d'uso termina con successo                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| Trigger                | (se presente) è una condizione che scatena l'avvio del caso d'uso da parte dei sistema. Se è l'utente ad avviare autonomamente il sistema, il trigger non esiste                                                                                                                                                                                                                                                                                                                                                  |
| Main success scenario  | Elenco numerato di interazioni utente-sistema (o sistema-utente nel caso di avvio da trigger) che descrive i passi necessari per raggiungere l'obiettivo. (best case scenario)<br><br>1. L'utente chiede di effettuare una domanda di assunzione<br>2. Il sistema chiede i dati anagrafici<br>3. L'utente inserisce i dati anagrafici<br>4. Il sistema chiede un CV in pdf<br>5. L'utente carica il CV<br>6. Il sistema salva il CV e notifica l'utente del successo<br>7. Il caso d'uso termina con successo<br> |
| Extensions             | Valutazione di tutte le possibili eccezioni che possono capitare nel main success scenario.<br><br>Ad esempio nel passo 4 il sistema può rilevare errori e si riprende dal punto 3.<br>                                                                                                                                                                                                                                                                                                                           |

### Design di un sistema

Se dobbiamo progettare non solo software, ma anche del sistema hardware, dovrò specificare tutte le componenti del sistema.
E' necessario considerare tutti gli elementi all'interno del **diagramma di contesto**. In particolare produrremo un modello UML di una certa entità del sistema.
Per ogni modulo (entità) andrò a definire dei metodi associati **ad alto livello** (non scendo nei dettagli):

![[Pasted image 20240320091629.png]]

Se all'interno del system design c'è una parte computazionale (nell'esempio: il computer) lo si chiama **nodo**. Se invece c'è una parte software lo chiamo **artefatto**.

Infine si definisce il **deployment diagram** che da un'idea della struttura del software nel sistema.
# Requirement doc: struttura

1 Overall description
2 Stakeholders
3 Context diagram and interfaces
4 Requirements
	• Functional
	• Non functional
	• Domain
5 Use case diagram
6 Scenarios
7 Glossary
8 System design

L'ordine può differire a seconda dell'importanza dei vari paragrafi, inoltre esistono numerose tipologie di metodi per sviluppare il documento.