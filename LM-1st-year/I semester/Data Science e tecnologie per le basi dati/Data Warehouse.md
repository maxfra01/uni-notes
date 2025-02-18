# Data Warehouse, Introduzione

Sono basi dati usate per il supporto alle decisioni, e sono mantenute separate dalle basi dati operative dell'azienda.
I dati contenuti all'interno di un data warehouse sono **orientati ai soggetti di interesse**, **consistenti** e **durevoli nel tempo** e sono d'aiuto per le decisioni aziendali.

I dati sono tenuti in basi separati per diversi motivi: il primo riguarda le **prestazioni**, infatti interrogare un data warehouse potrebbe compromettere le transazioni sulla base dati operativa.
Inoltre esistono diversi mezzi di accesso a livello fisico.
Il secondo motivo è legato alla gestione dei dati: **storico**, **consolidamento dei dati** e **qualità dei dati**.

### Rappresentazione dei dati

I dati in una warehouse sono rappresentati come un **iper cubo** ad n dimensioni: i valori numerici (detti **misure**) sono individuati dall'intersezione delle n dimensioni:

![[Pasted image 20231005115906.png]]

Le tre dimensioni **descrittive** dell'esempio rappresentano ad esempio le vendite di un supermercato.

Una rappresentazione alternativa è quella **a stella**: le 3 dimensioni del cubo diventano **entità del modello ER**, le relazioni legano le entità alle misure (centro stella):

![[Pasted image 20231005120305.png]]

### Architetture per data warehouse

Trattiamo separatamente l'elaborazione OLTP e elaborazione OLAP, quindi dobbiamo necessariamente avere architetture su due livelli (quelle a un livello sono complessi e non presi in considerazione nel corso).

![[Pasted image 20231005121240.png]]

I dati in arrivo dalle sorgenti sono estratti tramite strumenti **ETL** (extraction transform load) e portati nella **data warehouse**:
- E: si acquisiscono i dati dalla sorgente
- T: prima di Puliscono i dati (correttezza e consistenza), poi si convertono nel formato del warehouse (integrazione)
- C: Infine si propagano le informazioni nel warehouse

Sopra al data warehouse sono presenti gli **OLAP server** che servono per rispondere meglio alle interrogazioni di analisi. Sono di 3 tipi:
- ROLAP (Relational OLAP): sono DBMS relazioni estesi con operazioni efficienti per i dati, SQL esteso e potenziato.
- MOLAP (Multidimensional OLAP): in questo tipo di tecnologie i dati sono rappresentati in forma matriciale, perciò sparsi, infatti sono presenti tecniche di compressione e decompressione. Queste soluzioni sono spesso proprietarie.
- HOLAP (Hybrid OLAP) costituiscono soluzioni ibride.

I **Data marts** sono porzioni che poi crescono in maniera omogena per generare il warehouse (sono 'pezzi' che compongono il warehouse): normalmente l'approccio è bottom up, cioè prima si progettano i marts e poi crescendo si passa la warehouse.
Normalmente i marts possono essere:
- alimentati dal data warehouse primario
- alimentati direttamente dalle sorgenti

Gli **strumenti di analisi** prendono i dati e analizzandoli li servono al cliente finale.

I **Metadati** sono dati sui dati, e descrivono tutti gli oggetti e le strutture che fanno parte della base dati.
Servono per trasformazione e caricamento (da dove provengono i dati... ), per la gestione dei dati (formato dei dati ecc...) e per la gestione delle query (codice SQL, CPU usage ecc...)

Concettualmente distinguiamo **due livelli: livello delle sorgenti e livello della warehouse**.
Nelle architetture a 3 livello è presente una **staging area** fra i due livelli di prima: funziona da buffer su cui svolgiamo la fase di ET, prima di essere caricati nel warehouse con gli strumenti L.


# Progettazione concettuale di Data warehouse

Quando si comincia un progetto le aspettative degli utenti sono alte, e ci sono una serie di problematiche come la qualità dei dati OLTP di partenza (dati inaffidabili) e la gestione 'politica' del progetto (collaborazione con i 'detentori' delle informazioni) 

### Analisi dei requisiti

Questa fase raccoglie i dati che servono per realizzare le esigenze delle aziende, si individuano dei **vincoli**.
Il data mart che si andrà a progettare è strategico per una determinata azienda, e sarà alimentato da poche (affidabili) sorgenti.

```ad-note
title: Riconciliazione
La riconcilazione dei dati da parte di diverse fonti è un processo che richiede tempo e molta fatica

```

I **requisiti applicativi** descrivono gli eventi di interesse (ovvero i fatti) che interessano all'azienda (gli scontrini per un supermercato).
Questi fatti sono caratterizzate da dimensione descrittive.
Altro importante parametro è il **carico di lavoro**: per determinare quest'ultimo si ricorre a report aziendali o a specifiche domande in linguaggio naturale (quanti scontrini si fanno al mese).

I **requisiti strutturali** dipendono da:
- **Periodicità** dell'alimentazione, se voglio inserire nuovi dati giornalmente, mensilmente ecc...
- **Spazio disponibile** per dati e strutture accessorie
- **Tipologia di struttura**, ovvero il numero di livelli necessari, se i data mart sono indipendenti o no...
- **Pianificazione del deployment**: come si deciderà di avviare il data mart, e inoltre bisogna considerare un periodo di formazione per i nuovi utenti.

### Progettazione DFM

Si utilizza un modello detto **Dimensional Fact Model**, che per uno specifico fatto definisci **schemi di fatto** che modellano dimensioni gerarchie e misure.
E' un modello grafico e può essere anche usato come documentazione di progetto utile a priori e posteriori.
Il modello si rifà alla rappresentazione dei dati tramite iper cubo, con fatti, dimensioni e misure.

Il **Fatto** che prendiamo in esempio è la vendita di un certo prodotto, che definiamo graficamente come:

Le **dimensioni** aggiuntive sono **data e negozio**.
Ogni fatto è descritto da misure numeriche indicate all'interno del fatto.

![[Pasted image 20231006203829.png]]

Le **Gerarchie** rappresentano una relazione di generalizzazione tra un sottoinsieme di attributi di una dimensione: nella pratica è una relazione funzionale 1:n.

![[Pasted image 20231006204016.png]]

Prendendo in esempio la dimensione NEGOZIO notiamo che un negozio si trova in una sola città (il viceversa è 1:n), data una città c'è una sola regione e così via.
Questi attributi sono necessari per confrontare tramite group by e generare report significativi (esempio, confronto vendite Piemonte e Lombardia).

Per la gerarchia del tempo, oltre alla scomposizione della data possiamo usare un attributo **vacanza** oppure **evento speciale** per verificare se (ad esempio) le abitudini dei consumatori sono influenzate da giorni particolari (come natale...).

Altri componenti del modello DFM sono riportati nell'immagine di sotto:

![[Pasted image 20231006204850.png]]

Il primo componente è l'**arco di opzionalità** (0,1) come nel caso del prodotto dietetico o no.
Gli **attributi descrittivi** non servono per raggruppare ma solo a fornire maggiori dettagli (ad esempio il numero di telefono).

Un concetto importante è al **convergenza** delle gerarchie: ad esempio dei punti vendite sono legate a una certa area geografica (spesso non coincidente con la regione); questa caratteristica geografica nell'esempio è il distretto di vendita che converge poi nello stato.

La **non additività** pone un limite alle interrogazioni che posso fare al modello.
L'**aggregazione** è il processo di calcolo del valore di misure a granularità meno fine di quella presente nello schema di fatto originale.
Le misure possono essere **additive, non additive e non aggregabili**.

```ad-tip
title: Schemi di fatto vuoti
E' possibile inserire eventi che non sono caratterizzati da misure, ma semplicimente utili al calcolo di alcuni aggregati (come COUNT).
Si pensi ad esempio al numero di frequenze di uno studente a lezione: possiamo avere come evento la **frequenza**, con dimensioni matricola, data e corso. Procedere facendo una COUNT raggruppando per corso e studente.
```
### Misure

**Misure di flusso**: misure cumulative su un periodo di tempo, sono aggregabili con tutti gli operatori standard.
Ad esempio la quantità di prodotti venduti.

**Misure di livello**: misurano quantità in specifici istanti (snapshot), e non sono additive lungo la dimensione tempo.
Ad esempio il saldo del conto corrente

**Misure unitarie**: valutate in specifici istanti di tempo ed espressi in termini relativi, inoltre non sono additive lunga nessuna dimensione.
Ad esempio il prezzo di vendita di un prodotto.

### Operatori

**Operatori distributivi**: è sempre possibile il calcolo di aggregati da dati a livello di dettaglio maggiore.
Esempio: sum, min, max

**Algebrici**: il calcolo di aggregati da dati a livello di dettaglio maggiore è possibile in presenza di misure aggiuntive di supporto (ad esempio avg richiede count)

**Olistici**: Non è possibile il calcolo degli aggregati da dati a livello di gerarchia superiore. (moda, mediana)

### Altri costrutti 

![[Pasted image 20231006212107.png]]

La soluzione sotto non va bene, meglio unire le gerarchie e definire i **ruoli** delle due dimensioni.

Le **relazioni molti a molti** nel DFM vengono sostituite con un **arco multiplo**:

![[Pasted image 20231011084135.png]]

Se una certa dimensione contiene molti attributi che possono essere o veri o falsi, allora si parla di **attributo configurazione**: ogni dettaglio può essere Y/N e si usa una stringa binaria per identificare la configurazione:

![[Pasted image 20231011084430.png]]

### Rappresentazione del tempo

Normalmente la variazione dei dati nel tempo è rappresentata dal versificarsi degli eventi.
Ma in alcuni casi possono variare nel tempo anche le dimensioni.

Una prima tecnica di rappresentazione del tempo è quella '**istantanea**': non ci interessa studiare la variazione di un certo dato nel data warehouse.
Pertanto se una persona cambia stato civile, tutti i suoi acquisti sono registrati come 'sposato', e di fatto non si tiene traccia del passato

Tecnica 2: ogni volta che un attributo si modifica si crea una nuova istanza del fatto precedente.
Se una persona cambia stato civile allora ci saranno due istanze della stessa persona che ne tiene traccia.

Tecnica 3: Oltre a istanziare una nuova tupla quando si cambia un attributo, si aggiunge anche due **timestamp** che indicano il periodo di validità di una certa tupla. Inoltre si definisce anche un attributo che permette di ricostruire lo storico della variazione (**master**) a partire dalla prima istanza originale.

### Carico, volume, sparsità

Il **Carico** è stimato e definito in fase di analisi con gli utenti: tuttavia il carico è difficilmente stimabile durante la progettazione.
Dopo l'avviamente del sistema la fase di **tuning** è necessaria per monitorare il carico reale.

Il **Volume** si stima per lo spazio occupato dai dati e dalle strutture accessorie: inoltre dipende dal numero di fatti, di gerarchie, dalla lunghezza degli attributi e dalla gestione del tempo nel data mart.
Questo problema è condizionato dalla **sparsità dei dati**.
- La sparsità si riduce al crescere del livello di aggregazione dei dati
- Può ridurre l'affidabilità della stima della cardinalità dei dati

![[Pasted image 20231011091957.png]]


# Progettazione logica di Data Warehouse

L'obiettivo di questa fase è quella di **produrre in output delle tabelle**.
La seguente tecnica è **lo schema a stella.

Per **ogni fatto ci sarà una tabella**:
- le misure diventano attributi
- le dimensioni del fatto diventano chiavi

Per **ogni dimensione ci sarà una tabella**:
- la chiave primaria è detta surrogata (generata artificialmente)
- contiene tutti gli attributi della dimensione
- gerarchie non rappresentate esplicitamente
- ridondanza dei dati, quindi non c'è forma normale

Si fa un trade off tra spazio (molto spazio occupato) e efficienza (non ci sono join da fare ad esempio con la città): la normalizzazione si **mantiene solo sul fatto**, mentre sulle dimensioni non c'è normalizzazione.

![[Pasted image 20231011093009.png]]

```ad-seealso
title: Schema snowflakes
Stesso concetto di base della stella, ma ottimizza la dimensione occupata dei dati: le gerarchie diventano tabelle esterne con ID come chiave esterna.
Normalmente il tradeoff è poco vantaggioso e lo schema è poco usato nella progettazione di data mart.

![[Pasted image 20231011093541.png]]

```

### Rappresentazione molti a molti

Per quanto riguarda la traduzione di **archi multipli** abbiamo due possibilità:
- **Bridge table** che specifica la relazione molti a molti ed introduce un nuovo attributo 'peso' che indica la partecipazione delle tuple nella relazione
- **Push down**: in questa strategia l'arco multiplo è integrato nella tabella dei fatti con un'apposita chiave

![[Pasted image 20231011094013.png]]

Dagli archi multipli si creano due tipi di interrogazione distinti: il primo tipo è l'interrogazione **pesata** che tiene conto della partecipazione di un certo attributo in una relazione; ad esempio se voglio calcolare l'incasso delle vendite di libri per autore

```
SELECT ID_Autore, SUM(Incasso*Peso)
FROM...
...
GROUP BY ID_Autore
```

Al contrario **le interrogazioni d'impatto** non considerano il peso: ad esempio come la somma dei libri venduti per autore:

```
SELECT ID_Autore, SUM(Quantità)
FROM...
...
GROUP BY ID_Autore
```

I pro e contro della bridge table e del push down sono i seguenti:

| Bridge Table                                  | Push Down                                   |
| --------------------------------------------- | ------------------------------------------- |
| semplice                                      | E' difficile fare interrogazioni di impatto |
| Minore ridondanza                             | Maggior ridondanza dei dati                 |
| Costo di esecuzione maggiore dovuto alle join | Meno join, quindi più efficienza di esecuzione                                            |

### Dimensioni degeneri

Sono **dimensioni rappresentate da un solo attributo**.
Per integrare queste dimensioni (spesso indipendenti fra di loro) posso o facendo un **push** della tabella dei fatti (solo per piccole dimensioni).
In alternativa posso creare una **junk dimension** ovvero una tabella che accumula tutte queste dimensioni.

Nell'esempio qui sotto la dimensione MCS contiene solo dimensioni degeneri:

![[Pasted image 20231011095715.png]]


# Analisi di Data Warehouse

Per analizzare i dati presenti è necessario calcolare **aggregati**, anche di complessità maggiore (top ten, media mobile), effettuare **confronti** ed utilizzare le tecniche di **data mining**.
Abbiamo diversi strumenti per l'analisi:
- **Ambiente controllato da query**: comprende ricerche complesse con **struttura prefissata** (cruscotto), rapporti con struttura prefissata. Offre una vista a colpo d'occhio per monitorare i dati, e richiede la scrittura del codice ad hoc.
- **Ambiente di query ad hoc**: in questo contesto è possibile definire interrogazioni sul momento tramite interfacce simili a fogli di calcolo (pensato per non informatici)
- Specifici strumenti di query
- Data mining

### OLAP (Online Analytical processing)

Una prima tecnica di analisi è il **ROLL UP** che consiste nel **ridurre il livello di dettaglio**, con l'aumento di un livello nella gerarchia (ad esempio, partendo da group by negozio, mese passo a group by città, mese).
Alternativamente anche **eliminando una dimensione** ottengo lo stesso risultato.

![[Pasted image 20231012103121.png]]

La tecnica inversa è detta **DRILL DOWN**, consiste nell'**aumentare il livello di dettaglio**, quindi procederò o aggiungendo una dimensione oppure scendendo in profondità nella gerarchia.
Spesso il drill down opera su un sottoinsieme dei dati di partenza: considerando le vendita raggruppate per regioni, procediamo ad analizzarne SOLO UNA:

![[Pasted image 20231012104019.png]]

Terza tecnica: **SLICE AND DICE** consiste nella selezione di un sottoinsieme dei dati tramite **predicati**: nel caso di un predicato su una dimensione allora estrarrò una 'fetta' del cubo, nel caso di più predicati su più dimensioni selezionerò 'cubetti'.

![[Pasted image 20231012104245.png]]

Quarta tecnica: **PIVOT** consiste nel **riorganizzare i dati senza variare il livello di dettaglio** in modo da permettere una migliore visualizzazione delle informazioni. 

![[Pasted image 20231012105047.png]]

### Estensioni del linguaggio SQL

Nuova clausola **WINDOW** che definisce il partizionamento delle tuple senza modificarne la cardinalità.
Consideriamo un esempio:

![[Pasted image 20231012110512.png]]

Visualizzare, per ogni città e mese:
- l’importo delle vendite
- la media rispetto al mese corrente e ai due mesi precedenti, separatamente per ogni città

```SQL
SELECT Città, Mese, Importo,
	AVG(Importo), OVER (PARTITION BY Città
						ORDER BY Mese
						ROWS 2 PRECIDING)
FROM Vendite
```

La **finestra mobile** è definibile in due modi:
- Livello **fisico**, andando ad indicare un conteggio delle righe su cui operare
- Livello **logico**: formando un gruppo in base alla definizione di un intervallo intorno alla chiave di ordinamento.
La keyword **ROWS** definisce l'intervallo fisico:
- ROWS BETWEEN 1 PRECIDING AND 1 FOLLOWING indica la riga corrente, precedente e successiva
- ROWS BETWEEN 3 PRECIDING AND 1 PRECEDING non include la riga corrente ma include le righe -3,-2,-1
- ROWS UNBOUNDED FOLLOWING indica tutte le righe dopo quella corrente (fino alla fine della partizione)

E' anche possibile **combinare** group by e over per, prima semplificare i dati e successivamente andare a fare aggregati particolari.

C'è la possibilità di utilizzare la funzione **RANK()** o **DENSERANK()** per fare la classifica sulla base di una OVER con un ORDER BY interno.

Una ulteriore estensione del GROUP BY serve per calcolare più combinazioni di aggregati: in questo modo si evitano lettere multiple dei dati e query ridondanti.
Ad esempio con la sintassi GROUPU BY ROLLUP(Città, Mese, Anno) calcoliamo la query per tutte le possibili combinazioni di quei 3 attributi.
L'ordine con cui si vanno a togliere gli attributi e quello specificato: nell'esempio avremo CittaMeseAnno, MeseAnno, Anno, -

Con la sintassi GROUP BY CUBE(...) andiamo ad eseguire la query per tutte le possibili combinazioni delle dimensioni specificate.

Infine GROUPING SET(...) permette di specificare le singole combinazioni per le quali eseguire la query.

### Viste materializzate

Le **viste materializzate** sono memorizzate esplicitamente nel data warehouse, questo consente di aumentare l'efficienza della interrogazioni che richiedono aggregazioni.


# Progettazione fisica del Data Warehouse

Questo processo dipende da numerosi fattori:

| Parametro                     | Note                                                                                                                                  |
| ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| Carico di lavoro              | In un data warehouse ci sono interrogazioni complesse che richiedono accesso (in sola lettura tipicamente) a grandi quantità di dati. |
| Strutture fisiche             | Necessarie strutture non tradizionali: trattati più avanti. Sono usate spesso viste materializzate.                                                                          | 
| Ottimizzatore                 |Deve essere evoluto, serve capacità di riconoscere ed utilizzare viste materializzate                                                                                                                                       |
| Procedimento di progettazione | Progetto specifico volto ad ottimizzare esigenze specifiche di progetto: ho vincoli di spazio e tempo                                                                                                                                       |
| Tuning                        | Posso variare a posteriori le strutture fisiche di supporto, senza modificare le applicazioni                                                                                                                                       |
| Parallelismo                  |Se le strutture lo permettono, parallelizzazione di operazioni complesse                                                                                                                                       |

### Scelta degli indici

- Indicizzazione delle **dimensioni**
- Indici per i **join**
- Indici per i **group by**

# Alimentazione

Processo base dell'alimentazione è costituito dalla fase **ETL**, extract, transform e load.
L'**estrazione** è l'acquisizione dei dati dalle sorgenti, avviene in due modi:
- **statico**: è una fotografia dei dati operazionali 
- **incrementale**: selezione degli aggiornamenti avvenuti dopo l'ultima registrazione
In ogni caso scelgo di estrarre i dati in base alla loro qualità.

### Estrazione statica

L'estrazione avviene in base alle informazioni temporali che possiedo riguardo ai dati:
- **Storicizzati**:ho già tutte le modifiche memorizzate nel sistema OLTP
- **Semi storicizzati**: Solo un numero limitato di stati sono memorizzati nel sistema OLTP
- **Transitori**: Il sistema OLTP mantiene solo lo stato più recente del dato

### Estrazione incrementale

Normalmente questo processo è assistito da un'applicazione: **modifico opportunamente l'applicazione OLTP** per permettermi di salvare dati.
Questo richiede di aumentare il carico, costoso, complesso.

Un secondo metodo consiste nell'uso di **log**, in formato proprietario, per tenere traccia di ogni transizione nel sistema. Questo metodo è efficiente in quanto non impatta sul carico applicativo. 

Terzo modo: **uso dei trigger** per tenere traccia delle modifiche di interesse. Non modifico le applicazioni già esistenti ma aumento il carico applicativo

L'ultima soluzione è basato su **timestamp**: modifico opportunamente gli schemi logici per inserire un nuovo attributo per il tempo. Ogni transazione è marcata dal tempo e quindi indirettamente posso tener traccia dei dati. E' efficiente ma richiede la modifica di schemi e applicazioni (problema: se i dati sono transitori si possono perdere alcuni stati).

![[Pasted image 20231013091603.png]]

### Pulitura

Operazioni volte al miglioramento della qualità dei dati (correttezza e consistenza):
- dati duplicati
- mancanti
- uso non previsto di campo
- valori impossibili o errati
- inconsistenze
I problemi sono dovuti a errore umano, differenze di formato nei campi e evoluzione di paradigmi in azienda.

Ogni problema richiede una tecnica specifica di soluzione:
basate su dizionari per gli errori di battitura, tecniche di fusione approssimata. In ogni caso la **prevenzione** da errori è la migliore soluzione.

Join approssimato:

![[Pasted image 20231013093153.png]]

Problema Purge/Merge:

![[Pasted image 20231013093201.png]]

### Trasformazione e caricamento

I dati acquisiti vanno **normalizzati, standardizzati e poi corretti**. Per fare ciò si prevede un'integrazione dei dati per convertirli nel formato della base dati: questo prevede una **schema riconciliato** uniforme dei dati.
Altre metodologie di questa fase sono il filtraggio dei dati significativi, l'aggregazione, la generazione di chiavi surrogate e di valori aggregati

Dopo questa fase c'è il **caricamento** che prevede di aggiornare **in ordine** (per mantenere integrità):
1. Dimensioni
2. Tabelle dei fatti
3. Viste materializzate ed indici

![[Pasted image 20231013094442.png]]

![[Pasted image 20231013094525.png]]

[[Esercizio viste materializzate + trigger]]
[[Ditta Elettrodomestici]]
[[Eccellenze Made in Italy]]