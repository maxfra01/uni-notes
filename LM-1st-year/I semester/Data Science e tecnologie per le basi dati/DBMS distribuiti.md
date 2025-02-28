# Architetture distribuite

I dati e i calcoli sono distribuiti su **diverse macchine**, con diversi livelli di complessità a seconda dell'indipendenza dei nodi.
I vantaggi a cui portano queste soluzioni sono:
- Carico di lavoro distribuito, dunque maggiori performance
- Aumento della disponibilità dei dati stessi
- Forte robustezza

Diversi tipi di architetture distribuite.
- Un primo modello di architettura distribuito è il **client-server**: è il modello più diffuso. 
- Un secondo modello è il **DMBS distribuito**: ogni **nodo è un server, con la propria istanza del DBMS** su ognuno dei server: di base essi sono **autonomi**, ma in caso di necessità possono **collaborare** per garantire le proprietà ACID.
- Terzo modello: una **replica** è una copia di dati memorizzati in un certo nodo
- Quarto modello è quello **parallelizzato**.
- **Data warehouses**

Le 2 principali proprietà rilevanti sono:
- **Portabilità**: la capacità di spostare programmi da un certo sistema a un altro e garantirne il funzionamento
- **Interoperabilità**: di fatto è la capacità dei DBMS di cooperare per svolgere un dato compito, necessitiamo quindi di protocolli come ODBC e X-Open-DTP

# Sistemi DBMS distribuiti

In questo sistema una transazione accede a più DBMS. Di fatto però ogni server è **autonomo** e sono in grado di collaborare se necessario.

Vantaggi di questo approccio:

| Vantaggio          | Nota                                                                                             |
| ------------------ | ------------------------------------------------------------------------------------------------ |
| **Localizzazione** | Posso localizzare i dati dove ne ho bisogno, ad esempio vicino a una sede centrale di un'azienda |
| **Disponibilità**  | Da questo consegue che ho maggior affidabilità e resilienza                                      |
| **Scalabilità**    | Questo approccio è modulare e flessibile                                                                                                 |

Si basa sulla **frammentazione dei dati**.
### Frammentazione dati

Data una relazione/tabella R, definiamo un **frammento** come sottoinsieme della tabella in termini di tuple, colonne o misto.
Si parlerà di frammentazione **orizzontale, verticale o mista**

Nel caso di frammentazione **orizzontale** mantengo lo stesso schema di partenza, andando ad usare un operatore di selezione con predicato.
Tipicamente i frammenti sono disgiunti.
Per ricostruire basta fare l'unione dei frammenti.

Per frammentazione **verticale** si va a modificare lo schema prendendone un sottoinsieme che deve includere la **chiave primaria**.
I frammenti si vanno a sovrapporre sulla chiave primaria.
Per ricostruire bisogna fare un join.

Proprietà della frammentazione:
- **Completezza**
- **Correttezza**

### Allocazione dei frammenti

Lo schema di allocazione descrive come i frammenti sono memorizzati sui diversi nodi. Possono essere memorizzati con ridondanza o no:

| Con ridondanza                       | Senza Ridondanza |
| ------------------------------------ | ---------------- |
| ![[Pasted image 20231207125649.png]] | ![[Pasted image 20231207125703.png]]                 |

### Livelli di trasparenza

Descrivono la conoscenza sulla distribuzione dei dati. Sono 3 livelli: **frammentazione, allocazione e linguaggio**.

Nel primo caso la distribuzione dei dati **non è visibile alle applicazioni**, quindi l'utente accederà alla completa tabella come fosse su un unico nodo. Query esempio:

```sql
SELECT Sname
FROM S
WHERE S#=:CODE
```

Le applicazioni, nel secondo caso, sono a **conoscenza dei frammenti** ma **non** delle loro **posizioni**: occorre quindi numerare tutti i frammenti e scrivere quindi nel seguente modo:

![[Pasted image 20231213085537.png]]

Nel terzo caso (linguaggio) le applicazioni devono accedere specificatamente ai server nel seguente modo:
Il DMBS trasforma le query in questo formato quando vengono eseguite.

![[Pasted image 20231213085940.png]]

### Classificazione delle transazioni

#recuperare 

# 2 phase commit protocol

Coordinazione della conclusione di una transazione su un DBMS distribuito.
E' presente un **transaction manager** (TM) che rappresenta il coordinatore, poi sono presenti i **resource managers** (RM) che prendono parte alla transazione.

TM e RM hanno i **log separati** anche se sono sullo stesso nodo. Distinguiamo 3 campi per i log di un TM: **Prepare** dove si identificano tutti i nodi partecipanti alla transazione, il **global commit/abort** e il **Complete**.

Per i RM i log hanno un campo specifico detto **Ready** quando il singolo nodo è pronto a fare commit (la decisione non può essere cambiata, anche in caso di guasto): in sostanza da quando scrive READY, il nodo cede il suo potere decisionale ad altri nodi e al TM.

Partiamo con uno scenario d'esempio:

![[Pasted image 20231213093842.png]]

Qui sopra il TM scrive il log PREPARE, poi manda il prepare msg e setta un timeout per la risposta di RM. Una volta che tutti gli RM hanno mandato il ready si procede con il global commit:

![[Pasted image 20231213094033.png]]

Nel caso di un singolo rifiuto allora si fa rollback.

