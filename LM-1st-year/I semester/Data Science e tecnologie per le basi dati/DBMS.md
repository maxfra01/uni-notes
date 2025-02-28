# Buffer Manager

E' il componente del DBMS che **si occupa di trasferire i dati dal disco alla memoria principale e viceversa**.
Inoltre si occupa anche della gestione del buffer interno al DBMS: quest'ultimo è un grande blocco di memoria condivisa dalle transazioni in esecuzione.

![[Pasted image 20231117083306.png]]

La memoria su disco è organizzata in **pagine**, di dimensione variabile a seconda delle operazioni di I/O.

Il buffer manager si occupa anche di tenere **informazioni aggiuntive (snapshot) riguardo le pagine presenti nel buffer**, in particolare per ogni pagine si memorizza:
- La location fisica della pagina **sul disco**, perciò si tiene traccia del **FILE_ID** e del **Block Number**
- Variabili di stato: **Count** che conta il numero di transazioni che usano quella pagina, e **Dirty bit** per sapere se la pagina è stata modificata.

### Architetture e primitive

L'organizzazione di un buffer manager e la seguente:

![[Pasted image 20231116120648.png]]

Il DBMS fornisce le seguenti primitive per gestire il buffer: **fix, unfix, set dirty, force e flush**.
Per usare queste primitive serve l'**autorizzazione di accesso** condivisa.
Prima di fornire la pagine in ogni caso si controlla PRIMA il modulo per la concorrenza, per evitare accessi contemporanei.

La **primitiva di FIX** va a richiedere al buffer una pagina su disco:
1. Se la pagina non è in memoria allora c'è un'operazione di I/O per portare la pagina nel buffer
2. Se la pagina è già nel buffer si salta la fase 1
3. Viene incrementato la variabile di stato COUNT della pagina
4. Viene ritornato un puntatore alla pagina per accedere al buffer
Alla fine della primitiva la pagina si troverà nel buffer, valida (ovvero allocata in una transazione attiva) e con il COUNT incrementato.
Nel caso di dover caricare la pagina dal disco bisogna cercare posto nel buffer:
1. Si cerca una pagina vuota, se c'è bene
2. Se non c'è posto libero, si libera uno slot sulla base del COUNT: se è nullo posso sovrascrivere quella pagina (nel caso di dirty bit a 1 è necessario prima scrivere la pagina su disco)

La **primitiva UNFIX** serve solo per informare il buffer che la transazione non sta più usando quella pagina: perciò l'unica operazione che si fa è decrementare la variabile di COUNT di quella pagina.

La **primitiva di SET DIRTY** setta il bit a 1 se la transazione modifica la pagina in questione.

La primitiva di **FORCE** richiede un trasferimento **sincrono** della pagina attualmente in uso al disco: la transazione è bloccata fino al completamento del trasferimento.

La **primitiva di FLUSH** trasferisce indipendentemente le pagine dalla memoria al disco: è una richiesta generata internamente al buffer, eseguita nei tempi morti della CPU sulle pagine che non sono usate da molto tempo o con COUNT nullo.

### Writing strategies

La **politica STEAL** prevede che le pagine dirty appartenenti a transazione **uncommitted** vengano scritte su disco: ovviamente in caso di fallimento queste modifiche devono essere annullate.

La politica **FORCE**, durante il commit, tutte le pagine che la transazione ha modificato vengono scritte su disco in modo sincrono.
**NO FORCE** prevede che dopo il commit non si scriva necessariamente su disco: si scriveranno su disco poi usando FLUSH, quindi in modo asincrono

Tipicamente la **configurazione più usata è steal/no force**.

### File system

Il buffer manager sfrutta alcuni servizi del file system tra cui: creazione/distruzione file, apertura/chiusura, lettura (a blocchi specifici, tramite file id, numero blocco ecc...), lettura sequenziale, scrittura, scrittura sequenziale e gestione delle directory.

# Accesso fisico ai dati

Esistono diversi formati di memorizzazione su disco per soddisfare in modo efficiente le query.
Il blocco di **management of access method** riceve un piano d'accesso (generati dall'ottimizzatore) e li trasferisce come richieste al gestore del buffer: quando riceve le pagine dal buffer è in grado di restituire le informazioni richieste.

![[Pasted image 20231117083414.png]]

Ogni metodo di accesso è capace di scegliere i blocchi su disco che devono essere disponibili per le query.

L'organizzazione di una pagina su disco è la seguente, la pagina si divide in:
- Spazio per i dati
- Spazio per informazioni sul metodo di accesso
- Spazio per informazioni di controllo del file system

Le tuple possono avere lunghezze variabili e valori null: per questo motivo una tupla potrebbe avere uno span di diverse pagine!

### Strutture dati 

Sono usate quando facciamo una query SQL: in particolare vediamo due strutture, ovvero quelle che servono per **memorizzare i dati**, e quelle che servono per ottenere un **accesso efficiente** ai dati (**indici**).

Le strutture **sequenziali** servono per memorizzare i dati in certo ordine: posso però pensare a criteri di ordinamento diversi, come la struttura HEAP.

#### Heap

Le tuple di una tabella sono memorizzare nell'ordine di arrivo (messe in append): prima di iniziare un blocco nuovo devo terminare tutto lo spazio nel blocco corrente.
La modifica di tuple o l'eliminazione crea spazi vuoti e spazio sprecato.

#### Strutture ordinate

In questo contesto l'ordine delle tuple è deciso su un **campo di sort** (es. cognome): sono molto utili quando devo interrogare un database sulla chiave di ordinamento (join, sort e group by, search...)
L'unica **criticità** di queste strutture è l'inserimento di una nuova tupla e il mantenimento dell'ordine ogni volta.
Tipicamente queste strutture ordinate sono abbinate a un indice (come il B tree).

#### Tree

| B-tree | $B^+$-tree |
| ------ | ---------- |
| ![[Pasted image 20231117085809.png]]       |  ![[Pasted image 20231117085827.png]]          |

La B sta per **balanced**: ciò significa che la distanza fra la radice e una qualsiasi foglia è la stessa per tutte le foglie, ciò significa anche tempo di accesso costante indipendentemente dai dati.

Se parliamo di organizzazione **clustered** i dati sono contenuti all'interno della foglia. Invece in caso di indici **unclustered**, le foglie contengono dei puntatori ai dati che sono liberi di essere memorizzati in memoria dove si preferisce. (vedere sotto)

![[Pasted image 20231117091234.png]]

#### Hash index

Le strutture hash garantiscono **accesso diretto tramite una chiave**.
Quando arriva un record applico la funzione di Hash che mi da l'indice per accedere alla zona di memoria.
Clustered e unclustered hanno significato analogo ai b+ tree, se clustered i blocchi hanno i valori, altrimenti hanno i puntatori.

### Bitmap index

E' basato su una **matrice di bit**, che referenzia le righe dei dati attraverso un ROW ID.
Ogni colonna è un possibile valore dell'attributo, mentre ci sono tante righe quante tuple: se quella tupla ha quel valore dell'attributo c'è 1.

![[Pasted image 20231117093009.png]]

# Ottimizzatore

Seleziona la strategia ottimare per eseguire la query: garantisce la proprietà di **indipendenza dei dati** quindi noi dichiariamo cosa fare ma non come viene fatto. In altre parole c'è indipendenza fra le interrogazioni e come sono memorizzati fisicamente i dati.

![[Pasted image 20231117093056.png]]

Il compito dell'ottimizzatore è generare un **piano di esecuzione** che in genere è più efficiente, in quanto esplora tutte le possibili alternative, usando le migliori strategie e si adattano bene al cambiamento dinamico dei dati.

![[Pasted image 20231117094348.png]]

### Controllo sintattico ecc...

Primo compito dell'ottimizzare è il **controllo semantico, lessicale e sintattico**:
- Lessicali: parole scritte male
- Sintattico: errori nell'ordine delle clausole sql
- Semantico: errori nel referenziare strutture che non esistono
L'output di questa fase è una **rappresentazione in algebra relazionale estesa**.

### Algebra

Segue una fase di **ottimizzazione dell'algebra** che restituisce un albero canonico, a cui segue un'analisi dei costi per tutte le possibili scelte:
in output abbiamo il **programma eseguibile dal DBMS** e talvolta un set di dipendenze (da cui dipende la validità del piano della query).

Le **modalità di esecuzione**:
- **Compile and go**: dopo la compilazione si esegue immediatamente il codice, senza bisogno di memorizzare il query plan e senza necessità di dipendente
- **Compile and store**: Il query plan è memorizzato e l'esecuzione è su richiesta

La fase di **ottimizzazione algebrica** ha come obiettivo trasformare l'espressione di partenza in una **equivalente**: in teoria dovrebbe esserci una riduzioni dei dati intermedi da memorizzare in memoria. 
Alcune utili **proprietà** tra cui:
1. Atomicità della selezione
2. Cascata di proiezioni
3. Anticipazione della selezione rispetto al join ($F_{2}$ agisce solo sugli attributi di $E_{2}$)
4. Anticipazione della proiezione rispetto al join 
5. Derivazione del join rispetto al prodotto cartesiano
6. Distribuzione della selezione rispetto all'unione
7. Distribuzione della selezione rispetto alla differenza
8. Distribuzione della proiezione rispetto all'unione

$$

\begin{cases}
\sigma_{F_{1} \land F_{2}} (E) = \sigma_{F_{2}}(\sigma_{F_{1}}(E)) = \sigma_{F_{2}}(\sigma_{F_{1}}(E)) \\
  \\
\pi_{X}(E) = \pi_{X}(\pi_{X,Y}(E)) \\
 \\
\sigma_{F}(E_{1} \bowtie E_{2}) = E_{1} \bowtie (\sigma_{F} (E_{2}) ) \\
 \\
\pi_{L}(E_{1} \bowtie E_{2}) = \pi_{L}((\pi_{L1,J}(E_{1})) \bowtie (\pi_{L2,J}(E_{2}))) \\
 \\
\sigma_{F}(E_{1} \times E_{2}) = E_{1} \bowtie_{F} E_{2}
 \\
  \\
\sigma_{F}(E_{1} \cup E_{2}) = \sigma_{F}(E_{1}) \cup \sigma_{F}E_{2} \\
 \\
\sigma_{F}(E_{1}-E_{2}) = \sigma_{F}(E_{1}) - \sigma_{F}(E_{2}) = \sigma_{F}(E_{1}) - E_{2} \\
 \\
\pi_{X}(E_{1} \cup E_{2}) = (\pi_{X}(E_{1})) \cup (\pi_{X}(E_{2}))
\end{cases}
$$

![[Pasted image 20231122085910.png]]
![[Pasted image 20231122085922.png]]

### Cost based optimization

La **Cost based optimization** è l'unità che si basa anche sulle dimensioni delle tabelle: in particolare occorre fornire all'ottimizzatore delle statistiche sui dati, detti **profili**, per permettere una piano di esecuzione più efficiente.
L'unità stima i costi delle operazioni, esplorando le possibili alternative.

### Data profiles

Sono informazioni quantitative che descrivono le caratteristiche delle tabelle.
Si trovano nel dizionario dei dati.
- Cardinalità della tupla (numero di righe)
- Dimensione in bytes delle tuple
- DImensione in bytes di ogni attributo
- Numero di valori distinti di ogni attributo
- Valore massimo e minimo di ogni attributo

I valori sono aggiornati in automatico, oppure on demand. Nel caso di aggiornamenti automatici, si eseguono quando il sistema è scarico, per non impattare le prestazioni generali.

Sotto l'ipotesi di distribuzione uniforme dei valori degli attributi posso **stimare la dimensione** di una selezione tramite:
$$
\text{Card}(\sigma_{F}(T)) = \frac{\text{Card(T)}}{\text{Val}(A_{i} \in T)}
$$
Ovvero il numero di righe in T diviso il numero di valori differenti che assume l'attributo in T.

### Operatori di accesso

Nella rappresentazione ad albero le foglie corrispondo alle tabelle da leggere. Dobbiamo immaginare come accedere alle strutture fisiche dei dati.

Per eseguire le letture nel modo tradizione è usare una **full sequential scan**, dove **mentre leggo posso già fare delle operazioni (nel buffer) per scartare delle colonne non necessarie** (proiezione), oppure posso direttamente fare delle selezioni per tenere o scartare la tupla, ordinare le tuple, oppure fare insert, update, o delete.

Se ho a disposizione degli indici li posso usare:
- Nel caso di un predicato gli uguaglianza posso usare tutti gli indici (hash, bitmap, b-tree)
- Nel caso di un range di valori solo il B+ tree è appropriato
- Per predicati con selettività limitata (filtra metà dell'insieme) conviene una full scan (oppure bitmap va bene)
- Per congiunzione di predicati il più stringente è valutato prima: si estraggono le colonne bitmap e poi si va a fare l'intersezione

![[Pasted image 20231122093802.png]]

### Gestione dei join

La tecnica più semplice (**brute force o nested loop**) consiste in una scansione esterna completa della tabella di partenza, per ogni chiave che trovo nella outer faccio una scan interna della inner per cercare la corrispondente chiave.
- Approccio non simmetrico, la complessità dipende dal ruolo che assumono le tabelle
- Comodo quando la tabella interna è piccola e sta in memoria

Nella **merge scan** entrambe le tabelle sono ordinate sulla base della chiave di join, poi entrambe sono scannerizzate **in parallelo**.
- Se le tabelle sono molto grandi si dividono in blocchi e si effettua il join su ognuno dei blocchi
- Lo svantaggio sta nel sorting delle tuple

L'**hash join** funziona tramite applicazione della funzione di hash sugli attributi di join: generò quindi dei **bucket** (caratterizzati dalla stessa chiave) dove posso anche avere delle collisioni. Successivamente per ogni bucket se ho una sola tupla li unisco, altrimenti faccio un piccolo sort e unisco.
- La maggior parte delle implementazioni ci si basa su questo

Il **Bitmapped join index** si basa su una matrice, dove righe e colonne si basano sul valore del RID della tabella A e B, dove poi c'è 1 significa che occorre fare il join.

![[Pasted image 20231122095458.png]]

Tipicamente questo tipo di join si usa per query OLAP.

### Gestione di group by

- Primo metodo: si ordina sulla chiave della group by, si creano le partizioni e poi si calcolano gli aggregati. Così facendo un group by **implica sempre un sorting**.
- Usando una funzione di hash sulla chiave del group by, posso creare dei **bucket** di elementi e poi partizionare in base a quei bucket.
### Scelta del piano di esecuzione

Il compito di questa fase è valutare le alternative, tramite calcoli approssimati, per eseguire in un ordine specifico le operazione della query.
Si tengono in conto i seguenti fattori:
- Lettura da disco (full scan, index...)
- L'ordine di esecuzione delle operazioni
- La tecnica con la quale si implementano i join, le group by ...
- L'ordinamento se necessario

Si genera un **albero di alternative** dove ogni nodo rappresenta una decisione su una variabile, mentre le foglie rappresentano i piani di esecuzioni completi.

![[Pasted image 20231123102010.png]]

La strategia generale si basa sui costi $C$ dell'accesso alla CPU e all'I/O:
$$
C_{tot} = C_{I/ O} \cdot n_{I/O} + C_{CPU} \cdot n_{CPU}
$$
Ci si basa inoltre su tecniche di ottimizzazione (branch and bound)

# Gestione delle concorrenze

![[Pasted image 20231129090617.png]]

Grazie al gestore delle concorrenze è possibile incrementare l'efficienza del DBMS stesso e minimizzare il tempo di risposta da parte del sistema.
Lo **Scheduler** è il blocco fondamentale che decide se una certa transazione può eseguire un'operazione base (**read(x), write(x)**). In assenza dello scheduler si generano **anomalie**.

Definiamo una **transazione** come una sequenza di read e write caratterizzate dallo stesso TIP (Transaction ID).
Uno **Schedule** è una sequenza di read e write presentata da transazione concorrenti:
$$
r_{1}(z), r_{2}(z), w_{1}(y) w_{2}(z)
$$
Lo scheduler **deve decidere** se lo scheduler che riceve va bene o no.
L'ipotesi sotto cui lavoriamo è quella che ogni transazione va a fare commit (nessuna fallisce).
Uno **schedule seriale** è composto da read e write che non presentano TID che si sovrappongo, ma quando ne inizia uno finisce :
$$
r_{0}(x), r_{0}(y) ,r_{2}(x), r_{2}(z), r_{1}(x)
$$
Definiamo uno schedule S **serializzabile** se è diverso dallo schedule seriale ma conduce allo **stesso risultato sulla base dati**.

Per valutare due schedule devo ricorre a **classi di equivalenza**.

### View equivalence

Definiamo l'insieme **read from** e l'insieme **final write**.

![[Pasted image 20231129093650.png]]

Se due schedule hanno **questi stessi due insiemi** allora si dice che sono **view equivalent**.
Questo implica che uno schedule, che è view equivalent allo schedule di partenza, è **view serializable**.
Trovare fra tutti i possibili schedule uno che sia view equivalent è un problema NP-completo. (non è realizzabile nel mondo reale, si cercano altre vie).

### Conflict equivalence

C'è il focus solamente su operazioni successive: dobbiamo definire quando **due azioni sono conflittuali**:

![[Pasted image 20231129095250.png]]

Due schedules sono **conflict equivalence** se hanno lo stesso insieme di conflitti, e in ogni coppia conflittuale devo avere lo stesso ordine.
Implicazione: **conflict serializable**.

E' possibile andare a costruire un **grafo** caratterizzato da archi che corrispondono ai conflitti: se il grafo è **aciclico** allora non ci sono problemi.
La ricerca di aciclicità è un problema molto più semplice di quello analizzato con view equivalence.
Tuttavia è necessario notare che questo approccio è molto costoso :(

E' importante sottolineare la relazione fra **Conflict e view**:

![[Pasted image 20231206085036.png]]

### 2 phase locking

L'idea generale proviene dal campo dei sistemi operativi: bloccare temporaneamente l'accesso alle risorse.
Un **lock** è un blocco di una risorsa che potrebbe impedire l'accesso ad altri: posso richiedere lock per operazioni di write e lock per operazioni di read.
- I **read lock** sono **condivisi** tra diverse transazioni
- I **write lock** sono **esclusivi**
Un ulteriore concetto è quello di **lock escalation**: richiesta di  R lock seguito da W lock sullo stesso dato.

Quelli che prima erano i concetti i serializzazione, ora ci limitiamo a considerare un **lock manager** per proteggere l'accesso concorrente.

![[Pasted image 20231206085713.png]]

Il lock manager consulta la **conflict table** per capire come agire:

![[Pasted image 20231206090122.png]]

Per essere efficiente occorre che in memoria ci sia per ogni oggetto al quale si accede devo avere 2 bit per rappresentare i 3 stati (R,W,F) e una coda per le transazioni in attesa.

Per quanto riguarda i metodi di locking implementati realmente, sono divisi in due fasi:
- **Growing phase**, posso richiedere i lock ma non posso liberarli
- **Shrinking lock**, posso rilasciare tutti i lock

Purtroppo questo meccanismo va a scartare gli schedule che sono accettati da CSR e VSR:

![[Pasted image 20231206090834.png]]

Nella versione **strict** il rilascio dei lucchetti si verifica solamente dopo il commit (o rollback) della transazione: così facendo ho la sicurezza di avere dati stabili e senza anomalie.

1. Una transazione richieda una risorsa x
2. Se la richiesta può essere soddisfatta allora il lock manager modifica lo stato della risorsa x e ritorna il controllo alla transazione
3. Se la richiesta non può essere soddisfatta allora la transazione è inserita nella coda di attesa.

Se K è il numero di transazioni, M il numero medio di oggetti acceduti da una transazione ed N il numero di oggetti nel database, allora la **probabilità di un conflitto** è:
$$
P(\text{conflitc})= \frac{K \cdot M}{N}
$$
Ogni attesa in coda è caratterizzata da un timeout, se quest'ultimo scade allora la transazione è rimossa dalla coda e può decidere di fare rollback oppure riprovare ad inserirsi in coda.

### Hierarchical locking

L'idea generale è quella di applicare i lock **a diversi livelli di granularità** fra la tabella intera, frammenti di tabelle o tuple.

![[Pasted image 20231206093243.png]]
![[Pasted image 20231206093256.png]]

1. I lock sono sempre richiesti a partire dalla radice (tabella) per poi andare a scendere di granularità
2. Per il rilascio è il contrario: si parte dalla granularità più bassa e poi si aumenta
3. Per richiedere SL o ISL su un certo nodo, bisogna assicurarsi che la transazione abbia un ISL (o IXL) su nodo parente.
4. Per richiedere XL, IXL il nodo padre deve avere IXL o SIXL

![[Pasted image 20231206093628.png]]

### Deadlock

Si verifica quando due o più transazioni attendono il liberarsi di risorse che non si verifica mai, quindi c'è uno stallo:

![[Pasted image 20231207101850.png]]

Per **risolvere il deadlock** si fa uso di un **timeout**, che caratterizza tutte le primitive sulle transazioni. Dopo lo scadere del timeout la transazione viene tolta dalla coda di attesa e fa rollback (soluzione adottata nei sistemi commerciali).
La criticità sta nello scegliere la durata del timeout:
- troppo lungo: spreco tempo
- troppo corto: se il sistema è carico si scartano le transazioni che sarebbero andate a buon fine (situazione di **overkill**).

Per **prevenire un deadlock** abbiamo tecniche come il **2PL pessimistico**: la transazione inizia solamente quando ha acquisito tutti i lock (non è sempre realizzabile).
Una seconda tecnica è l'uso di **timestamp**: ovvero si permette solamente alle transazioni più giovani di attendere, quelle vecchie invece fanno rollback (può causare overkill).
In ogni caso la prevenzione è complessa.

Per **riconoscere un deadlock** possiamo modellare un **grafo di attesa**, dove ogni nodo è una transazione, l'arco rappresenta l'attesa di una risorsa di un'altra transazione.
Se questo grafo è **ciclico**, allora si ha un deadlock.
E' costoso adottare questa strategia, ma è utile **quando il rollback è costoso**, ovvero nei casi di database distribuiti.

# Gestione dell'affidabilità

Questa caratteristica del DBMS è quella per cui spesso si sceglie una soluzione commerciale (implementa meglio queste tecniche).
La gestione dell'affidabilità è quella che **garantisce** la **durability** e l'**atomicità** (ci assicuriamo che la transazione sia effettivamente persistente nella base dati, e ci assicuriamo che non ci si fermi in stati intermedi della transazione).
In altre parole, questo componente si occupa di `COMMIT` e `ROLLBACK`.

![[Pasted image 20231207102931.png]]

Da ora per **B** si intende Begin, **C** commit e **A** (abort) rollback.
Ci sono due primitive di **recovery**:
- **Warm** restart: per le sole perdite di memoria principale
- **Cold** restart: è la procedura per gestire anche perdite su disco (ed eventualmente su memoria principale)
Nel primo caso dovrò solo riavviare il sistema, nel secondo dovrò anche sostituire qualche pezzo.

### Log files

Il gestore dell'affidabilità dialoga con il buffer manager, intercettando le modifiche alla base dati: genererà quindi altre operazioni di lettura e scrittura su **log files** (file per tenere traccia degli stati e delle azioni d'interesse del DBMS).
Il costo dell'affidabilità è proprio l'aggiunta di scritture e letture. 

I log stanno nella **memoria stabile**: astrazione per dire che i dati in memoria stabile sono **ridondanti** e scritti con **protocolli robusti**, così facendo si garantisce che i log vengano scritti in modo corretto.

Il **contenuto dei log** è composto dalle transazioni eseguite in ordine cronologiche: si tiene traccia di **transactions records** e **system records**.
I record sono scritti nel blocco corrente in ordine sequenziale.

Per i record transactions:
Considero **T** come il TID, ovvero l'identificatore. Nei log compaiano:
- **B(T), C(T), A(T)** delle transazioni
- **Insert** I(T,O,AS), **Delete**(T, O, BS) e **Update**(T,O, BS, AS), dove in particolare **O è l'oggetto scritto**, **AS e BS sono After state e before state** dell'oggetto in questione.

Per i records system:
Si tiene traccia di salvataggi su dischi: abbiamo il **Dump** (backup) e il **Checkpoint** CK(L), dove si memorizza uno stato della base dati con L= insieme di transazioni attive in quel momento.

![[Pasted image 20231207105120.png]]

Per ogni operazione esiste l'operazione di **undo** e **redo**, in particolare grazie alla proprietà di **idempotenza** si riesce a garantire la gestione in caso di guasti.

### Checkpoint

L'operazione di **checkpoint** permette un recovery più veloce della base dati:
- Si scrivono su disco tutte le info per **transazioni completate**, tramite `force`, assicurando la durabilità.
- Si tiene traccia delle transazioni attive.

Fasi dell'esecuzione del CK:
1. Si **bloccano i commit**, durante la fase CK nessuna transazione può essere completata
2. Le pagine di transazioni concluse si scrivono su disco con `force`
3. Il record di checkpoint è scritto sui log, in memoria stabile, anch'esso con `force`

Dopo il commit, le transazioni concluse lo sono in modo permanente.
Per quelle attive lo stato delle pagine è sconosciuto.

### Dump

Si crea una **copia completa del database**: tipicamente si fa quando il sistema è offline, e si salva tutto in memoria stabile.
Dopo i dati si scrive un record con **data e tempo** del dump, e **dispositivo** di dump usato.
In linea teorica il dump va fatto con nessuna transazione attiva

### Regole per scrivere i log

Due tecniche differenti per permettere il recovery: **WAL** e **commit  precedence**.

Il primo caso prevede che il **BS di un dato** che va nel log, sia scritto **prima in memoria stabile** e solo dopo scrivo su disco. Così facendo sono sicuro che nella memoria stabile ho le informazioni per fare recovery.  

La seconda tecnica prevede che **AS di un dato** si scriva nella memoria stabile, e poi effettuare un commit. Come prima si garantisce che ho le informazioni per fare recovery.

Se applico queste due tecniche insieme allora dovrò scrivere il log in modo **sincrono** quando vado a fare modifiche sulla base dati e su commit (Commit, Insert o Update, o delete)
Il log può essere scritto in modo **asincrono** in caso di abort o rollback.

In particolare nel caso di commit, dobbiamo sempre verificare nei log il **record di commit**: dopo un fallimento devo verificare i log e a seconda se trovo o meno quella riga corrispondente deciso cosa fare:
- Se non trovo il record procedo con **undo**
- Se trovo il record devo fare **redone** 

### Recovery management

Due tipi di **failure**: system (perdita di memoria centrale) failure e media failure (perdita di memoria su disco).

![[Pasted image 20231207114312.png]]

### Warm restart

Consideriamo il primo caso di **warm restart** dove abbiamo un guasto che fa perdere i dati in memoria centrale.

![[Pasted image 20231207114633.png]]

Distinguiamo le **categorie**:
- T1 con commit prima di un checkpoint, per loro non devo fare nulla
- T2 Begin prima del checkpoint ma commit dopo il checkpoint, occorrerà fare un redo
- T3 inizia prima del checkpoint e non termina prima del gusto, dovrò fare undo
- T4 inizia dopo e finisce prima del guasto, occorre redo
- T5 inizia dopo e poi c'è il guasto, occorre fare undo

Ulteriori distinzioni sulla **presenza del checkpoint**:
- Se c'è allora il recovery è più veloce
- Se non c'è, devo rileggere tutto il log e spreco tempo

**Algoritmo di Warm restart**:
1. Al guasto si **legge il log all'indietro** fino all'**ultimo checkpoint** 
2. Determino quali transazioni vanno rifatti o annullate: creo due **liste UNDO e REDO** e metto in UNDO tutte le transazioni attive al checkpoint
3. **Leggo in avanti il log** dal checkpoint:
	1. Se trovo un BEGIN aggiungo la transazione all'UNDO
	2. Se trovo un record di commit allora sposto la transazione al REDO
	3. Quando arrivo al **tempo di guasto** le liste sono complete e corrette
4. Ora parte la fase di **data recovery**: si legge al contrario il log dal tempo di guasto fino all'inizio della più vecchia transazione nella UNDO list. Le **UNDO** si fanno proprio in questo **ordine backwards**.
5. Ora si legge il **log in avanti** dalla più vecchia transazione in REDO fino al tempo di crash: man mano che si incontrano i begin si fanno le operazioni di REDO.

### Cold restart

In questo caso si ha un danneggiamento dei dischi della base dati.
Si seguono i seguenti passi:
1. Si accede al più **recente dump (backup)** per ripristinare la porzione danneggiata
2. Da quell'ultimo dump si legge in avanti il log e si effettuano i **redo di tutte le azioni** sul database e le transazioni.
3. Per garantire poi la correttezza si effettua un **warm restart**

Alternativamente ai passi 2 e 3 è possibile fare solo le azione delle transazioni di cui è stato fatto il commit, ma questo richiede due letture del file di log.


Next: [[DBMS distribuiti]].