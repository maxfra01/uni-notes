# Richiami di geometria

L'obiettivo di questa sezione è quello di adattare tecniche sui vettori per usarle per l'analisi dei delle [[Grandezze di interesse sui segnali]]; in particolare cercheremo di rappresentare un segnale come un **vettore di numeri**.

Alcune definizioni di vettori estese ai segnali:

## Distanza euclidea fra segnali
spesso lavoreremo con la distanza al  quadrato $d^2(x,y)$$$d(x,y)=\sqrt{\int_{-\infty}^{\infty}|x(t)-y(t)|^2dt}$$
Per misurare quanto sono 'vicini' due segnali si utilizza la distanza al quadrato: più la distanza è piccola più il segnale è vicino.

## Prodotto scalare
Possiamo definire il prodotto scalare di due segnale a partire dalla definizione sui vettori$$\langle x(t), y(t)\rangle=\int_{-\infty}^{\infty}x(t)y^*(t)dt$$
## Norma di un segnale
Come per il prodotto scalare possiamo trovare una relazione con la norma$$||x(t)||=\sqrt{\int_{-\infty}^{\infty}x(t)x^*(t)dt}=\sqrt{\langle x(t), x(t)\rangle}$$
L'energia del segnale si può anceh definire come la norma al quadrato
$$E(x)=||x||^2$$
## Ortogonalità fra segnali
Due segnali si dicono ortogonali solamente se il loro prodotto scalare è nullo
$$
\int_{-\infty}^{\infty}x(t)y^*(t)dt=0
$$
## Riassumendo

![[Pasted image 20220930165836.png]]

Alcune definizioni già viste in [[Grandezze di interesse sui segnali]].

# Spazio dei segnali

Data una certa base di segnali ortonormali potremmo cercare di esprimere ogni segnale della base come combinazione lineare dei vettori$$x(t)=\sum\limits_{i=1}^M\alpha_iw_i(t)$$
A questo punto, se la base è nota, ciò che distingue il segnale sono gli $M$ coefficienti della combinazione.
Dunque associamo al segnale che cerchiamo di ottenere il vettore: $$X=(\alpha_1,\alpha_2,...\alpha_M)$$
Questo ci permette di estendere le operazioni precedenti anche alla rappresentazione con i coefficienti.
Si dimostra ad esempio che l'energia del segnale è equivalente fra le due possibili rappresentazioni, ovvero $E(X)=E(x(t))$

## Energia della somma di segnali
Si dimostra che l'energia della somma è pari alla somma delle energie più due volte la parte reale del prodotto scalare fra i due$$E(x+y)=E(x)+E(y)+2Re[\langle X, Y\rangle]$$
Se poi i segnali si considerano **ortogonali** allora annulliamo l'ultimo contributo e otteniamo che l'energia della somma è la somma delle energie.

## Scelta dei coefficienti
Ci domandiamo ora come costruire la combinazione lineare che approssimi meglio il segnale.
Cerchiamo dunque di minimizzare la differenza fra il segnale e la sua approssimazione (dimostrazione su slide).
Si ottiene che l'$i$-esimo coefficiente migliore è dato dal prodotto scalare fra il segnale reale e l'$i$-esimo vettore della base
$$
\alpha_{i}=\langle x,w_{i}\rangle\quad \hat X=\sum\limits_{i=1}^{n}\langle x,w_{i}\rangle w_i
$$
## Scelta della base
Come costruire una base ortonormale per rappresentare un insieme di segnali.
Usiamo la procedura di **Gram-Schmidt**.
- Step 1: prendo il primo segnale $x_1$ , lo normalizzo
- Dallo step 2 alla fine uso questo algoritmo $$\hat w_i=x_i-\sum\limits_{k=1}^{i-1}\langle x_{i,}w_{k\rangle}w_{k}\quad, \quad w_i=\frac{\hat w_i}{||w_i||}$$
- La procedura termina quando ottengo un vettore in output pari a 0, perchè significa che è combinazioni lineare dei precedenti.
- Se immaginiamo di lavorare con $N$ segnali, ottenere una base con $M\leq N$ elementi.