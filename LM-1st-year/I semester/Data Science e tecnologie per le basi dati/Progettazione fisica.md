# Physical design

Nel progettare un'applicazione percorriamo diversi step, tra cui analisi requisiti, progettazione concettuale e progettazione logica. Dobbiamo ancora definire **come i dati sono fisicamente memorizzati nel sistema**.

![[Pasted image 20231123102812.png]]

L'obiettivo è quello di massimizzare le performance della base dati, e di garantire l'indipendenza fra i dati: occorre perciò scegliere un [[DBMS]] adeguato, in quanto ogni soluzione prevede tecniche di gestione dei dati diversi.

### Input e Output

Occorre conoscere lo **schema logico** della base dati, le **features** del DBMS che ho scelto (es. Oracle) e il **carico di lavoro** (frequenza delle operazioni sul DB).

In output dobbiamo ottenere l'**organizzazione delle tabelle**, gli **indici**, e occorre impostare i vari **parametri** di configurazione (dimensione iniziale dei file, spazio libero, spazio buffer...)

### Dimensioni

Per l'organizzazione fisica dei file possiamo usare:
- file non ordinati (**heap**)
- file ordinati (**clustered**)
- funzioni di **hash**
- **clustering** sulle relazioni
Per gli **indici** possiamo usare diverse strutture (B+ tree, hash) e potranno essere clustered o unclustered.

### Workload

Per ogni query devono sapere le **tabelle a cui si accede**, gli **attributi da visualizzare**, gli attributi coinvolti in **join e selezioni**, e la selettività delle selezioni.
Dobbiamo quindi selezionare per **ogni tabella**:
- La struttura fisica
- Quali attributi vogliamo indicizzare

E' possibile cambiare lo schema logico, scegliendo se rispettare o meno la BCNF.

# Criteri generali per la progettazione fisica

Ci sono solamente dei criteri generali ed euristiche di buon senso, ma in generale è un processo **trial and error**. Gli errori possono essere corretti tramite una fase di **tuning** del database.

- La **chiave primaria va indicizzata**, in quanto è spesso usata per fare i join: la decisione sul tipo di indice va presa in base ad altri vincoli nella fase di design

- Per scegliere gli altri indici si guardano i predicati che **compaiono spesso** all'interno delle query:
  1. Si sceglie una certa query frequente
  2. Si considera il piano di esecuzione di partenza
  3. Si genera l'indice frequente e si genera il nuovo piano di esecuzione
  4. A seconda se miglioro o no scelgo se tenere l'indice 

- **NON indicizzare tabelle di piccole dimensioni**, perchè caricarla in memoria richiede poche operazioni I/O 

- **NON indicizzare attributi con bassa selettività** (ovvero con una cardinalità del dominio piccola): ad esempio per la scelta del genere (M o F) non ha senso indicizzare

- Nel caso di un **predicato semplici di uguaglianza** si preferisce indicizzare l'attributo con un **Hash**

- Nel caso di un **predicato semplici di range** si preferisce usare un **B+ tree**.

- Nel caso di **predicati multipli semplici** posso valutare l'utilizzo di indici composti, ovvero su più attributi: va valutato l'impatto sull'esecuzione delle query

- Per le **join** è utile tenere un indici sulla chiave di join delle tabella inner (nel caso di Nested loop). Nel caso del Merge scan, allora conviene avere un B+ tree delle chiavi di join

- Per la **group by** si consiglia di tenere indici (hash o B+ tree) sugli attributi di raggruppamento

- In alcuni sistema è possibile fare il **group by push down**, ovvero un anticipazione della group by rispetto al join

# Esempi di progettazione fisica

![[Pasted image 20231123114144.png]]

### Esempio 1

```sql
SELECT *
FROM EMP
WHERE Salary/12=1500
```

Si potrebbe pensare di fare un indice su `salary`, ma l'ottimizzatore non lo utilizza perchè c'è un'operazione sul campo stesso (/12).

### Esempio 2

```sql
SELECT *
FROM EMP
WHERE Salary = 18000
```

Anche in questo caso l'indice su Salary non valorizza le performance della query, perchè ad esempio 18000 è un valore molto frequente (selettività bassa).

### Esempio 3

Supponiamo che EMP abbia blocchi di 30 tuple (block factor). Ipotizzo inoltre che prima ci siano 50 dipartimenti, poi 2000 dipartimenti.

Se vogliamo accedere velocemente a Dept# in EMP devo indicizzare  la chiave Emp.Dept# ?
Nel primo caso (50 dipartimenti in tutto) in un blocco avrò circa 30 dipartimenti diversi, in questo caso non serve a nulla avere un indice.

Nel caso di 2000 dipartimenti, in un blocco avrò al massimo 30 dipartimenti diversi, quindi in questo caso conviene tenere un indice.

### Esempio 4

```sql
SELECT EName, Mgr
FROM EMP E, DEPT D
WHERE E.Dept# = D.Dept#
AND DName = ‘Toys’;
```

Posso provare ad indicizzare DName con un Hash (attributo categorico), poi devo considerare il join (immagino di fare nested join, con inner table = EMP), quindi posso mettere un hash index su EMP.Dept#

**HASH join è quasi sempre scelto**.

### Esempio 5

```sql
SELECT EName, Mgr
FROM EMP E, DEPT D
WHERE E.Dept# = D.Dept#
AND DName = ‘Toys’
AND Age=25;
```

Rispetto a prima abbiamo anche una condizione su Age, possiamo pensare di aggiungere un indice per l'età, ma dipende dalla selettività della query: in seguito sceglierò come inner table DEPT, così da ottimizzare.

### Esempio 6

```sql
SELECT EName, Mgr
FROM EMP E, DEPT D
WHERE E.Dept# = D.Dept#
AND Salary BETWEEN 10000 AND 12000
AND Hobby=’Tennis’;
```

Due condizioni operano su Employee: hobby è più selettivo di salary (in quanto range), quindi scelgo di avere un B+ tree su salary e Hash su hobby.
Per il join consideriamo EMP come outer (in quanto è quella filtrata dai predicati) e DEPT come inner.

### Esempio 7

```sql
SELECT Dept#, Count(*)
FROM EMP
WHERE Age>20
GROUP BY Dept#
```

La selezione non è per nulla selettiva, quindi non voglio assolutamente tenere un indice per l'età.

# Procedimento per gli esercizi

1. Analizzare le tabelle: procedo per prima cosa ad analizzare le **statistiche** sulle tabelle:
	 - Cardinalità della tabella
	 - min/max(un attributo) + ipotesi di distribuzione uniforme = stima sulla cardinalità di una tabella
	 - Numero di valori distinti -> stima sulla cardinalità

2. Ispezione dell'istruzione sql

3. Definizione del query tree (in algebra estesa) associato all'istruzione sql

4. Valutazione cardinalità dei risultati intermedi (nodi) e di quello finale:
	- Separare i diversi predicati di selezione

5. Definizione del **piano di esecuzione** senza strutture fisiche associate

6. Valutazione di **possibili strutture accessorie**:
	- Per ogni indice sensato vogliamo che abbia selettività, aiuto alla GB, che sia coprente, primario o secondario

```ad-note
title: Come procedere?
Ecco alcune regole euristiche per procedere:
- Una tabella è **piccola** quando la cardinalità è minore di $10^3$ tuple
- La tabella + **media/grande** se è più grande di $10^3$ tuple, posso quindi valutare di usare un indice
- La selettività è **elevata** quando la frazione di tuple estratte è minore di 1/10 del totale, posos dunque valutare un indice
- La selettività è **bassa** se le tuple che estraggo sono maggiori di 1/10 del totale, non userò indici (coerentemente con altre condizioni)

Procedimento da seguire:
- Analizzare cardinalità tabelle
- Ispezioni istruzioni sql
- Definizione del query tree
```