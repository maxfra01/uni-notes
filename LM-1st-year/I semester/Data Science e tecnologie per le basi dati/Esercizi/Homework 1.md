**Homework #1**
**Massimo Francios**
**s328914**
# 1 - Schema concettuale e logico

![[Pasted image 20231119212905.png]]

MUSEO(**_NomeMuseo_**, Categoria, Città, Provincia, Regione, Servizio1, Servizio2, ... , Servizio 10)
TEMPO (**_IdTempo_**, Data, Mese, Bimestre, Trimestre, Semestre, Anno, Festivo)
VENDITA(**_NomeMuseo_**, **_IdTempo_**, **_TipoAcquisto_**, **_TipoBiglietto_**, **_FasciaOraria_**, NumBiglietti, Ricavo)

# 2 - Query SQL

```sql
SELECT TipoBiglietto, Mese,
	   SUM(Ricavo)/COUNT( DISTINCT Data),
	   SUM(SUM(Ricavo) OVER (PARTITION BY Anno
					  ORDER BY Mese
					  ROWS UNBOUNDED PRECIDING)),
	100*SUM(NumBiglietti) / SUM(SUM(NumBiglietti)
							 OVER (PARTIION BY Mese))
FROM TEMPO T, VENDITA V
WHERE V.IdTempo = T.IdTempo
GROUP BY TipoBiglietto, Mese, Anno
```

```sql
SELECT NomeMuseo, TipoBiglietto,
		SUM(Ricavo) / SUM(NumBiglietti),
		100*SUM(Ricavo) / SUM(SUM(Ricavo) OVER (
				PARTITION BY Categoria, TipoBiglietto)),
		RANK() OVER (ORDER BY SUM(NumBiglietti) DESC)

FROM VENDITA V, TEMPO T, MUSEO M
WHERE T.IdTempo=V.IdTempo AND M.NomeMuseo = V.NomeMuseo AND Anno="2021"
GROUP BY NomeMuseo, TipoBiglietto, Categoria
```

# 3 - Vista materializzata

```sql
CREATE MATERIALIZED VIEW ViewRicavi
BUILD IMMEDIATE
REFRESH FAST ON COMMIT AS
(
SELECT Mese, Semestre, Anno, TipoBiglietto, TipoAcquisto, SUM(Ricavo) AS TotaleRicavi, SUM(NumBiglietti) AS TotaleNumBiglietti
FROM VENDITA V, TEMPO T
WHERE V.IdTempo = T.IdTempo
GROUP BY Mese, Semestre, Anno, TipoBiglietto, TipoAcquisto
)
```

Identificatore minimale: **Mese, TipoBiglietto, TipoAcquisto**.
E' utile tener traccia dei log per le tabelle VENDITA e TEMPO.

```sql
CREATE MATERIALIZED VIEW LOG ON VENDITA
WITH ROWID, SEQUENCE(Ricavo, NumBiglietti, TipoBiglietto, TipoAcquisto)
INCLUDING NEW VALUES;
```

```sql
CREATE MATERIALIZED VIEW LOG ON TEMPO
WITH ROWID, SEQUENCE(Mese, Semestre, Anno)
INCLUDING NEW VALUES;
```

Le operazioni che causano un aggiornamento della vista sono UPDATE e INSERT su VENDITA e/o TEMPO.

# 4 - Trigger

Creazione della vista tramite CREATE TABLE:

```sql
CREATE TABLE ViewRicavi
(
	Mese VARCHAR(20),
	Semestre VARCHAR(20),
	Anno VARCHAR(20),
	TipoBiglietto VARCHAR(20),
	TipoAcquisto VARCHAR(20),
	TotaleRicavi INTEGER
	CHECK (TotaleRicavi IS NOT NULL and TotaleRicavi >0),
	TotaleNumBiglietti INTEGER
	CHECK (TotaleNumBiglietti IS NOT NULL and TotaleNumBiglietti >0),
	PRIMARY KEY (Mese, TipoBiglietto, TipoAcquisto)
)
```

Popolamento della vista:

```sql
INSERT INTO ViewRicavi (Mese,Semestre,Anno, TipoBiglietto, TipoAcquisto, TotaleRicavi, TotaleNumBiglietti)
(
SELECT Mese, Semestre, Anno, TipoBiglietto, TipoAcquisto, SUM(Ricavo) AS TotaleRicavi, SUM(NumBiglietti) AS TotaleNumBiglietti
FROM VENDITA V, TEMPO T
WHERE V.IdTempo = T.IdTempo
GROUP BY Mese, Semestre, Anno, TipoBiglietto, TipoAcquisto
)
```

Trigger per propagare le modifiche:

```sql
CREATE OR REPLACE TRIGGER RefreshVM_ViewRicavi
AFTER INSERT ON VENDITA
FOR EACH ROW

DECLARE
VarMese, VarSemestre, VarAnno VARCHAR(20)
N INTEGER
BEGIN

---lettura dei valori necessari
SELECT Mese, Semestre, Anno INTO VarMese, VarSemestre, VarAnno
FROM Tempo T
WHERE IdTempo = :NEW.IdTempo

---verifico esistenza tupla per inserire o aggiornare
SELECT COUNT(*) INTO N
FROM ViewRicavi
WHERE Mese = VarMese AND
	TipoAcquisto = :NEW.TipoAcquisto AND
	TipoBiglietto = :NEW.TipoBiglietto

IF (N>0) THEN
	UPDATE ViewIncassi
	SET TotaleRicavi = TotaleRicavi + :NEW.Ricavo
		TotaleNumBiglietti = TotaleNumBiglietti + :NEW.NumBiglietti
	WHERE Mese = VarMese AND
		TipoBiglietto = :NEW.TipoBiglietto 
		AND TipoAcquisto = :NEW.TipoAcquisto
ELSE
	INSERT INTO ViewRicavo(Mese, Semestre, Anno, TipoBiglietto, TipoAcquisto, TotaleRicavi, TotaleNumBiglietti)
	VALUES(VarMese, VarSemestre, VarAnno, :NEW.TipoBiglietto, :NEW.TipoAcquisto, :NEW.Ricavo, :NEW.NumBiglietti)
END IF
END
```

L'evento che scatena il trigger definito precedentemente è una INSERT sulla tabella VENDITA.