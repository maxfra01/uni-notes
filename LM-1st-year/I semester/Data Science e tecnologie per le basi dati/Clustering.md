# Introduzione

Il **clustering** è un processo per il quale si cerca di raggruppare oggetti simili, senza etichetta di classe, andando quindi a ridurre la cardinalità del dataset.

Definiamo il **clustering** come un insieme di **cluster**. Tra insiemi di cluster esistono due definizioni:
- Il **partitional clustering** divide i dati in cluster che non si sovrappongono, facendo si che ogni oggetto sia in uno e un solo cluster
- **hierarchical clustering**: I dati sono dividi in nested cluster organizzati secondo un albero.

 Possiamo inoltre effettuare le seguenti distinzioni:
![[Pasted image 20231115090407.png]]

# Tipi di cluster

Un primo tipo è il **well separated cluster**, dove un cluster è un insieme di punti dove ogni punto è più vicino a qualsiasi altro punto del cluster rispetto agli altri non nel cluster.

![[Pasted image 20231115090629.png]]

Un secondo tipo è il **centre-based cluster**: un cluster (insieme di punti) in cui un qualsiasi punto è più (vicino) simile al centro del cluster stesso rispetto al centro di un qualsiasi altro cluster.
Il centro del cluster può essere un:
- **Centroid**, ovvero è la media di tutti i punti del cluster
- **Medoid**, ovvero il **punto più rappresentativo** del cluster

![[Pasted image 20231115090909.png]]

Il terzo tipo è **Contiguity-based cluster**: ogni punto del cluster è più vicino (simile) a uno o più punti del cluster stesso, rispetto ad altri punti non nel cluster.

![[Pasted image 20231115091022.png]]

Quarto tipo: il **density-based cluster**, dove il cluster è visto come una regione con un'alta densità di punti, separata da regione con bassa densità

![[Pasted image 20231115091156.png]]

Ultimo tipo: **cluster concettuale (shaped)**: sono cluster che condividono caratteristiche simili e servono per rappresentare un concetto particolare.

![[Pasted image 20231115091250.png]]

# K means Clustering

E' un approccio di tipo **partitivo**, che usa il concetto di centroide.
L'idea base è quella di partire da un insieme di centroidi, ogni punto nuovo che ho viene assegnato al centroide più vicino (il numero $K$ è proprio il numero di centroidi).

1. Prendo $K$ punti a caso dal set iniziale trattandoli come centroide
2. Ripeto
	1. Formo $K$ cluster assegnando tutti i punti al centroide vicino
	2. Ricalcolare il centroide per ogni cluster
3. Smetto quando la posizione dei centroidi non cambia

La distanza è quella euclidea, e generalmente questo algoritmo converge abbastanza rapidamente dopo le prime iterazioni.

La complessità è $O(nKId)$ dove $n$ è il numero di punti, $K$ è il parametro dell'algoritmo, $I$ il numero di iterazioni e $d$ il numero degli attributi.

### Valutazione del k means

La misura più utilizzata è la **SSE (Sum squared error)**:
$$
\text{SSE}= \sum_{i=1}^k \sum_{x \in C_{i}} \text{dist}^2(m_{i},x)
$$
dove $x$ è il punto che stiamo considerando nel cluster $C_{i}$, mentre $m_{i}$ è il punto rappresentativo del cluster.

### Come scegliere i centroidi

- Una prima soluzione è fare run multiple dell'algortimo ma non è una soluzione ottimale, a causa della probabilità.
- Una seconda soluzione è **considerare più di k centroidi** e poi sceglierne un sottoinsieme
- Posso fare del postprocessing,
- Posso usare i **clustering gerarchico** e poi lavorare con k means,

### Come scegliere K

Possiamo usare un approccio grafico (**Knee approach**), dove posso vedere al crescere di k il valore dell'SSE: scelgo $k$ come il ginocchio della curva, ovvero dove si spiana maggiormente rispetto a prima (qui sotto ad esempio 5).

![[Pasted image 20231115094148.png]]

### Cluster vuoti

L'algoritmo K means può generare dei cluster vuoti: questo accade se il centroide iniziale è scelgo in un punto con bassa densità di punti vicini.
Posso andare ad eseguire diverse operazioni per ovviare al problema: ad esempio eliminare il punto che contribuisce di più all'SSE.

### Pre e post processing

Nella fase di preprocessing:
- Normalizzo i dati
- Elimino gli outliers

Nella fase di postprocessing dovrò:
- Eliminare i cluster più piccoli perchè potrebbero rappresentare degli outliers
- Dividere i cluster molto ampi (con alto SSE)
- Fondere cluster vicini con relativo basso SSE

### Bisecting k means

Questa variante del k means può produrre un partitional o hierarchical cluster. Segue il seguente algoritmo:

![[Pasted image 20231115094809.png]]

# Hierarchical clustering algorithm

Processo che produce un **insieme di cluster nidificati** che possono essere visti come albero gerarchico: è possibile visualizzare i dati tramite il solito diagramma degli insiemi e con un **dendrogram**.

![[Pasted image 20231116101503.png]]

Il dendrogram ha sull'asse orizzontale i diversi dati (pallini blu) e sull'asse y ha la distanza alla quale c'è la creazione del cluster (la distanza fra i punti).

Il vantaggio del clustering gerarchico è il **non dover scegliere a priori il numero di cluster** $k$ (come richiesto dal clustering partitioning): è possibile ottenere il numero di cluster che voglio andando a 'tagliare' il dendrogram.

### Agglomerative clustering

E' l'approccio più usato per il clustering gerarchico.
Procedo nel seguente modo:

1. Calcolo la matrice di prossimità
2. Ogni punto inizialmente è un cluster
3. Ripeto 
	1. Unisco i due cluster più vicini
	2. Ricalcolo la matrice di prossimità
4. Termino quando rimane un singolo cluster

L'operazione **più importante è il calcolo della prossimità fra cluster**.

### Inter-cluster similarity

Dobbiamo quindi trovare un modo per calcolare la somiglianza fra cluster: abbiamo diversi approcci.
- **MIN**: consideriamo la distanza fra 2 cluster come la minima distanza fra qualsiasi 2 punti appartenenti ai cluster 
  ![[Pasted image 20231116102409.png]]
  
- **MAX**: analogo alla precedente ma opposto 
  ![[Pasted image 20231116102425.png]]
  
- **GROUP AVARAGE**: prendo tutte le distanze di tutti i punti da tutti gli altri punti dell'altro cluster e ne faccio la media
  ![[Pasted image 20231116102500.png]]
  
- **DISTANCE BETWEEN CENTROID**

Un metodo alternativo a questi elencati sopra è il **Ward's method**: la somiglianza fra cluster è basato sull'incremento dell'errore al quadrato quando i due cluster sono fusi insiemi.

### Complessità dell'algoritmo gerarchico

I vantaggi degli algoritmi gerarchici sono influenzati dal fatto che la complessità in termini computazionali è elevata: in particolare lo spazio in memoria è $O(n^2)$ dove $n$ è il numero di dati, dovendo memorizzare la matrice di prossimità.
In termini di tempo l'algoritmo ha complessità cubica $O(n^3)$

# DBSCAN

Si basa sulla scansione della **densità dei dati**: in particolare si sceglie un raggio dello sferoide che voglio definire, poi si sceglie il numero minimo di punti (minPTS).
Se un certo punto, all'interno del raggio scelto, supera una certa soglia di punti, allora è detto **punto core** e corrispondono al punto più interno del cluster.

Un **punto di bordo** ha nel suo raggio meno di minPTS punti, ma ricade nella sfera di influenza di un cluster.

Un **noise point** rappresenta tutti i punti che non ricadono nelle precedenti categorie.

![[Pasted image 20231116104734.png]]

Così facendo possiamo etichettare i noise points e l'**algoritmo confina questi punti di rumore in un unico cluster** (cluster del rumore).

# Valutazione di cluster

Vogliamo valutare i cluster per: evitare di trovare dei pattern nel rumore, confrontare i dati, confrontare i cluster e confrontare gli algoritmi.

1. Voglio capire se effettivamente ci sono delle strutture non casuali nei dati, quindi se ha senso clusterizzare.
2. Posso fare una comparazione fra algoritmi a partire da un dataset con etichette note, per vederne l'accuratezza.
3. Valutare  il risultato dell'operazione di clustering (SSE)
4. Confrontare due risultati di clustering per vedere quale è venuto meglio.
5. Determinare il numero corretto di cluster.

### Misure per la cluster validity

- **Indici esterni**: servono per confrontare i risultati del cluster con le etichette del training set
- **Indici interni**: mostrano la qualità del clustering senza informazioni esterne
- **Indici relativi**: usati per confrontare fra cluster

Per quanto riguarda gli indici interni posso definire la **coesione fra cluster** per misure quanto sono vicini fra loro gli oggetti di un cluster (basato su SSE):
$$
WSS = \sum_{i}\sum_{i \in C_{i}} (x-m_{i})^2
$$
Posso definire la **separazione fra cluster** per vedere quanto due cluster sono distanti fra loro:
$$
BSS = \sum_{i}|C_{i}|(m-m_{i})^2
$$
Posso analizzare queste due grandezza usando i grafi, con i rispettivi pesi dei link.

![[Pasted image 20231116111243.png]]

### Silhouette

Si va a prendere **in esame un punto singolo e si verifica quanto bene è stato collocato** all'interno di un cluster: è basato sul concetto di coesione e separazione.

Per ogni oggetto $i$ vado a calcolare $a(i)$ cioè la distanza media di $i$ da tutti gli altri punti appartenenti allo stesso cluster (simile alla coesione).
Poi vado a calcolare $b(i)$: calcolo la distanza di $i$ da ogni altro cluster che non è il suo, infine prendo il minimo di queste distanze (come fosse la separazione).
La **Silhouette** è quindi definita come:
$$
s(i)=\frac{b(i)-a(i)}{\max\{a(i),b(i)\}}
$$
Questo valore oscilla fra -1 e 1, e meglio ci avviciniamo a 1 e meglio è. Posso a questo punto calcolare $s(i)$ per tutti i punti del cluster per giudicare globalmente il cluster.

### Rand index

Ultimo indice che è molto utilizzato, la cui idea è: **qualsiasi due oggetti appartenenti allo stesso cluster devono appartenere alla stessa class**e.
Partendo da questa idea posso confrontare il cluster con le etichette (se le ho), oppure posso confrontare algoritmi diversi.
Definisco le seguenti grandezze:

![[Pasted image 20231116112125.png]]

Posso definire il rand Index come:
$$
\text{Rand Index} = \frac{f_{00}+f_{11}}{f_{00}+f_{01}+f_{10}+f_{11}}
$$
