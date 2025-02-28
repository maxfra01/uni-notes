# Data Mining: Association Rules

L'estrazione di informazione tramite regole di associazione è una tecnica esplorativa che si può applicare a qualunque tipo di dato.
In questo contesto ogni riga di un record è detta **transaction**: ogni transazione coincide con un **itemset non ordinato**.
- Considerando un sotto insieme di un itemset, detto k-itemset (contiene k elementi) chiamo **support count** il numero di occorrenze di un certo k-itemset all'interno dei miei dati.
- Il **Support** è invece la frazione di record che contengono un certo k-itemset
- Un **Frequent itemset** è tale se il suo supporto è superiore a una certa soglia

Data la regola di associazione $A \implies B$ allora definiamo il **supporto** come$$
\frac{\#\{A,B\}}{\lvert T \rvert}
$$dove $T$ è la cardinalità del database transazionale.
La **Confidenza** è la misura della 'forza' di associazione:
$$
\frac{\text{sup(A,B)}}{\text{sup(A)}}
$$
Lavorando quindi su un dataset transazionale ci concentriamo sul trovare le regole di associazione che superino la soglia minima di supporto e confidenza.
Inoltre dobbiamo classificare i risultati: sono completi se tutte le regole soddisfano le condizioni, sono corretti se le solamente le regole superano entrambi le condizioni.

### Algoritmi

Per estrarre tutte le regole di associazione creo tutte le possibili permutazioni (e quindi tutte le possibili association rules).
Computazionalmente, questo approccio **brute-force** è irrealizzabile:
1. Procedo generando gli itemset più frequenti (che superano la soglia di supporto)
2. Dopodichè genero le regole (permutazione degli items)

Considerando un itemset di 5 elementi allora potrò avere $2^5=32$ subset.
Posso rappresentare queste permutazioni su un reticolo (*lattice*).
Una volta generato, per ogni itemset **candidato** dovrò vedere se il supporto è sufficientemente grande.
La complessità di una tecnica del genere è:
$$
\text{Complexity} \sim O(|T| \cdot 2^d \cdot w)
$$
dove $T$ è la cardinalità delle transazioni, $d$ il numero di item, $w$ è la lunghezza delle transazioni.
Dobbiamo quindi diminuire la complessità.

### Apriori Algorithm

Si basa sul seguente principio: "Se un itemset è frequente, allora significa che tutti i suoi sottoinsiemi devono essere frequenti".
Dunque, dati due itemset A e B tale che B includa A, allora significa che il supporto di A è maggiore:
$$
\text{if} \, A \subseteq B \implies \text{sup(A)} \geq \text{sup(B)}
$$

![[Pasted image 20231027095243.png]]

Dati ad esempio gli itemset che superano il supporto minimo {abc, ABC, ABD, ACD, ACE, BCD}
Comincio facendo il **Join** tra due item che **condividono lo stesso prefisso**: ottengo così ABCD e ACDE.
Prendo ABCD e gli applico il principio a priori, se lo soddisfa (e in questo caso si) è un possibile candidato.
Per ACDE applico il principio ma mancano alcuni sottoinsiemi perciò lo scarto.

Ovviamente alla prima iterazione nessun itemset ha prefissi comuni, quindi dobbiamo farne il **prodotto cartesiano**.

Il passo più critico dell'algoritmo è la generazione degli Item-set di dimensione 2, in quanto dobbiamo poi generare tutti i subset.
Ogni controllo di frequenza richiede inoltre anche una scansione dell'intera base dati.

Un miglioramento dell'algoritmo è il **Dynamic Itemset Counting \[Motw98\]**.

### FP-growth Algorithm \[Han00\]

L'idea di questo algoritmo è **scansionare la base dati solo 2 volte**: una per analizzare i supporti, e la seconda volta per costruire un albero.
L'albero prefisso sarà memorizzato in memoria e si lavorerà su di esso con un algoritmo ricorsivo.

Per costruire l'**FP-tree**:
1. Una prima scansiona individua la cardinalità di ogni elemento (supporto) presenti nella base dati
2. Dopodichè si costruisce la **Header Table** ordinando in modo decrescente gli elementi in base al supporto (solo gli elementi che superano la soglia di supporto sono presenti)
3. Si crea l'albero con il seguente metodo: **per ogni transazione $t$**:
	- Ordino gli elementi di $t$ nell'ordine decrescente (coerente con la header table)
	- Inserisco $t$ nell'albero usando le regole degli alberi prefissi: se il percorso esiste già aumento il supporto, altrimenti creo un nuovo percorso

![[Pasted image 20231103085335.png]]

Inseriamo i link che permettono di trovare tutti i nodi contenenti l'elemento considerato.
Una volta che ho generato l'albero procedo nel seguente modo: facciamo uno scan della header table partenza da quello con supporto minore.
1. Per ogni item $i$ estraggo gli itemsets frequenti contenenti $i$ e gli item prima di $i$ nella header table
2. Costruisco la **Conditional Pattern Base** per l'elemento $i$ ($i$-CPB)
3. Invoco ricorsivamente FP-growth su $i$-CPB

Consideriamo di star analizzando l'elemento D:

![[Pasted image 20231103090115.png]]

Prendiamo a questo punto i prefissi di D: 
- (B,A,C) con supporto 1 (quello di D)
- (B,A) con supporto 1
- (B,C) con supporto 1
- (A,C) con supporto 1
- (A) con supporto 1

Reinvocando FP-growth dobbiamo ricreare la header table e rifare la stessa cosa:

![[Pasted image 20231103090417.png]]

Ora l'item con cardinalità minore è C, quindi andiamo a prendere i suoi prefissi:

![[Pasted image 20231103090505.png]]

Se ora rifacciamo la stessa cosa con B allora otterremo il DCB-CPB che ha solamente A con supporto 1 che è sotto la soglia di supporto.
Scarterò dunque questo ramo di ricorsione e riparto dalla DC-CPB: ricorro su A cercando di costruire DCA ma è vuota pure questa: backtrack su DB... e così via.

A fine algoritmo otterrò un risultato in questa forma:

![[Pasted image 20231103091141.png]]

### Altri approcci

Si può passare da una rappresentazione dei dati orizzontale a quella verticale, dove ogni item è una colonna, e per ogni riga ci sarà il Transactional ID (**TID**) se quell'item compare in quella transazione.

A volte è necessario rappresentare in modo compatto gli itemset **rappresentando solamente quello di lunghezza massima**: "Un itemset di lunghezza massima è frequente solamente se un suo superset (aggiungo un elemento qualsiasi) non lo è".

Un altro concetto è quello di **closed itemset**, ovvero quello i cui superset hanno supporto diverso dall'itemset di partenza. I massimale sono un sottoinsieme proprio di closed.

### Altri parametri

Per quanto riguarda la **scelta della soglia**:
- Se la soglia è troppo **alta** si vanno a perdere delle transazioni **rare ma interessanti** come la vendita di gioielli costosi ad esempio
- Se la soglia è troppo **bassa** allora il numero di itemset frequenti sarà elevato e il costo computazionale molto alto

Definiamo la **correlazione** fra due elementi A e B e la regola $r: A \implies B$
$$
\text{Correlation} = \frac{P(A,B)}{P(A)P(B)} = \frac{\text{conf(r)}}{\text{sup(B)}}
$$
Indipendenza statistica: correlazione pari a 1
Positive correlation: > 1
Negative: < 1
Nel caso di correlazione negativa è più probabile che, dato A, non ci sia B.
Nel caso di correlazione positiva allora è probabile che dato A ci sia B.

Per affinare i modelli è possibile introdurre un parametro di **peso** che va a impattare sui risultati e sul calcolo degli aggregati.