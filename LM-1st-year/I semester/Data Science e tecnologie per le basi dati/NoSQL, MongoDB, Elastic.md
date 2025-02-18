# NoSQL

"Nuovo" approccio per gestire un database dove **non sono presenti join**, **non c'è uno schema logico** ed è pensato per avere potenziale **scalabilità orizzontale**.
- Al posto delle tabelle ci sono delle strutture speciali come **document-based**, **coppie chiave-valore**, **DB basati sui grafi**, e columnar storage.
- Le query saranno dunque speciali e basati sulla struttura della memorizzazione dei dati.
- Non ci sono i join
- E' adatto per approcci gerarchici (come gestire JSON)
- Esempio: **MongoDB**

### Tipi di DB NoSQL

Le principali tipologie sono 4:

![[Pasted image 20231214102750.png]]

| Key-value                     | Column-family                                                   | Graph                                            | Document                                                                |
| ----------------------------- | --------------------------------------------------------------- | ------------------------------------------------ | ----------------------------------------------------------------------- |
| Approccio più semplice        | Le colonne rispecchiano gli attributi dei DB relazionei         | Basato su archi e nodi, teoria dei grafi         | I DB memorizzano i documenti                                            |
| Performance alte              | Anche in questo caso una chiave permette di accedere a una riga | Si usano per memorizzare informazioni sulle reti | I documenti sono esaustivi, quindi gli attributi e il valore coincidono |
| Facilmente scalabile e veloce |                                                                 | Perfetti per molte applicazioni reali            | I documenti hanno natura eterogenea                                                                        |
|                               |                                                                 |                                                  |MongoDB                                                                         |
 
### CouchDB

Un esempio di NoSQL è **Cluster Of Unreliable Commodity Hardware** (COUCH). E' un DB a tipo **document oriented** su cui è possibile eseguire query in una maniera **MapReduce**.
- Offre anche **replication**, per garantire la protezione dei dati, dato che usiamo hardware di commodity (economico).

# MongoDB

Nomenclatura di MongoDB:

![[Pasted image 20231221152715.png]]

Tra tutti i field c'è sempre `_id_`: è un field riservato come chiave primaria, immutabile, auto generato.
Vediamo ora alcuni comandi da terminale: per vedere la lista la lista dei DB disponibili e ed usane uno:

```
show databases
use <database-name>
```

Per eliminare (droppare) un DB usiamo:

```
db.dropDatabase()
```

Per creare una collezione (tabella) all'interno del DB usiamo:

```
db.createCollection(<collection-name>, <options>)
```

Per eliminare una collezione basta fare:

```
show collections
db.<collection-name>.drop()
```

### Create, Read, Update, Delete

![[Pasted image 20231221153446.png]]

![[Pasted image 20231221153503.png]]

![[Pasted image 20231221153546.png]]

### Query

Per fare una select dobbiamo usare l'operatore di FIND:

```
db.tabella.find({condizioni}, {campi-di-interesse})
```

Nelle condizioni è possibile usare operatori speciali per il confronto:

![[Pasted image 20231221153845.png]]

Ad esempio scrivo `{age: {$gt 25}}`.

# ElasticSearch

E' un motore di ricerca e analisi real-time: è scalabile orizzontalmente ed efficiente. La nomenclatura è la seguente:

![[Pasted image 20231220084911.png]]

### Query

Vediamo un esempio di query: cerchiamo tutti i documents all'interno dell'index 'departments' che contengono il nome John:

```
POST departments/_search
{
	"query": {
		"match" : "John"	
	}
}
```

- Per indicare condizioni booleane usiamo `should` per l'OR, mentre `must` serve per l'AND. `must_not` corrisponde al NOT.

![[Pasted image 20231220085349.png]]

Nei casi esaminati sopra abbiamo visto delle query **esatte**, dove per ottenere `_score` pari a 1 dev'esserci esattamente la parola ricercata

### Insert

```
POST /index_name/<id>
{
	JSON document
}
```

### Update

In Elastic i documenti sono immutabili, per fare l'update dobbiamo trovare il vecchio documento, modificarlo, cancellare il vecchio documento e poi re indicizzarlo.

```
PUT index_name/unique_id/_update
{
	"color": "red"
}
```

### Delete

I documenti non sono rimossi all'istante, ma in un momento successivo:

```
DELETE index_name/unique_id
```

### Result scoring

Per ogni documento viene calcolato il valore `_score`: è un indicatore di rilevanza alla soddisfazione di una query. Di default ES mostra i 10 risultati più rilevanti, ordinati per rilevanza.

Per il calcolo dello score si usa un algoritmo di TF/IDF che analizza la frequenza di ogni parola nel testo, ma anche la sua importanza. Dopodichè rappresento sia la query che il documento sotto forma di vettori, poi si valuta la somiglianza fra i vettori (Vector Space Model).

TF/IDM prende in esame:
- La frequenza del termine
- L'inverso della frequenza nell'intero documento
- La norma sulla lunghezza del field
$$
\text{Tf(t in d)} = \sqrt{ \text{frequency} }
$$
$$
\text{Idf}(t) = 1 + \log\left( \frac{\text{numDocs}}{(\text{docFreq}+1)} \right)
$$
$$
\text{norm(d)} = \frac{1}{\sqrt{ \text{numTerms} }}
$$
Ovviamente il TF IDF non viene calcolatore runtime, ma il calcolo avviene quando il documento è indicizzato per la prima volta.

