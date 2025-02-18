[[Progettazione fisica]]

# Esercizio 2

![[Pasted image 20231130101910.png]]

Consideriamo la query:

```sql
SELECT Matricola, Nome, AVG(voto)
FROM ESAMI E, STUDENTI S
WHERE <join> AND Voto >=26
GROUP BY Matricola, Nome
Having AVG(voto) >=26
ORDER BY Matricola
```

### Albero base senza strutture

Costruiamo il query tree: come foglie avremo le tabelle, e parallelamente alla lettura vengono eseguiti i predicati di selezione.

![[Pasted image 20231130103006.png]]

Calcoliamo la cardinalità dei diversi blocchi:
- Esami: 3x10^5
- Studenti: 10^4
- La selettività sul voto è (distribuzione uniforme) 13/30 ovvero circa un terzo: abbiamo quindi 10^5 tuple nella selezione
- Per il Join a ogni record di esami è associato un solo studente, quindi il numero di record sarà pari a 10^5 dopo il join
- Per la group by (non ho informazioni sulle matricole), quindi supponendo che si sia superato almeno un esame per studenti, la cardinalità di quella group by è pari a 10^4, ovvero il numero degli studenti
- La selezione avviene con selettività un cinquantesimo, quindi avrò cardinalità è 1/50 per 10^4 = 2x10^2
- Le altre due hanno la stessa cardinalità: 2x10^2

Ora possiamo supporre SENZA USO DI STRUTTURE ACCESSORIE il piano di esecuzione:
- per leggere le tabelle usiamo **full table scan** (non c'è l'indice), ma per ESAMI troveremo anche il dettaglio **+filter**
- Il Join va fatto con il metodo **HASH JOIN** (perchè le tabelle hanno grande dimensione)
- La GB è fatta sull'attributo di join: in questo caso usiamo la GROUP BY **NO HASH** (si usa per grossi volumi di dati e se l'operatore precedente ha definito un hash)
- HAVING corrisponde a un **filter**
- Il sort va fatto con il metodo **SORT**
- Clausola finale è quella di **SELECT**

### Anticipazione del GB sulla tabella studenti

**Non è possibile**, esiste un solo record per matricola: inoltre non c'è il voto.

### Anticipazione del GB sulla tabella esami

Già più sensato, abbiamo già in ESAMI tutte le informazioni necessarie:

![[Pasted image 20231130104753.png]]

La query può andare avanti in maniera lineare, il join serve solo più a recuperare il nome per il JOIN.
**E' possibile anticipare la GB sul ramo di esami, prima del join**.

### Creazione di strutture fisiche accessorie

- Per la tabella ESAMI: la tabella è grande, predicato di selezione su voto non molto selettivo, eventuale indice secondario su matricola per piano di esecuzione con anticipo GB eviterebbe l'ordinamento per il metodo GB SORT (tuttavia volume dati ampio, quindi si preferisce GB HASH)
- Per STUDENTI, la tabella è grande, non ci sono filtri di selezione quindi non conviene creare nessuna struttura fisica.

### Piano di esecuzione finale

- Per il join fra una tabella grande e una piccola di utilizza **NESTED LOOP** (outer table -> tabelle più grande)
- Il resto rimane invariato


# Esercizio 3

![[Pasted image 20231130110759.png]]

Statistiche sulle tuple:

![[Pasted image 20231130111001.png]]

La query è una query nidificata, quindi per ORACLE equivale ad ottimizzare due blocchi separati: per unirle si intende un **Query_esterna semi join Query_interna**.

```sql
SELECT  NomeSocietà, Città, COUNT(*)
FROM SOCIETÀ_SPORTIVA S, ATLETA A
WHERE S.CodS = A.CodS and A.Nazione = 'Italia'
	and A.DataNascita > 1979 and A.CodA IN
				(SELECT P.CodA
				FROM GARA G, PARTECIPAZIONE P
				WHERE G.CodG = P.CodG and G.Disciplina = 'Rana'
				and G.Data ≥ 2003 and P.Posizione ≥ 8
				GROUP BY P.CodA
				HAVING COUNT(*) ≥ 5 
				)
GROUP BY S.CodS, NomeSocietà, Città
```

### Piani di esecuzioni basi

