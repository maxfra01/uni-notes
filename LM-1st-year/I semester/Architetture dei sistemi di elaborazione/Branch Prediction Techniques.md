# Branch Prediction

I branch possono impattare negativamente sul [[Pipelining]], perciò occorre introdurre delle tecniche per capire il **comportamento dei branch**.

Due possibilità:
- Tecniche **statiche**: è il compilatore stesso, oppure il programmatore a fare una analisi del codice
- Tecniche **dinamiche**: è l'hardware stesso a decidere in questo contesto

Le tecniche statiche sono molto utili in combinazione ai **delayed branch** e il **rescheduling**.

### Static branch prediction

Differenti modi di trattare i salti:
- Considerare tutti i salti come **taken**
- Predire il salto in base alla direzione (all'indietro o avanti): spesso i salti all'indietro sono 'taken' in quanto vengono usati per i loop construct; i salti in avanti spesso sono 'untaken'
- Predire in base al codice, grazie a una raccolta di statistiche 

![[Pasted image 20231031085219.png]]

# Dynamic branch prediction

Le tecniche dinamiche sono basate sull'hardware: cercano di memorizzare l'indirizzo di destinazione dei salti e di fornire una storia del nostro programma.

Alcune fra le tecniche di predizione sono:
- Branch history table
- Two-level prediction schemes
- Branch target buffer
- Altri...

Il funzionamento di queste tecniche si basano sui concetti di località già descritti in [[Memorie Cache]].
### Branch history table

La **Branch History table BHT** è una piccola tabella che:
- Ha come **indici** la parte più bassa dell'indirizzo dell'istruzione di salto
- Contiene per ogni entry uno o più bit che registrano se il salto precedente a quell'indirizzo è stato eseguito o meno.

Ogni volta che un salto entra in fase di **decode**: si prende il target address e lo si usi come indici per vedere il contenuto della BHT. Si legge il contenuto e di aggiorna o meno il Program Counter in base alla predizione.

![[Pasted image 20231031090604.png]]

L'**efficacia** di questo metodo dipende dalla precisione della predizione ed associata al MIPS, la tecnica si rileva inefficacie.

### Two bit prediction scheme

Questo metodo ha la capacità di predire il salto in maniera più efficacie. Si basa su una macchina a stati FSM: per ogni salto si riservano due bit per conoscere la storia, e se si sbaglia due volte di fila la predizione viene cambiata.

![[Pasted image 20231031092300.png]]

La generalizzazione di questo metodo è detto **n-bit prediction scheme**.
Si basa su un contatore a n bit, dove ogni volta che il salto è preso allora incremento il counter, mentre se non prendo il salto allora decremento: quando il contatore raggiunge più della metà del suo valore massimo, allora la predizione del salto è positiva.

### Correlating predictors

Questo tipo di approccio è basato sullo specificare le dipendenze tra i risultati dalle branches.

```C
if (aa==2){
	aa=0;
}
if (bb=2){
	bb=0;
}
if (aa != bb){...}
```

Nel caso sopra il risultato dell'ultimo branch è fortemente dipendente dagli altri due.

### (m, n) predictors

Si utilizzano le ultime $m$ branches per decidere fra $2^m$ predittori, ciascuno dei quali è formato da $n$ bit.
Per l'hardware è necessario:
- la storia dei più recenti m branches è registrato in uno shift register ad m‑bit, dove ciascun bit registra se un salto è stato preso o meno.
- Il branch prediction buffer è indicizzato utilizzando una concatenazione di bit low order del branch address con m low order history bits. La storia delle branch ci fa vedere a quale BHT accedere.
Nel caso di un (2,2):

### Branch target buffer

Peer ridurre il numero di effetti sul controllo delle dipendenze richieste da sapere appena possibile, come l’informazione che il branch sia preso o meno e il nuovo valore del program counter (se il branch è assunto di essere prese), vengono risolti attraverso l’introduzione del branch‑target buffer (o cache).

Ciascuna entry del branch‑target buffer contiene l’**indirizzo del branch** considerato e il **target** value da caricare nel program counter.
Utilizzando un branch‑target buffer, il program counter è caricato con il nuovo valore alla fine dello stage di fetch, prima ancora che la branch instruction sia decodificata.

![[Pasted image 20231105161331.png]]

E' un meccanismo simile alla cache: si guarda se l'indirizzo contenuto nella istruzione da fetchare, se c'è allora ho già il target, ovvero l'indirizzo a a cui saltare.

![[Pasted image 20231105161624.png]]
