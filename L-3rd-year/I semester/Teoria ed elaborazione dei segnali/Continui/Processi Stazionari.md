# Definizione
---
A volte ci interessa considerare segnale che non inizio nell'origine nell'asse dei tempi $t=0$, dunque introduciamo il concetto di **processo stazioanario**:
Se un certo segnale appartiene a un processo casuale allora tutte le sue repliche traslate fanno parte di quel processo con la stessa probabilità:
$$
se\space x(t) \in P \to \forall t_{0} \space x(t-t_{0}) \in P
$$
$$
se \space x(t) \in P \to P[x(t)]=P[x(t-t_{0})]
$$
La precedente proprietà ci dice che le statistiche congiunte di $n$ campioni non dipendono dall'origine dell'asse tempi ma solo dalla differenza di tempo fra i campioni.

Se pongo $t_{0}=-t_{1}$ ottengo la **statistica del prim'ordine** (ovvero la densità di probabilità) dove non compare la dipendenza del tempo
$$
f_{X}(x_{1},t_{1})=f_{X}(x_{1},t_{1}-t_{1})=f_{X}(x_{1},0)
$$Le **statistiche del secondo ordine** dipendono dalla differenza fra due campioni ovvero:
$$
\tau=t_{2}-t_{1}
$$
In generale le **statistiche di ordine $n$** sono funzioni di $n-1$ variabili temporali.

## Processi Stazionari in senso stretto

I processi per cui è valida la relazione:
$$
f_{X}(x_{1},x_{2},\dots x_{m},t_{1},t_{2},\dots t_{m}) \implies f_{X}(x_{1},x_{2},\dots x_{m},\tau_{1}\dots \tau_{m}) \quad \forall m\leq n
$$
Sono processi stazionari in **senso stretto di ordine $n$** in cui le statistiche di ordine $n$ dipendono solo da $n-1$ variabili che rappresentano la differenza di tempo con il primo campione.

- la media di questi processi di ordine 1 non dipende dal tempo
- l'autocorrelazione di ordine 2 dipende solo dalla differenza di tempo fra le osservazioni. ($\tau$)

## Processi casuali in senso lato

Un processo è **Wide Sense Stationary WSS** quano le precedenti priorità valgono per la media e l'autocorrelazione, ovvero:
$$
m_{X}(t)=m_{X} \quad R_{X}(t_{1}-t_{2})=R_{X}(t_{1}-t_{2})
$$
I sistemi stazionari del secondo ordine in senso stretto sono anche WSS, ma non viceversa.
Inoltre per questa famiglia di segnali vale:
$$
R_{X}(\tau)=E\{X(t)X^*(t-\tau)\} \quad \quad R_{X}(\tau)=R_{X}^*(-\tau)
$$

Per un WSS REALI abbiamo autocorrelazione:
- pari
- nell'origine è **pari al valor quadratico medio**
- forma tipo

![[Pasted image 20221111112107.png]]

Un segnale non possiede memoria infinita quindi:
$$
x(t) \space x(t-\tau) \quad \tau \to \infty
$$
I due segnali $x(t)$ e $x(t-\tau)$ sono scorrelati!
Questo ha un eccezione, se considero ad esempio una variabile casuale a valore costante con media nulla.
Per processi lenti il processo e il relativo traslato abbiamo più correlazione, mentre per processi veloci risultano meno collegati.


# Densità spettrale di potenza
---
Per i processi WSS è possibile definire la potenza del processo come il valor quadratico medio edl segnale
$$
P_{X}\hat{=} E\{X^2(t)\}
$$
Per questi processi è ragionevole pensare che la media di insieme del processo al quadrato sia considerata come la potenza media.

Lo **spettro di potenza** definito secondo il teorema di Wiener-Khintchine è pari a:
$$
S_{X}(f)= \mathcal{F}(R_{X}(\tau))=\int R_{X}(\tau)e^{-2j\pi f\tau} \, d\tau 
$$
Questa quantità è sempre pari e sempre positiva.
Inoltre a partire dallo spettro si può ricavare la potenza media del processo casuale:
$$
P_{X}=E\{X^2(t)\}=R_{X}(0)=\int_{-\infty}^{\infty} S_{X}(f) \, df 
$$



