# Legge di controllo
---
Consideriamo un sistema dinamico a tempo continuo LTI, con un **solo ingresso** per semplicità $\implies B \in \mathbb{R}^{n \times 1}$
A questo punto ci chiediamo come sia possibile agire sull'ingresso in modo da:
- Rendere asintoticamente stabile un sistema instabile
- Cambiare le caratteristiche del movimento del sistema in modo da migliorare le proprietà di **smorzamento, e rapidità di convergenza**.
- Portare il sistema in una **dato stato di equilibrio**

Al fine di ottenere questi risultati occorre cambiare gli autovalori della matrice $A$ e questo può avvenire solo se l'ingresso $u(t)$ segue la **legge di controllo** del tipo:
$$
u(t)=-Kx(t)+ \alpha r(t)
$$
Dove $x(t)$ è lo stato del sistema, mentre $K$ rappresenta la matrice dei guadagni.
Il termine $\alpha$ è un fattore di scalabilità, pertanto possiamo affermare che $\alpha \in \mathbb{R}$
Il termine $r(t)$ è l'**ingresso di riferimento**.

### Retroazione statica dello stato

Consideriamo il seguente schema, dove appunto è presente **retroazione del sistema**:

![[Pasted image 20230406173103.png]]

Possiamo notare che l'ingresso del sistema $u(t)$ è somma di due contributi:
- Il **feedforward $\alpha r(t)$** che serve per imporre un dato movimento
- Il **feedback $-Kx(t)$** rappresenta appunto la retroazione dello stato

Possiamo dunque dire che la legge di controllo vista sopra è una **legge di controllo per la retroazione statica dello stato**.
Sostituendo il termine del nuovo ingresso nella solita equazione di stato si ottiene:
$$
\dot{x}(t)=(A-BK)x(t)+ B\alpha r(t)
$$
Questo sistema controllato è dunque quello che vogliamo controllare.

### Assegnazioni degli autovalori

A questo punto per controllare il sistema vogliamo che gli $n$ autovalori $A-BK$ coincidano con $n$ numeri fissati arbitrariamente.
Il problema non è banale, ma un **teorema** ci da un condizione necessaria e sufficiente affinchè sia possibile ammettere soluzione:

Il problema di assegnazione degli autovalori tramite retroazione statica dello stato presenta soluzione soltanto se la **coppia di matrici $(A,B)$ soddisfa la condizione di completa raggiungibilità**, ovvero:
$$
\rho(M_{R})=\rho([B \quad AB \quad \dots \quad A^{n-1}B])=n
$$
Nel caso di un sistema non completamente raggiungibile, allora la retroazione può modificare solamente $r$ autovalori corrispondenti alla sua **parte raggiungibile**.


### Matrice di trasferimento

Considerando  un sistema descritto dalle seguenti equazioni:
$$
\begin{cases}
\dot{x}(t)=(A-BK)x(t)+B\alpha r(t) \\
y(t)=(C-DK)x(t)+D \alpha r(t)
\end{cases}
$$
Allora passando nel dominio di Laplace otteniamo la **matrice di trasferimento** nella forma seguente:
$$
H(s)=\{(C-DK)[sI-(A-BK)]^{-1}B+D\} \alpha
$$

# Esempio
---
Consideriamo il seguente sistema:
$$
\dot{x}(t)= \begin{bmatrix}
1 & 3 \\
4 & 2
\end{bmatrix}x(t)+ \begin{bmatrix}
-1 \\
2
\end{bmatrix}u(t)
$$
Trovare se possibile i coefficienti della matrice di guadagno $K$ per ottenere tramite retroazione statica i seguenti autovalori del sistema: $\lambda_{1,des}=-2$ e $\lambda_{2,des}=-3$

Per prima cosa occorre verificare la completa raggiungibilità del sistema e per farlo calcoliamo la matrice di ragginugibilità:
$$
M_{R}=\begin{bmatrix}
B & AB
\end{bmatrix}=\begin{bmatrix}
-1 & 5 \\
2 & 0
\end{bmatrix} \implies \rho(M_{R})=2
$$
Poichè la dimensione del sistema è 2 allora il sistema è completamente raggiungibile.

Ora scriviamo il polinomio caratteristico desiderato:
$$
p_{des}(\lambda)=(\lambda+2)(\lambda+3)=\lambda^2+5\lambda +6
$$
A questo punto vogliamo che questo corrisponda al polinomio caratteristico della matrice $A-BK$:
$$
A-BK=\begin{bmatrix}
1 & 3 \\
4 & 2
\end{bmatrix}- \begin{bmatrix}
-1 \\
2
\end{bmatrix} \begin{bmatrix}
k_{1} & k_{2}
\end{bmatrix}= \begin{bmatrix}
1+k_{1} & 3+k_{2} \\
4-2k_{1} & 2-2k_{2}
\end{bmatrix}
$$
Cerchiamo a questo punto il polinomio caratteristico della matrice $A-BK$:
$$
p_{A-BK}(\lambda)=\lambda^2+(-3-k_{1}+2k_{2})\lambda + 8k_{1}-6k_{2}-10
$$
Ora vogliamo imporre che questo polinomio corrisponda a quello desiderato, perciò imponiamo l'uguaglianza fra il coefficiente di primo grado e il termine noto:
$$
\begin{cases}
-3-k_{1}+2k_{2}=+5 \\
8k_{1}-6k_{2}-10=6
\end{cases} \implies \begin{cases}
k_{1}=8 \\
k_{2}=8
\end{cases}
$$
Una volta fatto questo abbiamo finito perchè abbiamo determinato i coefficienti della matrice di guadagno $K$.

```ad-note
In Matlab è possibile calcolare la matrice di guadagno tramite due comandi:
- Autovalori con molteplicità unitaria: `K=place(A,B,p)` dove A e B sono le matrici del sistema, mentre p è il vettore con gli autovalori da assegnare
- Autovalori con molteplicità multipla: `K=acker(A,B,p)`
```



# Problema della regolazione
---
Consideriamo un sistema retroazionato, scegliamo una matrice $K$ tale per cui il sistema risulti asintoticamente stabile, SISO.
Data l'asintotica stabilità allora a seguito di un ingresso costante $\overline{r}$ lo stato e l'uscita tenderanno ai loro equilibri.

Ci chiediamo se sia possibile ottenere la seguente condizione:
$$
\overline{y}=\overline{r}
$$
Questo problema è noto come **regolazione dell'uscita**.
Si può dimostrare che per risolvere il problema basta agire sul coefficiente $\alpha$ andando a stabilire una **condizione di regolazione**:
$$
\alpha=[-(C-DK)(A-BK)^{-1}B+D]^{-1}
$$

### Esempi

Consideriamo il sistema seguente e calcolare il valore di $\alpha$ in modo da risolvere il problema di regolazione:
$$
\dot{x}(t)=\begin{bmatrix}
-1 & 0.5 \\
2 & -3
\end{bmatrix}x(t)+ \begin{bmatrix}
2 \\
4
\end{bmatrix}u(t)
$$
$$
y(t)=\begin{bmatrix}
3 & 6
\end{bmatrix}x(t)
$$
Per prima cosa occorre verificare l'asintotica stabilità, e successivamente calcolare $\alpha$ con la formula vista sopra.
Per la prima richiesta notiamo che il sistema è retroazionato quindi calcoliamo gli autovalori della matrice $A-BK$ ottenendo che essi sono $\lambda_{1}=-2$ e $\lambda_{2}=-4$. Poichè sono con parte reale negativa allora il sistema è asintoticamente stabile.

Poi possiamo calcolare $\alpha$ nel modo seguente:
$$
\alpha=[-(C-DK)(A-BK)^{-1}B+D]^{-1}=-(\begin{bmatrix}
3 & 6
\end{bmatrix} \begin{bmatrix}
-2 & 0 \\
0 & -4
\end{bmatrix}^{-1} \begin{bmatrix}
2 \\
4
\end{bmatrix})^{-1}=0.\overline{1}
$$

