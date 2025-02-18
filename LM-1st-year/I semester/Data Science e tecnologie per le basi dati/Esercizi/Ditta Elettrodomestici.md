[[Data Warehouse]]
[[Data Mining]]
# Esercizio 1 di SQL esteso

![[Pasted image 20231019101639.png]]

### Richiesta A

Visualizzare per ogni categoria di articoli
- la categoria
- la quantità totale fatturata per la categoria in esame
- il fatturato totale associato alla categoria in esame
- il rank della categoria in funzione della quantità totale fatturata
- il rank della categoria in funzione del fatturato totale

```sql
SELECT Categoria,
		SUM(TotFatturato),
		SUM(NumArticoli),
		RANK() OVER (ORDER BY SUM(NumArticoli)),
		RANK() OVER (ORDER BY SUM(TotFatturato))
FROM Fatturato F, Categoria C
WHERE F.CodCatArticolo=C.CodCatArticolo
GROUP BY Categoria
```

### Richiesta B

Visualizzare per ogni provincia
- la provincia
- la regione della provincia
- il fatturato totale associato alla provincia
- il rank della provincia in funzione del fatturato totale, separato per regione

```sql
SELECT Provincia, Regione, SUM(TotFatturato),
		RANK() OVER (
						PARTITION BY Regione
						ORDER BY SUM(TotFatturato) DESC
					)
FROM CLIENTE C, FATTURATO F
WHERE C.CodCliente = F.CodCliente
GROUP BY Provincia, Regione

```

```ad-warning
title: PARTITION e GROUP BY
All'interno della clausola `PARTITION BY` è possibile inserire solamente attributi contenuti nella `GROUP BY` 

```

### Richiesta C

Visualizzare per ogni provincia e mese
- la provincia
- la regione della provincia
- il mese
- il fatturato totale associato alla provincia nel mese in esame
- il rank della provincia in funzione del fatturato totale, separato per mese

```sql
SELECT Provincia, 
		Mese, 
		Regione,
		SUM (TotFatturato),
		RANK() OVER (
						PARTITION BY Mese, 
						ORDER BY SUM(TotFatturato)
					)
FROM CLIENTE C, FATTURATO F, TEMPO T
WHERE T.CodTempo=F.CodTempo AND C.CodCliente=F.CodCliente
GROUP BY Provincia,Mese, Regione
```

### Richiesta D

Visualizzare per ogni regione e mese
- la regione
- il mese
- il fatturato totale associato alla regione nel mese in esame
- l’incasso cumulativo al trascorrere dei mesi, separato per ogni regione
- l’incasso cumulativo al trascorrere dei mesi, separato per ogni anno e regione

```sql
SELECT Regione,
		Mese,
		SUM(TotFatturato),
		SUM(SUM(TotFatturato)) OVER (
								PARTITION BY Regione
								ORDER BY Mese
								ROWS UNBOUNDED PRECEDING
								),
		SUM(SUM(TotFatturato)) OVER (
								PARTITION BY Regione, Anno
								ORDER BY Mese
								ROWS UNBOUNDED PRECEDING
								),
FROM CLIENTE C, FATTURATO F, TEMPO T
WHERE C.CodCliente=F.CodCliente AND T.CodTempo=F.CodTempo
GROUP BY Regione, Mese, Anno
```


# Esercizio Progettazione Data Warehouse per elettrodomestici

Metodologia:
1. Identificazione del **fatto di interesse**
	- Misure 
2. Identificare le **Dimensioni di analisi**
	- Definire le granularità dei fatti
	- Gerarchie: verificare relazioni 1:n
3. Schema logico relazionale
	-  Dimensioni di analisi condivise
4. Viste materializzate:
	- Si osserva il carico di lavoro: predicati di selezioni, attributi di grouping by, funzioni aggregate
	- Individuare la lista che rappresenta il denominatore comune
	- Individuare correttamente la granularità della vista.

TRACCIA SUL PORTALE.
### Schema logico

**Magazzino**(*CodM*, Magazzino, città, provincia, regione)
**Tempo**(*CodT*, Data, Mese, Anno, Trimestre, Quadrimestre, Semestre)
**Modello**(*CodM*od, Modello, Categoria)
**Uso_Magazzino**(*CodM, CodT*, Mq_Liberi, Mq_Totali)
**Polizze**(*CodM*,*CodT*,*CodMod*, Numero_prodotti, Valore_prodotti)
### Query A

Relativamente al primo trimestre dell’anno 2013, considerando solo i magazzini della città di Torino, trovare per ogni coppia (magazzino, data) il valore complessivo di prodotti presenti in tale data nel magazzino e il valore complessivo medio giornaliero di prodotti presenti nel magazzino nel corso
della settimana precedente la data in esame (data in esame inclusa).

```sql
SELECT magazzino, 
		data,
		SUM(Numero_Prodotti),
		AVG(SUM(Valore_Prodotti)) OVER 
					(
					PARTITION BY Magazzino
					ORDER BY Data
					RANGE BETWEEN '6' DAYS PRECEDIND AND CURRENT ROW
					)
FROM MAGAZZINO M, TEMPO T, POLIZZE P
WHERE <cond_join> AND Trimeste='I-2013' AND città='Torino'
GROUP BY magazzino, data

```

### Query B

Relativamente all’anno 2004, trovare per ogni coppia(città,data) la percentuale di superficie libera giornaliera nella città. Associare ad ogni coppia un attributo di rank legato alla percentuale di superficie libera giornaliera nella città (1 per la coppia con la più bassa percentuale di superficie libera giornaliera)

```sql
SELECT Città, 
		data,
		100*SUM(Mq liberi) / SUM(Mq totali),
		RANK() OVER (
					ORDER BY SUM(Mq liberi) / SUM(Mq totali)
					)
FROM Magazzino M, Tempo T, USO_MAGAZZINI U
WHERE <condjoin> AND Anno='2004'
GROUP BY  Città, Data
```

### Query D

Relativamente all’anno 2013, trovare per ogni coppia (magazzino, mese) il valore complessivo medio giornaliero di prodotti presenti.

```sql
SELECT Magazzino, Mese,
		SUM(Valore_prodotti)/COUNT DISTINCT (Data)
FROM MAGAZZINO M, TEMPO T, POLIZZA P
WHERE <condjoin> AND Anno='2013'
GROUP BY Magazzino, Mese
```

### Creazione vista

Dalle 3 query necessito di selezionare (considero l'unione delle 3 query): Anno, Regione, Trimestre, SUM(Valori-prodotti), Provincia, Semestre.
Nella viste però non vanno incluse esplicitamente ma dobbiamo rendere possibile che vengano fatte.

Per consentire le GP:
- Provincia
- Mese
- Semestre
- Trimestre
- Regione
Per consentire le selezioni:
- Semestre
- Regione
- Anno

```sql
SELECT Provincia, Mese, Regione, 
		Semetre, Trimestre, Anno
		SUM(ValoreProdotti), SUM(NumProdotti)
FROM MAGAZZINO M, TEMPO T, POLIZZA P
WHERE <cond-j>
GROUP BY Provincia, Mese, Regione, Semetre, Trimestre, Anno
```

### Valutazione cardinalità

Magazzino --> Provincia (500-->90) circa riduzione 10
Data ---> Mese (30 gg -> 1) circa riduzione 10
Modello --> Riduzione circa 100 volte
Complessivamente (moltiplico le riduzione) ho una riduzione di $10^4$ quindi va bene dato che cerchiamo una riduzione di almeno 3 ordine di grandezza.

```ad-important
title: Note sulla definizione della vista 
- Gli attributi di selezione **devono** far parte della vista, ma non applico le condizioni di select
- COUNT(DISTINCT x) può essere calcolato dinamicamente se x fa parte della GROUP BY, quindi non va memorizzato.
```

Next: [[Eccellenze Made in Italy]]