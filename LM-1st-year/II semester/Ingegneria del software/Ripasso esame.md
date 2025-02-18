# Software process

![[Pasted image 20240627000558.png]]

# Requirement

Per i NF:
- Privacy
- Dominio
- Affidabilità
- Efficienza
- Precisione
- Portabilità
- Mantenibilità

# Configuration management

- Lock modify Unlock vs Copy Modify merge
- Centralizzato vs locale vs distribuito
- delta vs full copies
- snapshot vs differences

# Project management

- Milestone: evento/condizione chiave nel progetto
- Deliverable: prodotto parziale o completo pronto per una demo (interna o esterna)
- Effort: tempo necessario per una risorsa (persona) completi una task, si misura in person hour (month, week ...)
- Costo: person hour * cost per hour

![[Pasted image 20240628111246.png]]

- Per definire la grandezza del progetto possiamo usare le LOC, la lunghezza dei documenti, o dei test. Ovviamente le LOC non sono ottimali perchè variano (commenti, linee vuote ecc)

![[Pasted image 20240628111853.png]]

Durante la fase di planning non si è pagati, ma bisogna comunque produrre un preventivo per il cliente.

- Estimation: basata su progetti simili, sulla decomposizione (WBS, PBS) 
- Parkinson law: Il costo del progetto è limitato solo dalle risorse che si hanno (spesso cosi non si finisce mai)
- Dall'altra, dato il budget dobbiamo rientrarci, ma sicuro non riusciamo a soddisfare il cliente
- In WBS dobbiamo dividere in attività
- in PBS dobbiamo dividere in deliverable
- Poi facciamo il Gantt

Un'altra metrica sono i **function points**: sono le funzionalità base viste da un utente che il software deve fare

![[Pasted image 20240628113014.png]]

Occorre individuare le variabili ad assegnarle un peso.

![[Pasted image 20240628113853.png]]

# UML

Una **classe UML** descrive un insieme di oggetti che sono caratterizzati da **proprietà comuni**, che esiste a sè. Un' **istanza** di una classe è un oggetto del tipo che la classe rappresenta.

Per gli **attributi** occorre indicare il nome ed il suo tipo (numerico, booleano...).

Le cose modellabili come classi possono essere:
- persone fisiche
- ruoli
- entità legali/sociali/organizzative
- eventi
- intervalli temporali
- entità geografiche
- report e sommari

Possiamo definire delle **associazioni fra classi** (come relazioni nei modelli ER), occorre indicare la **molteplicità** e l'eventuale **ruolo** delle classi coinvolte
E' possibile definire **associazioni ricorsive** (impiegato supervisiona un impiegato)
Il nome delle associazioni dev'essere un verbo (good practice).
Spesso si mette una freccia per chiarificare la direzione dell'associazione.

Per indicare le molteplicità si usano solamente 3 simboli: `0`, `1`, `*` 

E' possibile associare a una associazione anche degli attributi (simile a una relazione ternaria):

![[Pasted image 20240626144008.png]]

E' possibile che delle **classi che facciano parte di altre classi**: si parla di **aggregazione**

![[Pasted image 20240626143630.png]]

La **composizione** è simile all'aggregazione ma qui il ciclo di vita delle classi "part / whole" è la stessa, in altre parole il collegamento fra le parti e l'intero è più "stretto".
Ad esempio se modelliamo una persona con delle classi composte testa, gambe e braccia parliamo di composizione, in quanto se la persona sparisce allora non considereremo più nemmeno gambe e braccia

![[Pasted image 20240626143847.png]]

Si parla di **specializzazione / generalizzazione** se una classe "figlia" è un **caso particolare** della classe "padre" più generale. Le due classi condivideranno gli attributi e in più le classi più particolari avranno i loro attributi esclusivi.
Si può usare questo modello solamente se passa il predicato "B è un A?"

![[Pasted image 20240626144145.png]]

Grazie all'ereditarietà le **sottoclassi ereditano sia attributi che le relazioni** 
Attenzioni: **un oggetto non cambia mai sottoclasse (uno studente rimane tale e un impiegato anche)**. Se i requisiti permettono questo cambio allora la specializzazione non è la soluzione esatta. 

| DO NOT                                                               |
| -------------------------------------------------------------------- |
| Usare il plurale per le classi                                       |
| Dimenticare le molteplicità                                          |
| Usare delle classi come attributi (inserire dunque una associazione) |
| Usare un attributo che rappresenta più oggetti                       |
| Usare molteplici associazioni che rappresentano eventi nel tempo     |
| Inserire attributi ridondanti (come fossero chiavi esterne in un db) |
| Creare loop con le associazioni                                      |
# Black box testing

Vedi esempi, noioso e lungo...

# White box testing

Ho la conoscenza interna del codice e cerco di testare in base alla **coverage**: devo scegliere il tipo di codice.
- **Node coverage**: per nodi si intendono istruzioni atomiche e le decisioni
- **Condition coverage**: l'obiettivo è coprire tutte le decisioni del codice (tutti le possibilità degli if), si parla di multiple condition coverage
- **Path coverage**: Cerco di coprire tutti i percorsi possibili nel codice a partire dal CFG, per percorso intendo una sequenza di nodi nel grafo. Attenzione: se è presente una ramificazione dei percorsi occorre calcolare il numero di test necessari per coprirli tutti. Se ad esempio siamo in un for loop che itera 6 volte e all'interno ci sono 4 percorsi disponibili allora dovremo scrivere 4^6 tests.
- **Loop coverage**: per coprire i loop scriviamo, per ogni loop separatamente, un test per non entrarci, un test per entrarci una volta, un test per iterare più volte. Attenzione: se c'è un ciclo for unico che itera un numero di volte arbitrario, allora la copertura è del 33% in quanto copriamo solo una possibilità


# Processi

| Nome                      | Caratteristiche                                                                                                                                                                                                                                        |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Build and fix             | - No requirements, no design, non validation, okay per solo programming, non scalabile                                                                                                                                                                 |
| Waterfall                 | - Attività sequenziali, prima di iniziare una fase la precedente va completata, molta enfasi sui documenti, nella pratica non si realizza, poca flessibilità e troppo burocratico                                                                      |
| Waterfall - V model       | Variante del waterfall con particolare enfasi su VV, i test di accettazione vengono scritti con i requisiti, i test di unità sono scritti durante il design                                                                                            |
| Waterfall - + prototyping | Come waterfall ma nei requisiti si fa un prototipo per accettarsi con il cliente della correttezza                                                                                                                                                     |
| Waterfall - Incremental   | Nel waterfall normale dopo gli integrations si produce in un colpo solo la build finale, in incremental ogni loop sugli integration produce una build                                                                                                  |
| Agile                     | Movimento che punta a fornire codice spesso, con comunicazione fra i team e soddisfazione del cliente                                                                                                                                                  |
| Agile - scrum             | Basato su sprint di 30 giorni in cui si sviluppa qualcosa che deve essere utilizzabile. Molti meeting( 1 giornaliero con i devs, uno solo per organizzare lo sprint di 30 giorni, 1 per fare la demo con il cliente di quanto si è fatto nello sprint) |
| Agile - DevOps            | Eliminazione barriera tra team sviluppatori e team operativo. Utilizza pratiche come il build, il deployment e il test automatizzati, e si basa su CI/CD                                                                                               |
| Agile - XP                | Design più semplice possibile, scrivere i test prima del codice e tenerli sempre i run, pair programming, iterazioni corte e tante consegne piccole, conoscenza del codice a tutto il team (non ci sono specialisti in un campo)                       |
