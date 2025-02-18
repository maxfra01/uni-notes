# Terminologia
---

### Errore
Il termine in passato veniva usato per distinguere fra errori sistematici (sempre gli stessi e con stesso valore) e errori accidentali (aleatori).

Oggi il termine, anche detto **measurement error**, indica la differenza fra una misura e un valore di riferimento.
In caso di taratura, allora è la differenza fra il valore misurato dallo strumento e un campione.

### Precisione
La **measurement precision** indica la capicità di fornire misure simili in condizioni specificate, in termini di:
- condizioni di ripetibilità (stesse specifiche della misurazione, campione, operatore ecc...)
- condizioni di riproducibilità (diverse specifiche)

E' solitamente espressa in termini di varianza e deviazione tipo.

### Accuratezza
La **measurement accuracy** indica la capacità del sistema di fornire misure 'vicine' al valore effetivo del misurando.

In pratica mettendo a confronto due sistemi X e Y, allora uno è più accurato se fornisce errori minori.

### Incertezza
La **measurement uncertainty** indica l'indeterminazione nella conoscenza della misura, tenendo conto di **tutti** i contributi di incertezza, ed è espressa mediante una fascia di valori.

### Risoluzione
La **measurement resolution** è la minima variazione della grandezza di ingresso di un dispositivo di misurazione che provoca una variazione apprezzabile della sua indicazione.

Di fatto indica la capacità del dispositivo di rilevare un 'piccolo' cambiamento nella misura.

La risoluzione è un limite per l'incertezza.

### Sensibilità
La **sensitivity** è definita come il Rapporto tra la variazione dell’indicazione (uscita) di un dispositivo di misurazione e la corrispondente variazione del misurando (ingresso).

Assume valore costante per i dispositivi con caratteristica (nominalmente) lineare.
La **costante di taratura** è il reciproco della sensibilità $K_{x}=\frac{1}{S_{x}}$

### Effetto sistematico
Detto anche **systematic error**, rappresenta l'effetto che in misure ripetute rimane costante, o varia in modo prevedibile (es. errore di fuori zero).

Dopo essere stato valutato la misura può essere corretta.
La valutazione di un effetto sistematico è sempre soggetto a incertezza.

Le cause principali di effetti sistematici sono:
- Carico strumentale, ovvero il fatto che per misurare dobbiamo per forza stabilire un contatto fra lo strumento e il misurando.
- Grandezze di influenza
- Non idealità della strumentazione (errori di fuori zero, guadagno ecc...)

Se un effetto sistematico non è corretto allora diventa un contributo di incertezza.
Se lo si corregge rimane comunque un piccolo contributo di incertezza.


# Principali contributi di incertezza
---

E' importante sottolineare che è **impossibile** ottenere una misura priva di incertezza per i seguenti motivi:

- Gli strumenti impiegati non sono ideali 

- La misura ottenuta è diversa dalla misura “a vuoto” (effetto di carico) 

- Le grandezze di stato e quelle di influenza non sono perfettamente note: Le grandezze di stato (del sistema in misura) sono quelle che modificano il misurando mentre le grandezze di influenza sono quelle che modificano le caratteristiche della strumentazione

- La risoluzione con cui si osserva il segnale di misura è limitata (incertezza di quantizzazione) 

- Al segnale di misura è sovrapposto un processo casuale (rumore)  

- Il limite inferiore all’incertezza è fissato dalla cosiddetta “incertezza intrinseca del misurando”, (definitional uncertainty):  la riduzione di questo contributo di incertezza richiede di complicare la definizione del misurando e, di conseguenza, anche la sua realizzazione.

# Modello deterministico e incertezza
---

Nel modello deterministico la misura di un parametro è assegnata sotto forma di intervallo limitato (**fascia di valore**), solitamente simmetrico rispetto al valore $m_{0}$ assegnato al parametro.
La fascia di valori ha le seguenti caratteristiche:
- il misurando appartiene alla fascia
- tutti gli elementi della fascia sono validi per rappresentare il misurando.

Esprimiamo una misura come :$$
m=(m_{0} \pm  \delta m)U
$$
dove la quantità $\delta m$ rappresenta l'**incertezza assoluta**.
Si utilizzano sempre o una o due cifre significative per l'incertezza.
Inoltre occorre che mi fermi alle cifre del misurando nello stesso modo in cui mi fermo alle cifre dell'incertezza.

L’incertezza assoluta non fornisce però una sensazione immediata della qualità di una misura: prendiamo in esame queste due misure: $$
I_{1}=(13.60 \pm 0.01)m
$$ $$
I_{2}=(2320.01 \pm 0.01)m
$$
Entrambe sono caratterizzate dalla stessa incertezza, ma la seconda è molto più complicata da ottenere!

Se si vuole è possibile ricorrere all'**incertezza relativa** riferita a un valore convenzionale $m_c$ , quantità adimensionata esprimibile anche in percentuale: $$
\epsilon m=\frac{\delta m}{m_{c}}
$$
La misura (con incertezza relativa) si esprime nella forma $$
m=m_{0}\space U, \pm \epsilon m 
$$
Spesso i costruttori indicano l'incertezza relativa ridotta rispetto ad altri prodotti, ma va sempre confrontata con la portata dello strumento, altrimento si rischia di cadere in trappole :(

# Propagazione degli errori
---

Occorre distinguere i metodi per calcolare la propagazione degli errori:
- metodi **diretti**: le misure sono assegnate a partire dalla lettura di un valore su uno strumento, o da un campione.
- metodi **indiretti**: misura asseganta come risultato di un calcolo che coinvolge grandezze ottenute direttamente.

Nel primo caso (diretto) occorre **sommare** tutti i contributi dell'incertezza fra cui incertezza strumentale, di lettura, effetti del rumore ecc...

Nel caso di misure indiretti, consideriamo $N$ grandezze ottenute con metodi diretti, tutte nella forma $X_{i}=(X_{i_{0}} \pm \delta X_{i})U_{i}$
Se cerchiamo una funzione del tipo $y_{0}=f(X_{1},X_{2},\dots X_{N})$ allora devo usare una serie di Taylor troncata al primo ordine, nell'intorno del punto di lavoro:
$$
\delta y=\sum_{i=1}^N|\frac{ \partial f }{ \partial X_{i} }|_{(X_{1_{0}},\dots X_{N})}\cdot \delta X_{i} =\sum_{i=1}^N|c_{i}|\cdot\delta X_{i}
$$
dove $c_{i}$ è detto coefficienti di sensibilità della funzione rispetto alle diverse variabili.

Questo metodo permette di valutare il caso pessimo:
- si tiene conto dell'estremo della fascia di valori.
- l'operatore valore assoluto esclude la possibilità di eventuali compensazioni fra grandezze.
- Così facendo si garantisce che il risultato sia compreso nella fascia di valori.

Analisi dei casi notevoli, da usare solamente se le grandezze considerate sono **INDIPENDENTI**.

### Somma o differenza
Consideriamo la funzione $Y=ax_{1}- bx_{2}$ con $a$ e $b$ costanti e maggiore di $0$.
possiamo calcolarne le derivare:$$
|\frac{ \partial Y }{ \partial x_{1} } |=a \quad |\frac{ \partial Y }{ \partial x_{2} }|=b 
$$
Ottengo dunque che l'incertezza assoluta su $Y$ è nella forma $$
\delta Y=a \cdot\delta x_{1} +b\cdot\delta x_{2}
$$
Quindi se abbiamo una combinazione lineare basta sommare le incertezze moltiplicate per il relativo coefficiente. 

### Prodotto o rapporto
Consideriamo la funzione $Y=a\cdot x_{1}\cdot \frac{x_{2}}{b\cdot x_{3}}$ ne faccio le derivate e ottengo un risultato sull'errore relativo di:$$
\epsilon_{y}=\epsilon_{x_{1}}+\epsilon_{x_{2}}+\epsilon_{x_{3}}
$$
Nel caso dunque di prodotto o rapporto, basta sommare le incertezze relative.

### Potenza e radice
Ci si riconduce facilmente nel caso di prodotto:
$$
Y=a\cdot X^N  \quad \epsilon_{Y}=N\cdot\epsilon_{X}
$$
$$
Y=a \cdot \sqrt[N]{ X } \quad \epsilon_{Y}=\frac{1}{N}\cdot \epsilon_{X}
$$

