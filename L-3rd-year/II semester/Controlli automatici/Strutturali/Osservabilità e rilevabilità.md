# Definizioni introduttive
---

### Osservabilità

A partire dalle misure dell'uscita $y(t)$ e l'ingresso $u(t)$ è possibile ricavare lo stato **iniziale** del sistema $x_{0}$.
Sia $x^* \neq 0$ uno stato del sistema. Esso si dice **non osservabile** se il movimento della risposta libera, a partire da questo stato, rimane sempre stabile per un certo intervallo di tempo, ovvero:
$$
y_{l}(t)=0, \quad \forall t \in [t_{0},t^*]
$$
L'insieme di tutti gli stati non osservabili è dato dall'insieme di **non osservabilità $X_{NO}(t^*)$** al tempo $t^*$

Il **sottospazio di non osserabilità** è definito come l'insieme di non osservabilità con cardinalità minima:
$$
X_{NO}=min_{t}[X_{NO}(t)]
$$
Ragionevolmente definiamo il **sottospazio di osservabilità** come il sottospazio orgonale a quello di non osservabilità:
$$
X_{O}=X_{NO}^{\perp}
$$
Un sistema si dice **Completamente osservabile** solamente se:
$$
X=X_{O}
$$
Valgono le definizioni di parte osservabile e non, analogamente a parte raggiungibile e non.

![[Pasted image 20230418150034.png]]


### Rilevabilità

A partire dalle misure dell'uscita $y(t)$ e l'ingresso $u(t)$ è possibile ricavare lo stato **finale** del sistema $x_{finale}$.
Sia $x^* \neq 0$ uno stato del sistema. Esso si dice **non osservabile** se il movimento della risposta libera, che **ha come stato finale $x_{finale}$**, rimane sempre stabile per un certo intervallo di tempo, ovvero:
$$
y_{l}(t)=0, \quad \forall t \in [t_{0},t^*]
$$
L'insieme di tutti gli stati finali che rispettano questa condizioni si raggruppano nell'**insieme di non rilevabilità $X_{ND}(t^*)$**

L'insieme di cardinalità minima da come risultato il **sottospazio di non rilevabilità**:
$$
X_{ND}=min_{t}[X_{ND}(t^*)]
$$
Il **sottospazio di rilevabilità** è il complemento ortogonale del sottospazio di non rilevabilità:
$$
X_{D}=X_{ND}^{\perp}
$$
Inoltre, un sistema è **completamente rilevabile** solo se:
$$
X=X_{D}
$$


```ad-note
Per sistemi LTI abbiamo che $X_O \subseteq X_D$, perciò se un sistema è completamente osservabile allora è anche completamente rilevabile.
Perciò ci limitermo a studiare le proprietà di osserabilità
```


# Analisi di osservabilità per LTI
---
Consideriamo un sistema **LTI a TD** con le solite due equazioni che descrivono stato e uscita.
Vogliamo:
- Trovare l'insieme di non osservabilitò $X_{NO}(l)$ al tempo $l$
- Trovare il sottospazio di non osservabilità
- Trovare il sottospazio di osservabilità
- Trovare una condizione necessaria e sufficiente per la completà osservabilità

Consideriamo che la dimensione dell'uscita del sistema sia 1 e inoltre l'ingresso $u(k)=0$
Allora scriviamo l'espressione della risposta libera:
$$
\begin{cases}
y(0)=Cx(0) \\
y(1)=Cx(1)=CAx(0) \\
\dots \\
y(l)=y_{l}(l)=CA^lx(0)
\end{cases}
$$
Oppure nella forma matriciale abbiamo:
$$
\begin{bmatrix}
y(0) \\
y(1) \\
\vdots \\
y(l)
\end{bmatrix}=\begin{bmatrix}
C \\
CA \\
CA^2 \\
\vdots \\
CA^l
\end{bmatrix}x(0)=M_{O}(l)x(0)
$$
La matrice $M_{O}(l)$ rappresenta il legame fra la sequenza delle uscita $y(l)$ con lo stato inziale $x(0)$
L'insieme di non osservabilità corrisponde allo **spazio nullo $\mathcal{N}(M_{O}(l))$** che coincide con gli insieme degli stati che danno risposta libera nulla. Dunque:
$$
X_{NO}(l)=\mathcal{N}(M_{O}(l))=\mathcal{N}\begin{bmatrix}
C \\
CA \\
\vdots \\
CA^l
\end{bmatrix}
$$
La dimensione dello spazio nulla della matrice di non osservabilità al tempo $l$ è **minima** quando il rango di $M_{O}(l)$ è **massimo**, ovvero quando $l=n-1$

Definiamo quindi la **matrice di osservabilità** $M_{O}$ come:
$$
M_{O}=\begin{bmatrix}
C \\
CA \\
\vdots \\
CA^{n-1}
\end{bmatrix} \implies X_{NO} =\mathcal{N}(M_{O})
$$
Ora considerando una proprietà dell'algebra si ha che $X_{O}=\mathcal{R}(M_{O}^T)$ si ha che la dimensione di $X_{O}$ è pari al rango di $M_{O}$.

Perciò abbiamo trovato una **condizione necessaria e sufficiente per la completà osservabilità** (e quindi anche per la completà rilevabilità), ovvero che il **rango della matrice di osservabilità corrisponda con la dimensione del sistema**:
$$
\rho(M_{O})=n
$$

```ad-note
In Matlab è possibile calcolare la matrice di osservabilità con il comando `obsv` secondo la seguente sintassi: `M_O=obsv(A.C)` dove A e C sono le matrici in rappresentazione di stato.
```


### Esempio

Considero il sistema descritto dalle equazioni:
$$
\begin{cases}
\dot{x}(t)=\begin{bmatrix}
1 & 0 & 0 \\
0 & 0 & 1   \\
1 & -2 & -3
\end{bmatrix}x(t)+ \begin{bmatrix}
0 \\
2 \\
2 \\
\end{bmatrix}u(t) \\
y(t)=\begin{bmatrix}
1 & 1 & 0
\end{bmatrix}x(t)
\end{cases}
$$
Studiarne le proprietà di osservabilità.
Dato che sono note le matrici $A,C$ del sistema possiamo calcolare la matrice di **osservabilità**:
$$
M_{O}=\begin{bmatrix}
C \\
CA \\
CA^2
\end{bmatrix}= \begin{bmatrix}
1 & 1 & 0 \\
-1 & 0 & 1 \\
2 & -2 & -3
\end{bmatrix}
$$
A questo punto ne calcoliamo il rango:
$$
\det M_{O} =1 \neq 0 \implies \rho(M_{O})=3
$$
Siccome la dimensione del sistema è pari a 3 e coincide con il rango della matrice di osservabilità allora possiamo affermare che il sistema è **completamente raggiungibile**.


# Problema della realizzazione
---
Il problema della realizzazione consiste nel passare a una rappresentazione in variabile di stato, data come partenza una funzione di trasferimento.
Una possibile soluzione è stata ottenuta tramite una forma canonica di raggiungibilità.

Ora come possibile soluzione introduciamo la **forma canonica di osservabilità**:
Data una funzione di trasferimento in forma strettamente propria:
$$
H(s)=\frac{b_{n-1}s^{n-1}+\dots+b_{1}s+b_{0}}{a_{n}s^n+a_{n-1}s^{n-1}+\dots+a_{1}s+a_{0}} +b_{n}
$$
Allora una sua possibile realizzazione è data dalle matrici nella seguente forma:
$$
A=\begin{bmatrix}
0 & \dots & 0 & -a_{0} \\
1 & \ddots &  \ddots & -a_{1} \\
0 & \ddots & 0 & \vdots \\
0 & \dots & 1 & -a_{n-1}
\end{bmatrix} \quad B=\begin{bmatrix}
b_{0} \\
b_{1} \\
\vdots \\
b_{n-1}
\end{bmatrix}
$$
$$
C= \begin{bmatrix}
0 & \dots & 0 & 1
\end{bmatrix} \quad D=\begin{bmatrix}
b_{n}
\end{bmatrix}
$$
La seguente forma ha le due proprietà:
- La matrice $A$ è in **forma compagna destra** e dunque il suo polinomio caratteristico è: $\lambda^n+a_{n-1}\lambda^{n-1}+\dots+a_{1}\lambda+a_{0}$
- Il sistema individuato dalle precedenti 4 matrici $A,B,C,D$ risulta essere **sempre completamente osservabile**


# Principio di dualità
---
E' possibile studiare le proprietà di raggiungibilità e osservabilità tramite lo studio del **sistema duale associato**.

Per prima cosa occorre definire il sistema **duale**: considero per prima cosa un sistema **primale** $S^P$ descritto dalla matrici $A,B,C,D$.
Se a questo punto effettuo la sostituzione:$$
A \to A^T,B \to C^T, C \to B^T,D \to D^T
$$ovvero traspongo tutte le matrici, ottengo il sistema duale:
$$
\begin{cases}
\dot{w}(t)=A^Tw(t)+C^Tv(t) \\
z(t)=B^Tw(t)+D^Tu(t)
\end{cases}
$$
Si dimostra che il **sottospazio di raggiungibilità** del sistema primale coincide con il sottospazio di osservabilità del sistema duale, e viceversa:
$$
X_{R}^P=X_{O}^D \quad X_{O}^P=X_{R}^D
$$
Pertanto si ottiene il **principio di dualità**:
Un sistema primale $S^P(A,B,C,D)$ è completamente raggiungibile (osservabile) se e solo se il sistema duale $S^D(A^T,C^T,B^T,D^T)$ è completamente osservabile (raggiungibile).

![[Pasted image 20230419092405.png]]

### Esempio

Dato
$$
\dot{x}(t)=\begin{bmatrix}
0 & 1 \\
-1 & 2
\end{bmatrix}x(t)+ \begin{bmatrix}
1 \\
1
\end{bmatrix}u(t) \quad y(t)=\begin{bmatrix}
0 & 2
\end{bmatrix}x(t)
$$
Studiare l'osservabilità tramite il principio di dualità.
Per prima cosa ricavo il sistema duale:
$$
\dot{w}(t)=A^Tw(t)+C^Tv(t) \quad y(t)=B^Tw(T) D^Tv(t)
$$
Ovvero trasponendo le matrici:
$$
\dot{w}(t)=\begin{bmatrix}
0 & -1 \\
1 & 2
\end{bmatrix}w(t)+\begin{bmatrix}
0 \\
2
\end{bmatrix}v(t) \quad \quad y(t)=\begin{bmatrix}
1 & 1
\end{bmatrix}w(t)
$$
A questo punto studiamo la raggiungibilità che dipende dalle due matrici della prima equazione:
$$
M_{R}^D=\begin{bmatrix}
C^T & C^TA^T  
\end{bmatrix}= \begin{bmatrix}
0 & -2 \\
2 & 4
\end{bmatrix}
$$
Notiamo che il rango è massimo pertanto $S^D$ risulta **completamente raggiungibile**, perciò $S^P$ è **completamente osservabile**.

```ad-note
Per lo studio delle proprietà di raggiungibilità ed osservabilità  
di sistemi LTI bisogna sempre eseguire i metodi diretti (ed evitare il passaggio al sistema duale).
```
