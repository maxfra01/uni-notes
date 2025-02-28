# Crosstalk
---
Per **Diafonia (crosstalk)** si intende il passaggio di segnale fra due canali separati (nello spazio, nella codifica, nel tempo).
Spesso avviene fra conduttori diversi (tipo capcitivo o induttivo, raremente resistivo).
E' anche possibile che si verifichino **ISI**, interferenza intersimbolica.

Nella pratica consideriamo delle piste fisicamente affiancate, che manifestano una capacità parassita:

![[Pasted image 20230503134019.png]]

Nella linea rossa (**linea aggressore**), se c'è commutazione, è possibile che si abbiano appunto dei disturbi sulla linea verde (**linea vittima**).
Il **segnale disturbante** $V_{s}$ che viene generato è del tipo:

![[Pasted image 20230503134146.png]]

E quindi dipende contemporaneamente dall'ampiezza del segnale, e anche dal tempo di salita del segnale: per limitare diafonia devo avere segnali che variano lentamente nel tempo.
Pertanto, matematicamente il disturbo dipende dalla **derivata del segnale rispetto al tempo** $\frac{ \partial V_{s} }{ \partial t }$, dalla **capacità e induttanza di accoppiamento**.

## Ridurre la diafonia

Per **Ridurre il crosstalk**:
- Rallentare i fronti dei driver della linea disturbante
- Ridurre $C_{m}$ e $L_{m}$
- Usare segnali differnziali

Per **Ridurre gli effetti** del crosstalk:
- Filtrare i segnali della linea vittima
- Tecniche di rilevazione e correzione d'errore (EDC,ECC)

Per quanto riguarda la riduzione dei **fronti di salita** dei driver procediamo diminuendo $\Delta V$ e aumentanto $\Delta t$.
Esistono standard come **LVDS** che garantiscono questa lenta salita dei driver.
Ovviamente il tutto va confrontato con le richieste di progetto.

### Correnti di ritorno

Normalmente all'interno di una scheda la corrente segue percorsi ad area minima.
E' importante che il percorso di queste correnti non intercettino il **piano di massa** (strato metallico a potenziale di riferimento sulla scheda).

![[Pasted image 20230503135012.png]]

### Accoppiamenti induttivi

A causa del fenomeno della mutua induzione, è possibile che una corrente che circola in una zona della scheda disturbi un circuito fisicamente distante:

![[Pasted image 20230503135235.png]]

Per evitare ciò occorre **evitare spire ampie e concatenate**.
E' importante anche che le diverse piste abbiano percorsi di ritorno a massa non coincidenti!

![[Pasted image 20230503135356.png]]


## Distribuzione del clock

Una prima soluzione è un collegamento multipunto, ma è caratterizzato da un elevato skew, assolutamente da non usare:

![[Pasted image 20230503135820.png]]

Un secondo modo consiste nell'usare una logica dedicata per gestire il clock:

![[Pasted image 20230503135850.png]]

Per andare nelle condizioni ottimali occorre si usare dispisitivi specifici (clock driver IC), ma anche far sì che le piste che vanno dal clock driver al componente siano della stessa lunghezza: per garantire questo si fa uso di piste a **meandro**:

![[Pasted image 20230503140215.png]]



# Distribuzione dell'alimentazione
---
Le correnti di alimentazione possono avere dei percorsi comuni all'interno di un circuito stampato.
Quando però la corrente ha delle **forti variazioni** si può verificare quello che viene detto **ground bounce**, ovvero lo spostamento del riferimento.
Quando ci sono commutazioni simultanee è possibile che si generi del rumore.

Questi due fenomeni sono legati a:
- Pendenza delle transizione $\frac{ \partial V }{ \partial t }$
- Quantità di cariche da spostare 
- Induttanza dei collegamenti a GND o $V_{AL}$

![[Pasted image 20230503141114.png]]


# Rumore di commutazione
---
Il rumore è legato principalmente alle uscite del circuito (presentano un carico capacitivo maggiore), dipendono inoltre da:
- numero di porte di uscita
- velocità dei fronti
- Capacità del carico
- Induttanza di $V_{AL}$ e GND.

Occorre dunque limitare il numero di uscite che commutano, oppure si fa uso di **condensatori di bypass**, montati il più vicino possibile alla corrente che si vuole scaricare.

![[Pasted image 20230503141525.png]]

E' possibile inserire più condensatori di bypass in parallelo.