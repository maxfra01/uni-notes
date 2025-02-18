# Discrete Fourier Transform DFT
---
La DTFT è una funzione di variabile continua $f$ (oppure $\omega$) quindi introduciamo una nuova trasformata (campionando in frequenza la DTFT con frequenza di campionamento $$f_{k}=\frac{k}{N}\quad k=0,1,\dots N-1$$
Definiamo dunque la DFT, applicabile a sequenze fino a $N$:
$$
X(k)=\sum_{n=0}^{N-1}x(n)e^{-2j\pi \frac{k}{N} n} \quad \forall k=0,1,\dots N-1
$$
Vale la proprietà per la convoluzione circolare, per cui quest'ultima, nel dominio della trasformata, equivale a un prodotto.

```ad-note
Dato che devono essere noti N-1 campione di x(n) allora potrebbe essere necessarario aggiungere degli zeri che non hanno significato al segnale
```


# Analisi in frequenza tramite DFT
---
Lavoriamo sotto alcune ipotesi, tra cui $x(t)$ ha durata limitata nel tempo, inoltre ancche la sua banda è limitata nel tempo.
Per la frequenza della DFT la chiamiamo freq. analogica $f_{a}$

Il segnale viene campionato, rispettiamo il teorema e prendiamo la sequenza $$
x(n)=x(nT_{c})
$$
Il numero di campioni $N$ è tale da includere tutto il supporto temporale del segnale.
Nel dominio della frequenza possiamo esprime il segnale come: $$
X_{c}(f_{a})=\frac{1}{T_{c}} \sum X_{T}\left( f_{a}-\frac{n}{T} \right)
 \implies X(f_{a})$$
 Da cui ricaviamo che:$$
X(e^{j 2\pi f})=X_{c}(f_{a})|_{f_{a}=f \cdot f_{c}}
$$
Infine se andiamo a valutare $f=\frac{k}{N}$ allora otteniamo il risultato finale $$
X_{c}\left( \frac{k}{N} f_{c} \right )=DFT[x(n)]
$$
Dunque abbiamo messo in relazione il campionamento di un segnale con la DFT, ottenendo 3 parametri di interesse, $T_{0},T_{c},N$ rispettivamente il periodo di osservazione nel dominio del tempo, il tempo di campionamento e il numero di campioni.
Definiamo la **risoluzione in frequenza** è pari a  $$
\Delta f=\frac{f_{c}}{N}=\frac{1}{NT_{c}}=\frac{1}{T_{0}}
$$
![[Pasted image 20230119102208.png]]

 Occorre scegliere con attenzione $T_{0}, f_{c}$ e stimare il numero di campioni come $$
N=\frac{T_{0}}{T_{c}}$$
E si prende la prima potenza di 2 maggiore del valore ottenuto.