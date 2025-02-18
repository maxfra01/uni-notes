# Esercizio 1

![[Pasted image 20231126093548.png]]

L'informazione sulla data descrive il suo dominio, e con l'ipotesi di uniformità si può stimare la selettività.
Per l'ultima condizione ci viene detto (supponendo l'uniformità) la selettività della selezione.
Per la selezione del tipo 10 sappiamo esattamente quanto il predicato sia selettivo per un valore del suo dominio (istogramma).

[[Progettazione fisica]]

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

Consideriamo la query:

```sql
SELECT Data, COUNT(*)
FROM MULTE
WHERE Data >=1/10/2005 and Data <=30/11/2005
GROUP BY Data
```

Abbiamo già descritto la tabella, e la query è piuttosto semplice in questo caso. Definiamo quindi il query tree:

![[Pasted image 20231126100525.png]]

Ora definiamo il piano di esecuzione:
- Full scan della tabella tupla, applicando nel mentre anche la selezione e la group by (HASH o SORT (i dati non sono ordinati)), infine si fa il SELECT STATEMENT

Discutiamo ora l'introduzione di un indice su data:
- La selettività di data è bassa
- La group by però è fatta su Data, quindi aiuta la group by ed è **coprente** (non dovrò rileggere la tabella), sarà secondario.

Per questi due motivi è comodo creare indice sulla data con:
CREATE INDEX MyIndex ON MULTA(Data)

Preferibile inoltre un group by HASH.

Nuovo piano di esecuzione con MyIndex (coprente, ovvero permette di eseguire l'interrogazione senza accedere all'intera tabella).
- Strategia 1:
	Facciamo un **fast full index scan** on MyIndex, subito dopo faccio GROUP BY HASH (ordine perso), poi la select
- Strategia 2: fare un **Full index scan** on MyIndex, poi GB NO SORT (uso l'ordinamento), poi select

![[Pasted image 20231129084233.png]]

Il migliore sarà deciso dall'ottimizzatore in base alle stime calcolate.
### Query 2

```sql
SELECT Data, COUNT(*)
FROM MULTE
WHERE Data >=1/10/2005 and Data <=30/11/2005 AND
	TipoInfrazione = 'Tipo10'
GROUP BY Data
```

Andiamo a costruire l'albero, sapendo che il predicato di selezione sul tipo ha selettività 1/100:

![[Pasted image 20231129084849.png]]

SENZA INDICI
Comincio con una **full scan table**, poi posso fare una GB SORT, tanto ho poche tuple e quindi posso ordinarle.

CON UN INDICE
Possiamo mettere un indice su data o su Tipo Infrazione: vogliamo cercare attributo con alta selettività e che aiutino la group by.
Data non è coprente, ha bassa selettività e non aiuta GB.
Tipo infrazione ha alta selettività, non aiuta la GB, non è coprente (devo rileggere per la select su data).
Possiamo **provare a mettere un indice per Tipo Infrazione**: MyIndex

NUOVO PIANO DI ESECUZIONE

Uso un **range index scan** on MyIndex, poi table access by row ID, poi GB sort

![[Pasted image 20231129085500.png]]

Potevo fare un indice coprente composto da data e tipologia:
Devo scegliere fra <Tipo, Data> o <Data, Tipo>.
Il primo ha migliore selettività, aiuto il GB ed è coprente.
