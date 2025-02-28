# Architettura

Il documento dei requisiti ci fornisce specifiche sul cosa dovrebbe fare un software.
L'**architettura** definisce come il sistema dovrebbe svolgere il proprio compito: definiamo perciò dei **componenti** (alto livello) che interagiscono fra di loro.

Molti difetti di un software arrivano dalla fase di requirement e design: alcune scelte di design vanno prese nelle fasi iniziali di un progetto. Ovviamente, dato un documento dei requisiti ci sono molteplici possibilità per il design.

1. Scelta dell'**architettura**: 
	- Definizione di componenti che interagiscono tra loro ad alto livello
	- Scegliere modelli di comunicazione e coordinazione
	- Scelta di eventuali patterns
2. **Design**
	- Definire le classi vere e proprie e le loro interazioni
	- Usare eventuali pattern

Principali proprietà del design:
- Il design rispetta i requisiti funzionali
- Reliability, Portability...
- Specifiche proprietà come il **coupling (relazione fra componenti)** e **Cohesion (consistenza dei componenti)**

**Formalizzazione dell'architettura**: tramite mezzi informali (diagrammi, linee, box) oppure tramite **ADL (Architecture description language)** o UML...

# Patterns

"The pattern is, in short, at the same time a thing, which happens in the world, and the rule which tells us how to create that thing and when we create it. It is both a process and a thing …"

Divisi in due categorie: architetturali e design.
Tra i **pattern architetturali** troviamo: Layer, pipes and filter, repository, client server, broker, MVC, microkernel, microservice

### Repository

Ci sono sottosistemi che si scambiano i dati attraverso:
- Un repository centrale a cui tutti i sotto sistemi possono accedere
- Ogni sotto sistema possiede un repository proprio e manda esplicitamente dati ad altri sottosistemi.
Se i sottosistemi vogliono comunicare correttamente devo condividere lo **stesso modello di dati** (se chiamo un campo 'temp' anche gli altri devono leggere 'temp).

![[Pasted image 20240508094843.png]]

Vantaggi:
- Efficiente per condividere grandi quantità di dati
- I sottosistemi non si preoccupano di come i dati sono stati generati
- Gestione centralizzata (sicurezza, backup)

Svantaggi:
- Il modello dei dati deve essere lo stesso per tutti i sottosistemi
- L'evoluzione dei dati è complesso e costoso
- Difficile da distribuire efficacemente

### Client server

Già visto in moltissimi corsi.

### Pipes and filter

Se dobbiamo processare uno stream di dati possiamo usare questo pattern: I dati passano in 'stazioni' devo vengono elaborati. Poi i dati sono ricombinati. Inoltre step non adiacenti non possono condividere i dati.

![[Pasted image 20240508095824.png]]

### Broker

In un contesto con molti componenti eterogenei, si usa questo pattern dove un **broker** è in grado di far comunicare componenti differenti, nascondendo a quest'ultimi la maggior parte dei dettagli del sistema.
Di fatto è un nodo centrale che espone diversi metodi per il corretto funzionamento del sistema.

![[Pasted image 20240508100245.png]]

### MVC

Il pattern architetturale MVC, acronimo di Model-View-Controller, è un approccio comunemente utilizzato nello sviluppo software per separare le responsabilità di un'applicazione in tre componenti principali:

1. **Model (Modello)**: Il model rappresenta i dati dell'applicazione e le regole che governano il loro accesso e la loro modifica. In sostanza, gestisce lo stato dell'applicazione e le operazioni su di esso. Questo può includere logica di business, operazioni di accesso al database, e altro ancora.
    
2. **View (Vista)**: La vista è responsabile per la presentazione dei dati all'utente e per l'interazione con esso. Tipicamente, la vista visualizza l'informazione in un formato comprensibile all'utente e in genere reagisce ai comandi dell'utente, trasmettendo le azioni a appropriate parti del controller o del model.
    
3. **Controller (Controllore)**: Il controller agisce come intermediario tra il modello e la vista. Risponde alle azioni dell'utente, facendo chiamate appropriate al modello per effettuare le modifiche necessarie e aggiornando la vista per riflettere tali modifiche. In sostanza, gestisce il flusso di controllo e le operazioni di gestione degli eventi.
    
L'obiettivo principale di MVC è **separare la logica di presentazione dall'implementazione del business logic, rendendo così il codice più organizzato, riutilizzabile e facile da manutenere**. Questo modello favorisce anche la scalabilità e la collaborazione tra membri del team di sviluppo, in quanto le responsabilità sono chiaramente definite e separate.

![[Pasted image 20240508100811.png]]

### MicroKernel

Il pattern Microkernel è un'architettura software che si basa sulla **separazione delle funzionalità del sistema in componenti fondamentali e plug-in opzionali**, con un nucleo minimale che gestisce solo le operazioni essenziali. In sostanza, il microkernel si concentra sull'essenziale, delegando altre funzionalità a moduli esterni.

Le caratteristiche principali del pattern Microkernel includono:

1. **Nucleo Minimale (Microkernel)**: Il nucleo del sistema è ridotto al minimo, comprendendo solo le funzionalità essenziali come la gestione delle comunicazioni tra i componenti e la gestione delle risorse di base.

2. **Modularità**: Le funzionalità del sistema sono implementate come plug-in esterni al nucleo. Questi moduli possono essere aggiunti o rimossi in base alle esigenze dell'applicazione senza influire sul nucleo stesso.

3. **Interfaccia Standardizzata**: Il microkernel fornisce un'interfaccia standardizzata per la comunicazione tra i moduli. Questo consente una facile integrazione di nuovi moduli nel sistema esistente.

4. **Flessibilità e Estensibilità**: Grazie alla sua architettura modulare, il sistema può essere facilmente esteso con nuove funzionalità senza dover modificare il nucleo del sistema. Questo rende il sistema altamente flessibile e adatto a una vasta gamma di applicazioni.

5. **Scalabilità**: Poiché il nucleo del sistema è ridotto al minimo, il microkernel è altamente scalabile e può essere utilizzato su una varietà di piattaforme hardware e software.

Il pattern Microkernel è spesso utilizzato in sistemi operativi, sistemi distribuiti e applicazioni complesse in cui la **modularità**, la **flessibilità** e la **manutenibilità** sono prioritari. Tuttavia, l'implementazione di questa architettura può comportare un overhead aggiuntivo a causa della necessità di gestire la comunicazione tra i diversi moduli.

![[Pasted image 20240508100934.png]]

### Microservice

I microservizi sono un approccio all'architettura software in cui un'applicazione è **suddivisa in componenti autonomi, ciascuno dei quali esegue un'unica funzione specifica**. Questi componenti, chiamati microservizi, sono indipendenti l'uno dall'altro e comunicano tramite protocolli leggeri come HTTP o messaggistica asincrona. Questo approccio favorisce la modularità, la scalabilità e la facilità di manutenzione delle applicazioni, consentendo agli sviluppatori di aggiornare, testare e distribuire singoli servizi senza influenzare l'intera applicazione.

Ad esempio EZElectronics.

# Design

- Il design del software, o design dell'architettura, è il processo di **dettaglio e implementazione delle decisioni architetturali** a livello di singoli componenti o moduli del sistema.
- Si concentra sulla progettazione dei **dettagli interni di ciascun componente**, come le interfacce, le classi, i metodi e le relazioni tra di essi.
- Il design del software si svolge tipicamente dopo che l'architettura è stata definita e fornisce una guida più dettagliata per l'implementazione del sistema

### Pattern di Design

1. **Livello di astrazione**: I pattern di design operano a un livello più dettagliato rispetto ai pattern architetturali. Si concentrano sulla progettazione di classi, oggetti e interfacce, definendo le relazioni e i comportamenti specifici tra di essi.
    
2. **Scopo principale**: L'obiettivo dei pattern di design è fornire soluzioni flessibili e riutilizzabili a problemi specifici che si verificano a livello di codice, come la creazione di oggetti, la gestione delle relazioni tra di essi e la gestione del comportamento degli oggetti.

In sintesi, mentre i pattern architetturali definiscono la struttura generale e le relazioni di alto livello tra le componenti di un sistema, i pattern di design forniscono soluzioni più dettagliate a problemi specifici che si verificano all'interno delle componenti del sistema. Entrambi sono importanti nel processo di sviluppo del software e lavorano insieme per creare sistemi software robusti e scalabili.

1. **Factory Method**:
    - Obiettivo: Definire un'interfaccia per la creazione di un oggetto, ma lasciare alle sottoclassi la decisione su quale classe istanziare.
    - Utilizzo tipico: Creazione di oggetti di una famiglia di classi correlate senza specificare la classe effettiva.

2. **Abstract Factory**:
    - Obiettivo: Fornire un'interfaccia per la creazione di famiglie di oggetti correlati o dipendenti senza specificare le classi concrete.
    - Utilizzo tipico: Creazione di oggetti correlati all'interno di una gerarchia, ad esempio in un'applicazione che supporta più stili di interfaccia utente.

3. **Builder**:
    - Obiettivo: Separare la costruzione di un oggetto complesso dalla sua rappresentazione, consentendo la creazione di oggetti con configurazioni diverse.
    - Utilizzo tipico: Creazione di oggetti complessi step-by-step o con configurazioni personalizzate, come ad esempio nell'implementazione di un'interfaccia fluent per la costruzione di oggetti.

4. **Prototype**:
    - Obiettivo: Definire un'interfaccia per la creazione di un oggetto basato su un prototipo di istanza, permettendo la creazione di nuovi oggetti copiandone le caratteristiche.
    - Utilizzo tipico: Creazione di nuovi oggetti basati su istanze esistenti, ad esempio nell'implementazione di un sistema di creazione di oggetti preconfigurati.
 
5. **Singleton**:
    - Obiettivo: Garantire che una classe abbia una sola istanza e fornire un punto di accesso globale a tale istanza.
    - Utilizzo tipico: Gestione di risorse condivise come connessioni al database o file di configurazione.