# Modello a Linea di trasmissione
---
Se scegliamo di modellare l'interconnessione come un filtro passa basso RC, allora il modello matematico è lo stesso visto per il modello a cavo equipotenziale:

![[Pasted image 20230418180753.png]]

In questo caso la resistenza del cavo va in serie con quella del driver, mentre la capacità del cavo va in parallello a quella del receiver, ma i calcoli sono gli stessi.

### Linea di trasmissione

Preferiamo usare altri **modelli a parametri concentrati** che tengono conto di eventuali induttanze nei cavi reali.
A volte si aggiungono anche **condensatori con capacità parassite**.
Se la linea è lunga queste capacità, induttanze e resistenze si mettono in serie e ce ne sono molte:

![[Pasted image 20230418181030.png]]

Consideriamo quest'ultimo modello in particolare senza perdite, se ne mettiamo tanti in fila, otteniamo una **Linea di trasmissione** (senza perdite).
Notiamo inoltre che in corrente continua, allora la linea si comporta come una connessione equipotenziale.

Abbiamo perciò definito questa linea come un modello dipendente da diversi parametri:

![[Pasted image 20230418181250.png]]

| Parametro    | Significato                     |
| ------------ | ------------------------------- |
| $Z_{\infty}$ | Impedenza caratteristica        |
| $L$          | Lunghezza dell'interconnessione |
| $P$          | Velocità di propagazione        |
| $t_{p}$      | Tempo di propagazione (di volo)                                |

Noi considereremo solo modelli a linea senza perdite, ovvero resistenza del cavo nulla e capacità parassite assenti.

Possiamo calcolare alcuni parametri a partire da specifiche fisiche come la **capacità unitaria $C_{U}$** e l'**induttanza unitaria**:
$$
Z_{\infty}=\sqrt{ \frac{L_{U}}{C_{U}} } \quad \quad P= \frac{1}{\sqrt{ L_{U}C_{U} }} \quad \quad t_{p}=\frac{L}{P}
$$
Valori tipici di $Z_{\infty}$ sono $10\dots1000 \Omega$, mentre per $P$ si hanno valori simili a $0.6c\dots 0.8c$ dove $c$ è la velocità della luce.

I modelli a linea sono fondamentali per **collegamenti lunghi e segnali con transizioni veloci**.


# Riflessione e pilotaggio
---
Consideriamo un driver modellato con generatore e resistenza di uscita $R_{O}$. Esso pilota una linea senza perdite:

![[Pasted image 20230418182644.png]]

Se metto in ingresso un gradino, si forma un partitore di tensione con l'impedenza del cavo, perciò la tensione che entra effettivamente nella linea è:
$$
V_{B}=\frac{Z_{\infty}}{Z_{\infty}+R_{O}}V_{A}
$$
Il segnale che arriva inizia a propagarsi sulla linea, con una certa velocità. Dopo un tempo $t_{p}$ il segnale raggiungerà l'estremo opposto.
A seconda di ciò che collegato alla fine della linea ho dei **fenomeni di riflessione**.

![[Pasted image 20230418183335.png]]

E' possibile evitare ciò solamente nel caso di una resistenza esattamente di valore $Z_{\infty}$, ovvero non c'è discontinuità.
Se la resistenza $R_{T} \neq Z_{\infty}$ l'energia del gradino non è ben assorbita, dunque si genera un'**onda riflessa**.
L'**onda incidente** invece è il gradino che viaggiava sulla linea. 
Si definisce dunque un **coefficiente di riflessione** pari a:
$$
\Gamma_{T}=\frac{R_{T}-Z_{\infty}}{R_{T}+Z_{\infty}}
$$
Il coefficiente ci dice quanto il sistema dista dalla condizione di adattamente (ovvero quando $R_{T}=Z_{\infty}$).
L'onda riflessa si propagherà nel verso opposto in modo proporzionale al coefficiente di riflessione e all'ampiezza dell'onda **progressiva** (incidente):
$$
V_{r}=\Gamma_{T}V_{p}
$$
Notiamo che nei casi limite in cui $R_{T}=Z_{\infty}$ abbiamo $\Gamma_{T}=0$ dunque non c'è riflessione.
Nel caso di circuito aperto abbiamo invece $\Gamma=1$ e quindi avrò una retropropagazione.
Nel caso di corto $\Gamma_{T}=-1$ avrò un segnale opposto e quindi tensione nulla alla terminazione.

Notiamo inoltre sulla linea che la **tensione che troviamo è la somma del contributo dell'onda incidente e riflessa**.

### Riflessioni multiple

![[Pasted image 20230418184101.png]]

$v_{1}$ sarà determinata dal partitore che si forma tra driver e linea di interconnessione. Posso andare a definire $v_{2}=v_{1}\Gamma_{T}$ e la tensione totale sulla linea $V_{C}=v_{1}+v_{2}$

Se c'è una riflessione, il segnale torna indietro fino al driver e se si trova una resistenza alla terminazione diversa allora si può generare un'altra onda riflessa.
Questo fenomeno può portare a una tensione totale sulla linea che non raggiunge la soglia di 1 logico, dunque non è valido in questo scenario.

![[Pasted image 20230418184623.png]]

![[Pasted image 20230418184849.png]]



# Esercizio C2.1
---
![[Pasted image 20230426113903.png]]

Siccome il driver è modellabile con un generatore e una resistenza interna allora la tensione iniziale sulla linea è data dal partitore di tensione sulla linea:
$$
V_{A}=V_{DD} \cdot \frac{Z_{\infty}}{Z_{\infty}+R_{0}}
$$
Dato che ho un circuito aperto in terminazione si avra $\Gamma_{T}=1$
Quando l'onda riflessa torna indietro trova un coeffficiente di terminazione pari a $\Gamma_{G}=\frac{1}{3}$.
Nella situazione a regime si ha che la tensione sulla linea è pari a $V_{DD}$ in quanto non passando corrente allora la linea si troverà allo stesso potenziale del driver.
I gradini di tensione sulla linea tenderanno a $5V$ man mano che il tempo passa.