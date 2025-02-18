# Data Mining: Intro

Introduzione su [[Data Science Intro]].
- **Metodi descrittivi**: si estraggono modelli interpretabili che descrivono i dati (ex. raggruppare clienti che hanno abitudini simili)
- **Metodi predittivi**: Analizzare dati noti per effettuare classificazioni su dati sconosciuti

# Data Mining: Data types

Per prima cosa prendiamo in esame i **tipi di dato**: consideriamo un insieme di dati (noto come 'dataset') contenente una serie di **oggetti**, caratterizzati da  **attributi**.

### Attributi 

Gli attributi vanno distinti dal valore degli attributi, ad esempio consideriamo una stessa quantità in diverse unità di misura.

Gli attributi si dividono in:
- **Nominal**: normalmente rappresentati da stringhe, su cui ha senso applicare solo operatori di **distinzione** $= \neq$
- **Ordinal**: sono ad esempio ranking da 1 a 5, hanno senso solo operazioni di **distinzioni e ordine** $= \neq <> \leq \geq$
- **Interval**: Valori intervallari, hanno senso operazione di distinzione, ordine, addizione
- **Ratio**: Sono misure di fatto, sono sensate tutte le operazioni

Un altro aspetto da considerare è il range di valori assumibili da un attributo:
**Discreti** e **Continui**.

### Record Data set

Si distinguono in diverse tipologie: i **Record** si dividono a loro volta in **Tabular, Document, Transactional**.

I Tabulari sono normali tabelle con attributi fissi.

Poi abbiamo i document, divisi in semi-strutturati(CSV, JSON) e non strutturati (una mail).
Per convertire un documento in dati utilizzabili si procede a creare un vettore di tutti i documenti, e ogni parola che compare nei documenti forma un altro vettore: le misure saranno il numero di volte in cui una parola compare in un documento. Così facendo si perde l'ordine della parole e quindi il senso del documento.

![[Pasted image 20231020113144.png]]

Infini i dati transazionali: speciali tipi di dati in cui ogni record contiene un insieme di oggetti, ad esempio uno scontrino della spesa.

![[Pasted image 20231020113443.png]]

### Graph Data set

![[Pasted image 20231020113558.png]]

### Qualità dei dati

Per **Rumore** si intende una sovrapposizione di informazione di disturbo sul segnale che vogliamo analizzare. 
Un **Outlier** è un oggetto che ha caratteristiche molto diverse dalla maggior parte degli oggetti all'interno del dataset:
- Possono essere trattati come interferenze (simile al rumore)
- Possono essere proprio l'oggetto delle mie analisi

I **Valori mancanti** derivano da diverse ragioni:
- L'informazione non è stata raccolta
- Alcuni attributi non si addicono a tutti i record
Per gestire i valori mancanti:
- si può direttamente eliminare i record
- si può stimare il dato mancante
- si può ignorare la mancanza del dato

I **Dati duplicati** possono essere un problema quando si cerca di fondere due o più dataset da differenti fonti.
Bisogna effettuare processi di data cleaning, o certe volte è necessario mantenere i duplicati.

# Data Mining: Preparation

### Aggregazione

Un primo obiettivo dell'**aggregazione** è quello di **ridurre** **il** **numero** **dei** **dati**: cerchiamo ad esempio di **fondere due attributi** in uno solo, oppure cambiando la **visibilità** con cui guardiamo i dati, inoltre avere meno dati consente di avere più **stabilità**. (ex. trattare vendite a livello di regione e non di città)

![[Pasted image 20231025084934.png]]

Diventa quindi un obiettivo avere una dimensione del dataset ridotta: in questo modo elaborando meno dati possiamo fornire analisi simili all'originale.
Tecniche più utilizzate per ridurre la dimensione del dataset:
- **Sampling**, riduce la cardinalità del dataset
- **Feature** **Selection**, riduce il numero di attributi
- **Discretizzazione**, riduce la cardinalità del dominio degli attributi

### Sampling

E' una tecnica preliminare per analizzare il dataset: prevede un approccio statistico perchè analizzare per intero il dataset è troppo costoso.
I principi di base per cui questo meccanismo funziona sono:
- Usando un campione le cose funzioneranno bene come se usassimo il dataset completo, se il campione è **rappresentativo**
- Un campione è **rappresentativo** se ha approssimativamente le stesse proprietà di interesse del dataset originale.

Esistono due tipi di campionamento:
- **Campionamento casuale classico**: C'è la **stessa possibilità** di estrarre un qualsiasi elemento del dataset, inoltre è possibile avere la versione di campionamento con reimmissione e senza reimmissione.
- **Campionamento stratificato**: Si divide il dataset in diverse partizioni, dopodichè si estraggono campioni casuali dalle partizioni.

![[Pasted image 20231025090300.png]]

### Dimensionality reduction

Quando la dimensionalità diventa più grande i dati diventano sempre più sparsi nello spazio in cui li analizziamo.
Consideriamo di generare 500 punti casuali e calcoliamo la distanza massima e minima fra due punti qualsiasi:

![[Pasted image 20231025090555.png]]

Dal grafico notiamo che al crescere delle dimensioni, le misure delle distanze massime e minimi diventano sempre più simili.
Questo fenomeno è detto **Curse of dimensionality**.

Il nuovo obiettivo ora è quello di ridurre il numero di attributi: così facendo vado ad evitare la *curse of dimensionality*, riduco lo spazio usato in memoria, rendo i dati visibilmente migliori e riduco rumore e features inutili.
Le tecniche per fare ciò sono:
- **PCA**
- **Singolar value decomposition**
- Altre: **supervised and non-linear techniques**

Nel caso della PCA l'obiettivo è quello di creare **combinazioni lineari** degli attributi per diminuire il numero di attributi, e in modo da catturare un grande quantitativo di dati:

![[Pasted image 20231025091141.png]]

### Feature subset selection

Questa è un'altra tecnica per ridurre il numero di attributi, selezionando un **subset delle features** originali.

Un primo modo è quello di eliminare le **features ridondanti**:
- In alcuni casi sono informazioni duplicate
Altre features sono invece **irrilevanti**:
- Non ci sono informazioni utili per il processo di data mining 

Le tecniche per selezione un sottoinsieme di attributi sono:

| Nome        | Approccio                                                                                                                                  |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| Brute-force | Posso dare in input agli algoritmi di data mining tutte le possibile combinazioni di attributi, dopodichè mi scelgo quella più conveniente |
| Embedded    | La scelta degli attributi è lasciata all'algoritmo stesso                                                                                                                                           |
| Filtri      | Si filtrano gli attributi di interesse prima di applicare l'algoritmo                                                                                                                                            |
| Wrapper     | Applica l'algoritmo di data mining (in modo black box) su una **piccola selezione di subset di feature**, scegliendo il migliore                                                                                                                                           |

### Feature Creation

Questo processo va a **creare nuove features sulla base di quelle originali**, andando a non perdere informazioni. Ci sono 3 metodi per fare questo:
- **Feature Extraction**
- **Mapping data to new space** (ex. TdF)
- **Feature construction**

### Discretizzazione

Processo che si basa sul **convertire un attributo con valore continuo in uno con valore ordinale**.
Potenzialmente un'infinita fascia di valori può essere mappata in un piccolo insieme di variabili ordinali. E' un processo usato soprattutto in casi di **classificazioni**.

Consideriamo il seguente scenario:

![[Pasted image 20231025092944.png]]

Come fare per creare discretizzazioni ottimali per la lunghezza del petalo?
- **Unsupervised discretization**: Se ci sono grosse interruzioni fra i dati allora si crea automaticamente una partizione
- **Supervised discretization**: Si usano label di classe per creare le partizioni

### Binarization

Questo processo **trasforma un attributo in uno o più attributi binari**.
- **Continuous attribute**: in questo caso si mappa un attributo in poche categorie (ex. LOW, MEDIUM, HIGH)
- **Categorical attribute**: Si mappa un attributo in cifre binarie, il più usato è **one hot encoding** (su n bit, solo uno di questo è settato a 1)

### Attribute Transformation

E' una funzione che mappa un intero insieme di valori di un attributo in una nuova scala di valori differenti.
Un esempio pratico è la **normalizzazione**, dove trasformo un valore con un dominio qualsiasi in un attributo con dominio \[0,1\] o \[-1,1\]

![[Pasted image 20231025094930.png]]

# Data mining: Similarity and dissimilarity

La **similarity** è la misura di **quanto** **due oggetti sono simili**, e spesso può assumere un valore compreso fra 0 e 1.
La **dissimilarity** è la misure di **quanto due oggetti sono diversi**, e spesso la minima dissimilarità è 0 (oggetti simili).

![[Pasted image 20231027084016.png]]

### Distanza Euclidea

Normalmente per misurare la distanza fra due punti utilizziamo la **distanza euclidea**:
$$
d(x,y)=\sqrt{ \sum_{K=1}^n (x_{k}-y_{k})^2}
$$
Dove $n$ è il numero di dimensione, mentre $x_{k},y_{k}$ sono i valori del k-esimo attributo degli oggetti $x$ e $y$.
Ovviamente è necessario **standardizzare** le variabili prima di calcolare le distanze.

### Distanza di Minkowski

Per generalizzare le misure di distanza possiamo usare la distanza di **Minkowski**:
$$
d(x,y)=\left( \sum_{k=1}^n |x_{k}-y_{k}|^r\right)^ {1/r}
$$
A seconda del valore di $r$ possiamo valutare il significato della distanza:
- Quando $r=1$ allora si parla di **City Block**
- Quando $r=2$ questa distanza coincide con quella euclidea
- Quando $r \to \infty$ si parla di **norma $L_{MAX}$**.

Alcune proprietà delle distanze sono:
- **Non negatività** $d(x,y)\geq 0$
- **Simmetria** $d(x,y)=d(y,x)$
- **Disuguaglianza triangola**: $d(x,z) \leq d(x,y)+d(y,z)$

La proprietà di simmetria vale anche per la similarity.

### Distanza di Mahalanobis

Presi due punti A e B notiamo quando lo spazio fra i due punti è popolato e in base a quello calcoliamo la distanza:
$$
\text{mahalanobis}(x,y) = (x-y)^T \, \Sigma^{-1}(x-y)
$$

![[Pasted image 20231027085638.png]]

### Similarity fra vettori binari

Consideriamo due vettori binari $p,q$ di cui dobbiamo calcolare la similarity.

Dobbiamo calcolare il valore di:
- $M_{01}$ ovvero il numero di volte in cui $p=0$ e $q=1$
- $M_{10}$
- $M_{00}$
- $M_{11}$
Possiamo calcolare la similarity con il **Simple Matching Coefficient (SMC)** come segue:
$$
\text{SMC}=\frac{M_{11}+M_{00}}{M_{01}+M_{10}+M_{00}+M_{11}}
$$

Oppure possiamo usare la distanza di **Jaccard**:
$$
\text{J}=\frac{M_{11}}{M_{01}+M_{10}+M_{11}}
$$
Questo procedimento è applicabile anche con vettori di caratteri e quindi stringhe.

Un'altra tecnica per calcolare la similarity va a calcolare l'angolo fra  due vettori e poi ne se considera il **coseno**:
$$
\cos(d_{1},d_{2})=\frac{d_{1} \cdot d_{2}}{\lvert d_{1} \rvert  \lvert d_{2} \rvert }
$$
```ad-note
title: Calcolo vettoriale
Il prodotto scalare di due vettori è la somma dei prodotti degli elementi componente per componente.
La norma è la somma dei quadrati delle componenti, di cui poi si prende la radice quadrata.

```

# Data Mining: Correlazione

E' una relazione lineare fra due dati: si può misurare con variabili binarie o continue.
L'utilità della correlazione è essere a conoscenza della relazione fra attributi, magari in modo da eliminarne alcuni (feature selection).

La **correlazione di Pearson** è quella che useremo nel corso:
$$
\text{corr}(x,y)=\frac{\text{covariance}(x,y)}{\sigma_{x} \cdot \sigma_{y}}
$$

![[Pasted image 20231027091033.png]]

# Algoritmi

[[Regole di associazione]]
[[Classificazione]]
[[Clustering]]