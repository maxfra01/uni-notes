# Modello concettuale

- Fonte dati primari: Ordini di acquisto
- Integrazione con altre fonti: 
	- certificazioni
	- dimensioni dell'ordine
	- anagrafica aziende

Per le interrogazioni si è interessati a quantità totale di vino esportata, prezzo medio per litro (che non includiamo nella WH).

| EXPORT       |
| ------------ |
| Quantità     |
| Litri totali |
| Valore totale             |

```ad-note
Per le **misure** indico i dati utili per il calcolo della media, ovvero l'importo toale e i litri totali esportati.
```

Dopodichè colleghiamo le informazioni temporali per mese, bimestre, quadrimestre e semestre, gli ultimi due convergono nell'anno.

Per quanto riguarda le certificazioni ne dobbiamo memorizzare 3: DOC, DOP, DOCG, quindi usando 3 bit possiamo mappare le 8 possibili combinazioni di certificazioni (000 significa nessuno, 111 tutte e tre).
Una soluzione alternativa consiste nell'usare un arco multiplo.

![[Pasted image 20231026120022.png]]

# Modello Logico

Si noti che le dimensioni certificazione e dimensioni sono degeneri, quindi possiamo fare una junk dimension, un push down nel fatto o una tabella apposita.
Scegliamo per la dimensione dell'ordine di dare il push down nel fatto, mentre per le certificazioni scegliamo di fare una tabella dedicata (cardinalità piccola).

TEMPO (_CodT_, Mese, 2m, 3m, 4m, 6m, Anno )
TIPO_VINO (*Cod-TV*, Tipo-Vino, DOC, DOP, DOCG)
TIPO_CONF(*Cod-TC*, Confezione)
DESTINAZIONE(*Cod-D*, Stato, Continente)
INFO_AZIENDA(*Cod-AV*, Provincia, Regione, Area-Geografica)
EXPORT(*CodT, Cod-TV, Cod-TC, Cod-D, Cod-AV, Dimensione-Ordine*, Litri-totali, Valore-totale )

# Interrogazioni specifiche

### Query A

Considerando solo i vini che hanno la certificazione “DOC” esportati in Asia, selezionare il prezzo medio al litro per ogni anno, la percentuale di litri esportati nell’anno rispetto al totale di tutti gli anni, e il totale cumulativo annuale di litri esportati. Si effettui l’analisi separatamente per ogni tipo di confezionamento.

```sql
SELECT Confezione, Anno,
		AVG(ValoreTotale/LitriTotali),
		(SUM(LitriTotali)*100)/ 
		SUM(SUM(LitriTotali)) OVER (PARTITION BY Confezione),
		SUM(SUM(LitriTotali)) OVER (PARTITION By Conferzione
								ORDER BY Anno
								ROWS UNBOUNDED PRECEDING)
		
FROM TIPO_VINO, DESTINAZIONE, EXPORT, TEMPO
WHERE <condjoin> AND DOC=1 AND Continente='Asia'
GROUP BY Confezione, Anno 
```

### Query B

Per ogni regione, selezionare il prezzo medio al litro, il numero medio di litri di vino esportati per provincia, e la percentuale di litri di vino esportati da ogni regione rispetto al totale dell’area geografica di appartenenza (Nord, Centro, Sud). Si effettui l’analisi separatamente per ogni anno.

```sql
SELECT Regione, Anno, SUM(ValoriTotali)/SUM(LitriTotali),
	SUM(LitriTotali)/ COUNT(DISTINCT Provincia),
	100*(SUM(LitriTotali)) / SUM(SUM(LitriTotali))
			 OVER (PARTITION BY AreaGeografica, Anno)
FROM TEMPO, INFO_AZIENDA, EXPORT
WHERE <condjoin>
GROUP BY Regione, Anno, AreaGeografica
```

```ad-note
- Se in PARTITION BY ho attributi non inzialmente presenti in GROUP BY, devo verificare che possano essere inseriti (dallo schema).
In caso negativo la query va riformulata.

- Per il numero medio di litri per provincia: bisogna chiedersi se nella partizione considerata ho una unica tupla per ogni provincia? In caso negativo non posso usare AVG per calcolare una media.
```

