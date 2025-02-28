![[Viste-Materializzate-e-trigger_Testo.pdf]]
### Schema logico

INCASSO(IdSede, IdServizio, IdCategoriaAzienda, IdTempo, \#Consulenze, Incasso)
SERVIZIO(IdServizio, Servizio, TipologiaServizio)
TEMPO(IdTempo, Data, Mese, Bimestre, Trimestre, Quadrimestre, Semestre, Anno)
SEDE-CONSULENTI(IdSede, Sede, Città, Regione, AreaGeografica, \#Consulenti)
AZIENDA(IdCategoriaAzienda, CategoriaAzienda, SettoreCommerciale, Nazionalità)

### Definizione della vista materializza (Punto 1)

- Per ogni coppia (tipologia dei servizi, semestre), visualizzare l’incasso totale e il numero totale di consulenze, effettuate dai consulenti delle sedi site in Lombardia.
- Considerando solo le aziende italiane e tedesche, selezionare per ogni coppia (Regione delle sedi dei consulenti, Servizio), l’incasso totale e il numero di consulenze effettuate, separatamente per ogni anno.
- Considerando solo gli incassi del 2017, 2018, e 2019, per ogni coppia (Tipologia dei servizi, nazionalità dell'azienda), visualizzare l’incasso semestrale e l’incasso semestrale medio per consulenza

Dalla richiesta 1 notiamo che servono:
- GB tipologiaServizi, Semetre
- Selezione di Regione
- Aggregati richiesti: SUM(Incassi), SUM(\#Consulenze)

Dalla richiesta 2:
- GB regione, Anno, servizio
- SEL nazionalità
- stesse somme di 1

Dalla richiesta 3:
GB servizio, nazionalità
SEL anno

Richiesta 3:

```sql
SELECT TipologiaServizio, Nazionalità, Semestre, SUM(Incasso),
		SUM(Incasso)/SUM(#Consulenze)
FROM SERVIZIO S, AZIENDA A, INCASSO I, TEMPO T
WHERE <cond-join> AND (Anno = 2017 OR Anno=2018 OR Anno= 2019)
GROUP BY TipologiaServizio, Nazionalità, Semetre
```

Il **BLOCCO A** risulta essere:

```sql
CREATE MATERIALIZED VIEW ViewIncassi
BUILD IMMEDIATE
REFRESH FAST ON COMMIT AS
(
SELECT TipologiaServizio, Servizio, Regione, Nazionalità, Anno, Semetre,
	SUM(#Incassi) AS SommaIncassi, SUM(#Consulenze) AS SommaConsulenze
FROM SERVIZIO S, TEMPO T, AZIENDA A, INCASSI I
WHERE <Condjoin>
GROUP BY TipologiaServizio, Servizio, Regione, Nazionalità, Anno, Semetre
)
```

### Punto 2

Identificatore minimale per la vista materializzata:
**Semestre, Nazionalità, Regione, Servizio**
Questo perchè l'anno non è necessario (Semestre -> Anno) e stessa cosa per la tipologia del servizio.

### Punto 3

```sql
INSERT INTO (schema vista materializzata)
 (SELECT BLOCCO A)
```

### Punto 4

Richiamo trigger:

```sql
TRIGGER AFTER (opera dopo l evento innescante)
ROW (eseguito 1 volta per ogni riga modificata dall eveneto innescante)
```

```sql
CREATE OR REPLACE TRIGGER Refresh_VM_ViewIncassi
AFTER INSERT ON Incassi
FOR EACH ROW
DECLARE
BEGIN
--- Inizio corpo del trigger
--- Lettura dei valori degli attributi
SELECT Servizio, TipologiaServizio

```