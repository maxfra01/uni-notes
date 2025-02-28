Per analizzare l'andamento di LTI TC in modo preciso occorre utilizzare la trasformata di Laplace (quantitativa), invece per un andamento **qualitativo** si fa uso dell'analisi modale.

# Modi naturali
---
Vogliamo studiare le caratteristiche del movimento libero $x_{l}(t)$ di un sistema LTI TC che dipende da:
$$
\dot{x}(t)=Ax(t)
$$
La complessità risiede dunque nel calcolare la matrice $e^{At}$, ma poniamoci nella situazione in cui:
- la matrice $A$ ha autovalori reali e distinti
- la matrice $A$ è diagonale
Allora è possibile calcolare la matrice $e^{At}$ nel seguente modo:
$$
e^{At}=\begin{bmatrix}
e^{\lambda_{1}t} & 0 & \dots & 0 \\
0 & e^{\lambda_{2}t} & \dots & 0 \\
0 & \dots & e^{\lambda_{3}t} & 0 \\
0 & 0 & 0 & e^{\lambda_{4}t}
\end{bmatrix}
$$
Otteniamo una matrice che è ancora diagonale e i cui elementi dipendono dagli autovalori di $A$.

Considero funzioni del tipo:
$$
m_{i}(t)=e^{\lambda_{i}t}
$$
Questi sono detti **modi naturali**, o modi propri del sistema associato all'autovalore $\lambda_{i}$.
Non sono sempre esponenziali pure, ma variano a seconda della molteplicità, se reali o complessi.


# Analisi modale per tempo continuo
---

L'analisi modale si concentra sull'andamento **sintotico** dei vari modi naturali di un sistema, ovvero vado a studiare i modi per un tempo $t \to \infty$
Definiamo la seguente topologia:
- **Convergente**se solo se:$$
\lim_{ t \to \infty } |m(t)|=0
$$
- **Limitato** solo se:$$
0\leq |m(t)| \leq M\leq \infty
$$
- **Divergente** solo se:$$
\lim_{ t \to \infty } |m(t)|= \infty
$$

E' possibile dimostrare che la matrice esponenziale si può calcolare come:
$$
e^ {At}=Te^{\widetilde{A}t}T^{-1}
$$
Dove $T$ è una matrice costante, mentre $\widetilde{A}$ è una matrice in forma di Jordan (diagonale oppure diagonale a blocchi).

### Autovalori in $\mathbb{R}$, semplici

Caso più semplice di tutti, come quella nella definizione sopra:
D'ora in poi compariranno le immagini di $e^{\widetilde{A}t}$:

![[Pasted image 20230314152005.png]]

A seconda del valore dell'autovalore abbiamo i seguneti casi:
1. Autovalore $<0$, allora è **esponenzialmente convergente**
2. Autovalore $=0$, allora il modo associato è **limitato**
3. Autovalore $>0$, allora il modo associato sarà **divergente**


### Autovalori in $\mathbb{C}$, semplici

I **blocchi della matrice esponenziale** corrispondono a coppie di autovalo complessi coniugati con molteplicità unitaria del tipo $\lambda=\sigma \pm j\omega$ saranno del tipo:

![[Pasted image 20230314152536.png]]

I modi associati si possono classificare in:
1. **esponenzialmente convergenti** se $\sigma<0$
2. **limitati (oscillanti)** se $\sigma=0, \omega \neq 0$
3. **esponenzialmente divergenti** se $\sigma >0$


### Autovalori in $\mathbb{R}$, multipli

I blocchi della matrice esponenziale, con autovalori $\lambda_{i}$, rispettivamente con le loro molteplicità $\mu_{i}$ sono del tipo:

![[Pasted image 20230314154159.png]]

I modi associati sono:
1. **esponenzialmente convergenti** se $\lambda<0$
2. **polinomialmente divergenti**, se $\lambda=0$
3. **esponenzialmente divergenti**, se $\lambda>0$

```ad-note
Il termine $\mu'$ fa riferimento alla molteplicità **geometrica**, ed è possibile calcolarne il valore tramite $\mu'=\mu -d +1$ dove $d$ è la **dimensione dell'autospazio** associato all'autovalore e $\mu$ è la molteplicità algebrica.

La dimensione dell'autospazio associato è il numero di vettori $v$ linearmente indipendenti che ottengo imponendo il vincolo: $(\lambda I-A)v=0$
```


### Autovalori in $\mathbb{C}$ multipli

Considerando autovalori del tipo $\lambda=\sigma+j\omega$, essi daranno origine a $\mu'\leq\mu$ modi naturali del tipo:

![[Pasted image 20230315092221.png]]

Sono:
- **Esponenzialmente convergenti** per $\sigma<0$
- **Polinomialmente divergenti** per $\sigma=0$
- **Esponenzialmente divergenti** per $\sigma>0$

### Esercizio

Considero la matrice $A$ e determinare le caratteristiche dei modi naturali:
$$
A=\begin{bmatrix}
-1 & 0 & 0 & 0 \\
3 & -2 & 0 & 0 \\
-6 & 0 & 4 & 0 \\
-3 & 1 & 10 & 0
\end{bmatrix}
$$
La matrice è triangolare, dunque gli autovalori si leggono dunque sulla diagonale principale e sono -1,-2-4-0.
Sono tutti reali e distinti, quindi siamo nel primo caso:
$\lambda_{1} \to e^{-t}$ esponenzialmente convergente
$\lambda_{2} \to e^{-2t}$ esponenzialmente convergente
$\lambda_{3} \to e^{4t}$ esponenzialmente divergente
$\lambda_{4} \to e^{0t}$ limitato costante


### Costante di tempo

Per un modo **convergente** associato ad un autovalore $\lambda$ con $Re(\lambda)<0$ si definisce come **costante di tempo** la quantità:
$$
\tau=|\frac{1}{Re(\lambda)}|
$$
Questa costante da informazioni riguardo la velocità di convergenza a 0 del modo associato. In particolare se una costante di tempo $\tau'$ è minore di un'altra $\tau$ allora il modo associato alla costante minore $\tau'$ converge più in fretta.

Graficamente è possibile valutare $\tau$ come l'intersezione fra l'asse dei tempi (ascisse) e la retta tangente alla curva $e^{-\lambda t}$ nel punto di ascissa nulla:

![[Pasted image 20230315093419.png]]


### Esercizio 2

Si consideri la matrice $A$ e si determini l'andamento dei modi e calcolare le costanti di tempo:
$$
A=\begin{bmatrix}
-0.5 & 5 & 0 & 0 \\
-5  & -0.5 & 0 & 0 \\
-6 & 0 & 0 & 1 \\
-3 & 1 & -3 & -4
\end{bmatrix}
$$
E' una matrice triangolare a blocchi composta dai blocchi $A_{11}, A_{22}$:
$$
A_{11}=\begin{bmatrix}
-0.5 & 5 \\
-5 & -0.5
\end{bmatrix} \quad \quad A_{22}=\begin{bmatrix}
0 & 1 \\
-3 & -4
\end{bmatrix}
$$
E' possibile dunque ricavare gli autovalori dalle due sottomatrici:
$\lambda_{1,2}=-0.5 \pm 5j$
$\lambda_{3}=-1$, $\lambda_{4}=-3$
Per i primi 2, l'inviluppo è strettamente negativo perciò sarà **esponenzialmente convergente**
Per il terzo e quarto abbiamo ancora modi esponzialmente convergenti
Per tutti questi convergenti posso calcolare la costante di tempo:
$$
\tau_{1,2}=|\frac{1}{0.5}|=2s \quad \tau_{3}=\frac{1}{|-1|}=1s \quad \tau_{4}=\frac{1}{|-3|}=0.3s
$$


# Analisi modale per tempo discreto
---
Nel contesto discreto il ruolo della matrice esponenziale (per sistemi TC) è ricoperto dalla matrice $A^k$, avremo dunque a che fare con potenze di autovalori.

### Autovalori in $\mathbb{R}$, semplici

I blocchi di $\widetilde{A}^k$ hanno forma diagonale e danno luogo ad autovalori del tipo:
$$
\begin{bmatrix}
\lambda_{1}^k  & 0 & 0 \\ 
0 & \lambda_{2}^k
 & 0	 \\
0 & 0 & \lambda_{3}^k
\end{bmatrix}
$$
I modi associati agli autovalori sono:
1. Geometricamente convergenti se$|\lambda_{i}|<1$
2. Limitato se $|\lambda_{i}=1|$
3. Geometricamente divergente se $|\lambda_{i}>1|$

Inoltre notiamo che se $\mathcal{Re}(\lambda_{i})<0$ diamo origine a un **modo alternato**.


### Autovalori in $\mathbb{C}$, semplici

Gli autovalori sono del tipo $\lambda=\sigma \pm j\omega=\nu e^{\pm j\theta}$s dunque la forma della matrice è del tipo:

![[Pasted image 20230317110016.png]]

Modi:
1. Geometricamente convergenti se $\nu <1$
2. Limitati se $\nu=1$ e inoltre $\theta \neq 0$
3. Geometricamente divergenti se $\nu>1$

### Autovalori in $\mathbb{R}$, multipli

![[Pasted image 20230317110500.png]]

Per i modi stesse considerazioni del caso semplice.

### Autovalori in $\mathbb{C}$, multipli

![[Pasted image 20230317110538.png]]

Stesse considerazioni modali per il caso semplice.


### Esempio

Considerare la matrice seguente di un sistema a tempo **discreto**:
$$
A=\begin{bmatrix}
0.1 & 0 & 0 & 0 \\
0 & -2 & 0 & 0 \\
0 & 0 & -0.4 & 0 \\
0 & 0 & 0 & 0
\end{bmatrix}
$$
Dato che la matrice è diagonale si hanno i seguenti autovalori associati.
$\lambda_{1}=0.1^k$ geometricamente convergente
$\lambda_{2}=(-2)^k$ geometricamente divergente
$\lambda_{3}=(-0.4)^k$ geom convergente
$\lambda_{4}=0^k=\delta(k)$ geom convergente, in quanto gli impulsi convergono a zero in un passo.


