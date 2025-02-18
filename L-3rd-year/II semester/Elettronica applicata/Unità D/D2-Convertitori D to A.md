# Parametri dei convertitori D/A
---

### Caratteristica di conversione

In questi sistema la variabile in ingresso è **discreta** e corrisponde a $M=2^N$ punti allienati ed equispaziati: $N$ rappresenta il numero di bit.
Nel caso di $M$ abbastanza grande possiamo approssima la caratteristica a una retta.

![[Pasted image 20230509171358.png]]


### Errori

Possiamo distinguere due tipologie di errori: **Errori statici** ed **Errori dinamici**
Nel primo caso possiamo avere **errori di offset** ovvero una traslazione della caratteristica di una quantità, oppure possiamo avere **errori di guadagno** che modificano la pendenza della retta.

Entrambi sono **Compensabili**! 

Questi errori sono lineari, in quanto suppongono la caratteristica lineare. Nel caso reale è possibile avere a che fare con **errori di non linearità**:

![[Pasted image 20230509171814.png]]

In questa situazione possiamo approssimare una retta (ad esempio tramite il metodo dei minimi quadrati) che corrisponde alla **migliore retta approssimante**. Inoltre definiamo una **fascia di non linearità**:

![[Pasted image 20230509171936.png]]

A questo punto è possibile **correggere la retta approssimante** andando ad agire sull'offset e sul guadagno. Idealmente voglio fare corrispondere la retta approssimante con la retta ideale.

| Errore di Offset         | Errore di guadagno |
| ------------------------ | ------------------ |
| $\epsilon_{OFF}=V_{OFF}$ | $\epsilon_{K}=\frac{\Delta K}{K}$                   |

Esistono poi gli errori di non linearità differenziale e integrale, già viste in [[Conversione Analogico-Digitale]].
Parliamo inoltre di errore di non monotonicità, se ci troviamo in una zona in cui la caratteristica vera e quella ideale non hanno la stessa monotonia.


### Tempo di assetto

Nel transitorio in uscita passa un tempo $T_{S}$ prima che l'uscita del convertitore D/A si stabilizzi.
Durante la transizione degli ingresso è possibile che si formino dei **glitch**, ovvero variazioni molto ampie e rapide del segnale.
Questi glitch **nascono dai ritardi di commutazione**, in quanto certe transizione portano il segnale in modo anomalo sullo 0 o sul fondo scala (Immaginiamo di dover passare da 0111 a 1000, potremmo avere transizioni su 1111 e 0000).

### Riassunto

![[Pasted image 20230509173054.png]]



# Struttura del convertitore
---

![[Pasted image 20230509182345.png]]

La struttura generale è quella sopra. Le grandezze di riferimento servono per dare il peso alle cifre in binario, poi il blocco finale le somma e restituisce il valore analogico.

Nel caso di conversione con rete **pesata** abbiamo il seguneete schema:

![[Pasted image 20230509182455.png]]

Nel caso di grandezze uniformi, ogni posizione ha peso pari a 1.
Dal punto di vista del circuito, nel caso uniforme abbiamo le **correnti** che identificano la scelta degli interrutori chiusi:

![[Pasted image 20230509182703.png]]

Nel caso delle grandezze pesate, le resistenze sono scalate per un fattore 2:

![[Pasted image 20230509182736.png]]

### Rete a scala

Il problema principale di una rete a grandezze pesate con resistenze multiple di 2, abbiamo come richiesta che la dinamica di $R$ sia pari alla dinamica di corrente: difficile da realizzare su integrati, perciò usiamo la **rete a scala**.

Con soli due valori di resistenza $R,2R$ riusciamo a realizzare le stesse funzioni:

![[Pasted image 20230509183200.png]]

In ogni passo nel ramo più a destra aggiungiamo il paralleo di 2 $2R$ per far dividere la corrente per 2 in ogni passo.
Abbinando lo schema con i deviatori di corrente otteniamo il nostro schema finale, garantendo che passi sempre corrente:

![[Pasted image 20230509183755.png]]

Nel caso di $1010$ abbiamo il primo e il terzo deviatore chiuso, quindi avremo:
$$
I_{tot}=I+\frac{I}{4}
$$
Supponendo $R=10 k\Omega$ $V_{R}=4V$ e $I=0.2mA$ allora abbiamo che $I_{tot}=0.25mA$

Usando questo approccio ho un consumo di potenza continuo. Per ovviare a ciò devo ricorrere a **reti di peso capacitive**.
In questo caso il consumo è ridotto:

![[Pasted image 20230509184332.png]]


