# Introduzione

L'obiettivo della classificazione è quello di **predirre la label** di un certo dato: partendo da un dataset **di training** si produce un **modello** per poi andare a predirre le etichette di dati provenienti da un **unclassified dataset**.
Per poi validare il modello ci serviamo di un **test set**.

Per **valutare un modello** occorre considerare certe metriche:

| Metrica          | Significato                                                                                 |
| ---------------- | ------------------------------------------------------------------------------------------- |
| Precisione       | Quante volta il modello effettivamente predice la corretta label                            |
| Interpretabilità | Come il modello lavora e come sono realizzate le astrazioni di ogni classe                  |
| Incrementalità   | Come il modello si comporta quando vengono aggiunti nuovi dati etichettati                  |
| Efficienza       | Si intende il tempo di costruzione del modello e il tempo necessario per fare le predizioni |
| Scalabilità      | Come si comporta il modello quando si aumentano la dimensione dei dati o degli attributi    |
| Robustezza       | Comportamento del modello in presenza di rumore o dati mancanti                                                                                            |

Le **tecniche di classificazione** sono molte, tra quelle descritte nel corso abbiamo:
- [[#Decision tree]]
- [[#Random Forest]]
- [[#Rule-base classification]]
- [[#Associative classification]]
- [[#K-Nearest-Neighbor]]
- [[#Bayesian classification]]
- [[#Support Vector Machine]]
- [[#Neural Networks]]

# Decision tree

E' una tecnica che funziona solamente per **problemi semplici**.
Una volta generato l'albero si può passare alla fase di test, dove basta fare uno scan del record e seguire l'albero per arrivare alla predizione.

![[Pasted image 20231108085205.png]]

Il principale problema degli alberi decisionali è quello della **generazione dell'albero** stesso: per farlo abbiamo diversi algoritmi tra cui quello di Hunt, CART, ID3, C4.5, C5.0, SLIQ, SPRINT.

### Algoritmo di Hunt

Consideriamo un generico nodo T, che ha entrante un dataset $D_{t}$:
- Se $D_{t}$ contiene record con la stessa classe $y_{t}$, allora il nodo $t$ è una foglia e ha una classe pari a $y_{t}$
- Se $D_{t}$ contiene più classi allora devo scegliere il miglior attributo (fra quelli del $D_{t}$) per fare lo split, creerò un nodo con quell'attributo e gli archi uscenti avranno tutti i valori nel dominio dell'attributo.
- Se $D_{t}$ è vuoto allora $t$ è una foglia e ha la classe con maggioranza

![[Pasted image 20231108091310.png]]

La questione importante è decidere il miglior algoritmo per fare lo split dei dati: la strategia è **greedy**, ad ogni passo scelgo il massimo locale, non quello globale.
La scelta dipende dal **tipo** di **attributo** (binario, discreto, continuo) e dal **numero di archi** uscenti che vogliamo considerare (multi-way split o binary split) e nel caso di binari split (ma se l'attributo ha un dominio con più di 2 valori) selezionare la **partizione ottimale**.
Per gestire gli attributi con dominio continuo posso **discretizzare** o usare una **decisione binaria**: ad esempio se considero un campo età posso discretizzare per ottenere gli archi (0,25), (26,50), (51, 75) oppure posso fare uno split binario ottenendo età<=34 o età > 34.

Come seleziona l'attributo per lo split?
Si preferisce selezionare gli attributi con una distribuzione omogenea delle classi (ovvero che ci sia più sbilanciamento), perciò è necessaria una misura di **impurità**.
L'idea generale è quella di calcolare la purezza dei dati prima dello split e poi per tutti gli split possibili: si guarda la differenza di queste due grandezze e si prende quella più conveniente.

### GINI index

Una prima misura per l'impurezza è il **GINI Index**:
$$
\text{GINI}(t) = 1 -\sum_{j} [p(j | t)]^2
$$
Dove $p(j | t)$ è la frequenza della classe $j$ nel nodo $t$.
- Valore massimo del GINI è $1-\frac{1}{n_{classi}}$ che comporta che i record siano equamente distribuiti e ciò implica massima impurità
- Nel caso minimo, dunque con minore impurità il GINI vale 0

Quando dobbiamo fare lo split di un nodo $p$ in $k$ partizioni allora la **qualità della partizione** si ha tramite:
$$
\text{GINI}_{\text{split}} = \sum_{i=1}^k \frac{n_{i}}{n} \cdot \text{GINI}_{i}
$$
Dove $n_{i}$ è il numero di record dei figli $i$, mentre $n$ è il numero di record al nodo $p$.

Per splittare multi-way quindi:
- calcoliamo il GINI di dati iniziali (prima dello split)
- Scegliamo un attributo, e calcoliamo il GINI dei due rami derivati
- Calcoliamo il GINI totale dello split sull'attributo

![[Pasted image 20231108094734.png]]

Per splittare in modo binario:

![[Pasted image 20231108094749.png]]

### Entropy (INFO)

Dato un certo nodo $t$ possiamo calcolarne l'entropia con:
$$
\text{Entropy}(t) = - \sum_{j}p(j|t) \cdot \log_{2} p(j | t)
$$
Il Gain in questo caso si calcola con:
$$
\text{GAIN} = \text{Entropy}(p) - \left( \sum_{i=1}^k  \frac{n_{i}}{n} \cdot \text{Entropy}(i)\right)
$$
Dove $p$ è il nodo parent.

### Condizioni di stop dell'algoritmo

Le due condizioni sono: fermarsi quando tutti i record in un nodo appartengono alla stessa classe, oppure posso fermarmi quando tutti i records hanno attributi simili.

Quando l'albero ha pochi nodi, il modello è troppo generico e predirrà classi sbagliate: ci troviamo in una condizione di **underfitting**.
Nel caso di troppi nodi (quindi di un albero troppo specifico) si parla di **overfitting**: perciò il modello ha imparato 'a memoria' i dati di training e quindi commetterà molti errori nei dati di test.

![[Pasted image 20231109103311.png]]

Per evitare di finire in overfitting devo bloccare la crescita dell'albero prima di arrivi a compimento (**Pre-pruning**):
- Le condizioni di terminazione sono quelle già citate sopra
- Inoltre possiamo aggiungerne di nuove, come introdurre soglie per le istanze di record in un nodo
- Si può stoppare se le classi nel nodo sono indipendenti (si effettua un test $\chi^2$)
- Si può fermare l'albero se il nodo corrente non ha un GINI migliore

Una seconda tecnica per evitare di andare in overfitting è il **post-pruning**: si lascia crescere l'albero fino alla fine e poi tramite decisioni bottom-up lo si riduce.

### Riassunto

| Misura           | Condizione                                                                     |
| ---------------- | ------------------------------------------------------------------------------ |
| Accuracy         | Per dataset semplici è comparabile con altre tecniche                          |
| Interpretabilità | Per alberi piccoli è facilmente interpretabile                                 |
| Incrementalità   | Non incrementali                                                               |
| Efficienza       | Molto veloci, sia per costruzione del modello che per tempo di classificazione |
| Scalabilità      | Scalabili sia per dimensione dei dataset che degli attributi                   |
| Robustezza       | Poco efficienti per gestire i dati mancanti                                                                               |

# Random Forest

Sono un esempio di **ensemble learning**: si mettono insieme più modelli per prendere la decisione finale.
Una **random forest è un insieme di decision tree**, si procede alla generazione di più alberi durante il training, dopodichè ogni albero classificare un item di set e dopodichè scelgo ciò che vota la maggioranza.

### Bootstrap aggregation

Dato un training set $D$ con $n$ elementi, si selezionano $B$ volte dei random samples (con ripetizioni) e poi si fa costruiscono gli alberi su quei samples.
Durante la generazione, per ogni possibile split nel processo di learning si selezionano $\sqrt{ p }$ attributi candidati (dove $p$ è il numero totale di attributi): così facendo si hanno **alberi che non sono correlati**.

| Misura           | Condizione                                                                     |
| ---------------- | ------------------------------------------------------------------------------ |
| Accuracy         | Migliori di un decision tree                                                   |
| Interpretabilità | NON interpretabile                                                             |
| Incrementalità   | Non incrementali                                                               |
| Efficienza       | Molto veloci, sia per costruzione del modello che per tempo di classificazione |
| Scalabilità      | Scalabili sia per dimensione dei dataset che degli attributi                   |
| Robustezza       | Robusti a rumore e dati mancanti                                               |

# Rule-base classification

Questa tecnica si basa su una collezione di 'if...then' ovvero tramite una serie di predicati si determina un outcome:
$$
\text{(condition)} \to \text{label}
$$
Definiamo **LHS** come al condizione e $RHS$ come la classe risultato.

Si dice che una regola $r$ può '**coprire**' un record $x$ se tutti le condizioni della regola sono rispettati, tuttavia può capitare che un record copra più di una regola o addirittura nessuna.
Le regole perciò devono essere **mutualmente esclusive** e **esaustive**: ogni record è coperto al massimo da una sola regola e tutti i record devono essere coperti (in tutte le combinazioni degli attributi).

Nel caso di un record che è coperto da più regole si può intervenire andando o a votazione o usando un set di regole ordinate.
Nel caso di un record che non triggera nessuna regola allora si procede usando una default class.

![[Pasted image 20231109114732.png]]

# Associative classification

Su slide.

# K-Nearest-Neighbor

Usiamo i k-dati più vicini a quello che devo classificare e gli assegno la classe con maggioranza.
Ho bisogno di una **metrica per misurare la distanza** fra punti.
Ogni volta che devo classificare un punto:
1. Calcolo la distanza dagli altri record di training
2. Trovo i k più vicini
3. Assegno la classe che ha la maggioranza

La distanza che devo calcolare può essere la distanza euclidea o altri tipi di distanze, inoltre la scelta di $k$ influenza la classe: infatti se scelgo $k$ troppo piccolo la decisione sarà molto influenzata da **outliers**, mentre con $k$ troppo grande potrei includere record che hanno differenti classi.

![[Pasted image 20231109120348.png]]

Bisogna ricordarsi di normalizzare i valori di distanza e fare attenzione alla maledizione delle dimensionalità.

![[Pasted image 20231109120450.png]]

# Bayesian classification

Richiamo sul **teorema di Bayes**:
$$
P(C|X) = \frac{P(X|C) \cdot P(C)}{P(X)}
$$
Dato una classe $C$ e i record $x_{1}x_{2},x_{3}\dots$ allora sappiamo che $P(C)=\frac{N_{c}}{N}$ e $P(X)$ è costante. Dobbiamo cercare di calcolare $P(X|C)$ dobbiamo considerare di calcolare un momento di ordine $k$, assumendo che le variabili siano i.i.d. allora diventa:
$$
P(X|C) = P(x_{1}|C) \cdot P(x_{2}|C) \dots
$$

![[Pasted image 20231109121633.png]]

# Support Vector Machine

L'idea è quella di creare un **decision boundary** che **massimizzi il margine** fra i dati di differenti classi.

![[Pasted image 20231109122443.png]]

La divisione può anche essere **non lineare**.

![[Pasted image 20231109122532.png]]

# Neural Networks

Sono strutture ispirate al cervello umano: i **neuroni** sono i centri di calcolo, mentre le sinapsi sono viste come il collegamento dei neuroni.
Ne esistono diversi modelli: FFNN, CNN, RNN.

### Feed Forward Neural Networks (FFNN)

Hanno una struttura composta da un **input layer**, che si occupa di ricevere i dati
, un **hidden layer** che effettua il processamento dei dati e l'**output layer**che restituisce il risultato.

![[Pasted image 20231110142635.png]]

In particolare ogni neurone ha la seguente struttura, volta ad effettuare una sommatoria dei valori in input per i rispettivi pesi:

![[Pasted image 20231110142716.png]]

Alla fine della sommatoria viene sempre applicata una **funzione di attivazione** che induce **non linearità** all'interno del modello.
Ne esistono di diversi tipi fra cui:
- **Sigmoid**, usata per mappare l'output su valori compresi fra 0 e 1
- **Binary step**, dove se in input c'è un numero negativo allora in output avrò 0, mentre se in input ho un valore positivo allora avrò 1 come output
- **ReLU** (Rectified Linear Unit): se l'input è negativo avrò 0 in output, altrimenti un valore proporzionale
- **Softmax** è una funzione usata solo per i neuroni dello strato di output e considera tutti i neuroni del livello.

Per costruire una FFNN sono necessari i seguenti passi:
1. **Assegnare pesi,** offset e activation function in maniera **casuale** all'inizio
2. Processare un solo record alla volta e **analizzare il risultato calcolandone l'errore**
3. **Propagare all'indietro l'errore** per andare a correggere i pesi, gli offset e la funzione di attivazione
Il processo termina quando l'accuratezza o l'errore rispettano un certo **threshold**.

![[Pasted image 20231110143829.png]]

### Convolutional Neural Networks (CNN)

Introduce dei convolutional layer che consentono di modellare i dati di input mediante feature extraction, l’output viene poi dato in pasto alla FFNN.
I vari livelli hanno la capacità di estrarre features con caratteristiche diverse:
- I primi livelli estraggono feature molto specifiche del dato
- I livelli successivi estraggono feature che tendono a sintetizzare il dato

I dati che passano attraverso una CNN si basano sul concetto di **tensore** che rappresenta un vettore multidimensionale: **rank** rappresenta il numero di dimensioni, mentre **shape** il numero di elementi per ogni dimensioni (altezza, larghezza):

![[Pasted image 20231110144533.png]]

Un’immagine può essere rappresentata come un tensore con tre matrici per i colori RGB e una per la scala di grigi.

L’elaborazione dei singoli livelli convoluzionali segue tre fasi:
- **Convoluzione**: ottiene in input i dati di input iniziali oppure il tensore prodotto dal livello precedente e restituisce un tensore con le features estratte. Questa operazione viene fatta mediante n filtri scorrevoli di pesi e produce un pixel (nel caso delle immagini) ottenuto elaborando il pixel precedentemente in quella posizione correlandolo anche ai pixel adiacenti. ![[Pasted image 20231110144843.png]]
- **Attivazione**: viene applicata una funzione di attivazione al tensore in input, come avviene nelle FFNN. Tipicamente viene usata ReLU che consente un training più veloce, non satura (ciò consente la non propagazione degli errori) e permette un calcolo delle derivate più veloce per la propagazione all’indietro.
- **Pooling**: effettua un downsampling del tensore mediante un filtro scorrevole che rimpiazza i valori del tensore con una sintesi statistica degli output vicini. Viene solitamente utilizzato max-pool che considera il valore massimo.![[Pasted image 20231110145032.png]]

Durante la fase di training ogni filtro impara a riconoscere specifici **pattern** nel tensore di input:
- Nei layer superficiali vengono riconosciuti la struttura e i bordi dell’immagine
- Nei layer più profondi vengono caratterizzati i singoli oggetti e le diverse parti dell’oggetto.
 
In pratica la **CNN effettua una segmentazione semantica
dell’immagine** che rende la classificazione mediante FFNN più
semplice e diretta.

### Recurrent Neural Networks (RNN)

Consente l’elaborazione di dati che considerano il concetto di **sequenza**, ha la capacità di mantenere uno stato che evolve nel tempo. È utilizzato per diversi scopi:
- Traduzioni
- Predizione di serie temporali
- Trascrizione vocale
- Analisi grammaticale

La rete riceve come input un vettore $x(t)$ a lo stato allo step
precedente $s(t-1)$, il training viene fatto mediante **Backpropagation
Through Time**: dato $x(t)$ e l’output atteso $y(t)$ l’errore viene
propagato all’indietro nel tempo e i pesi vengono aggiornati in modo
tale da minimizzare l’errore ad ogni step temporale.

RNN presenta il problema del **vanishing gradient** il quale decresce velocemente facendo in modo che i pesi non vengano aggiornati propriamente, rendendo difficile utilizzare memorie a lungo termine.
La soluzione a questo problema è **LSTM** (Long Short Term Memories) che utilizza delle porte logiche che permettono all’informazione sullo stato di attraversare lunghi intervalli di tempo.

![[Pasted image 20231110145529.png]]

### Autoencoders

![[Pasted image 20231110145700.png]]

### Word embedding

Le parole vengono rappresentate da vettori numerici che catturano l’informazione semantica e fanno in modo che parole con significati simili abbiano caratteristiche simili.
La vicinanza/lontananza semantica fra due parole viene calcolata mediante le posizioni dei vettori.

![[Pasted image 20231110145809.png]]

# Model evaluation

Come valutare la scelta della mia tecnica e dei parametri che ho associato alla mia tecnica, perciò il relativo modello? L'obiettivo è quindi ottenere una percentuale stimata delle performance.

Sono necessarie diverse cose tra cui: il partizionamento dei dati in training set e test set, le metriche di valutazione (errore, accuratezza) e delle tecniche per il confronto fra modello (ROC).

![[Pasted image 20231110093531.png]]

### Partizionare i dati

Quando si partizionano i dati voglio che la distribuzione delle etichette di classi nel training set e nel test set siano simili: questo lo si ottiene con un **campionamento stratificato senza rimpiazzamento**.

- **Holdout** (partizionamento **fisso**): tecnica che prevede che l'80% dei dati vadano nel training set e il restante 20% nel test set. E' la tecnica standard per **databases molto grandi**
- **Cross validation**: prendo il dataset e lo partiziono in $k$ sottoinsiemi, ognuno dei quali è generato con la tecnica stratificata: a questo punto **$k-1$ partizioni sono di training, mentre la $k$-esima sarà la partizione di test. Dopodichè ripeto il procedimento per tutte le partizioni** (tutte meno una diventeranno training, e tutte almeno una volta sono il test set).

La cross validation è migliore, perchè inevitabilmente ogni record diventa un record di test, ma Computazionalmente è complessa e richiede molto tempo per la generazione delle partizioni.

Esiste una versione di cross validation detta **Leave one out** che prevede di usare $k=n$, ovvero genero tante partizioni quanti sono i record e **userò come test un solo record alla volta**. Va bene solo per piccoli dataset.

### Stima delle performance

Nel caso di una classificazione di tipo **binario** posso costruire la seguente **matrice di confusione** per tener traccia dei risultati forniti dal modello:

![[Pasted image 20231110095404.png]]

Da qui possiamo calcolare l'**accuratezza**:
$$
\text{accuracy} = \frac{a+d}{a+b+c+d} = \frac{TP+TN}{TP+TN+FP+FN}
$$
L'accuratezza però ha delle limitazioni (esempio su slide): va bene solamente per dataset in cui la distribuzione delle classi è **bilanciata**, e nei dataset dove le classi hanno la stessa importanza.

### Class specific measures

Definiamo il **recall**:
$$
\text{Recall(r)} = \frac{\text{Number of objects correctly assigned to class C}}{\text{Number of objects belonging to C}}
$$
e La precisione:
$$
\text{precision(p)} = \frac{\text{Number of object correctly assigned to C}}{\text{Number of objects assigned to C}}
$$
Per ottenere migliori performance occorre **massimizzare la quantità**:
$$
\text{Fmeasure(F)} = \frac{2rp}{r+p}
$$
Se invece pensiamo alla **classificazione binaria** ricadiamo nel caso della matrice di confusione e quindi **le quantità descritte dalla recall e precision si possono esprimere con $a,b,c,d$** ovvero FP, FN, TP, TN.

### ROC (Received Operating Characteristic)

E' una particolare curva che da informazioni sul trade off tra **positive hits** e **false alarms**.
Definiamo il **true positive rate** e il **false positive rate**:
$$
\text{TPR}= \frac{TP}{FN+TP} \quad \quad FPR = \frac{FP}{FP+TN}
$$
Sulla base della curva costruita con questi due assi possiamo classificare un oggetto:

![[Pasted image 20231115085023.png]]

Nel caso descritto qui sopra avremo:
- Nel punto $(0,0)$ sappiamo che sia FP che TP sono a 0, quindi per forza l'oggetto va nella classe negativa
- Nel punto $(1,1)$ l'opposto, quindi l'oggetto appartiene alla classe positiva
- Il caso ideale prevede una linea da $(0,0) \to (0,1) \to (1,1)$

Come costruire la curva ROC?
1. Consideriamo una classificazione binaria, ci serve dunque un classificatore che dia l'informazione $P(+|A)$. 
2. Ordiniamo i record in ordine decrescente basandoci sulla misura ottenuta precedentemente.
3. Si applica una soglia ad ogni $P(+|A)$
4. Per ogni soglia calcoliamo TP,TN,FP,FN e successivamente TPR e FPR che **andranno a rappresentare i punti sul grafico**:

![[Pasted image 20231115085544.png]]

Per confrontare modelli possiamo **confrontare l'area sottesa dalla curva ROC**.