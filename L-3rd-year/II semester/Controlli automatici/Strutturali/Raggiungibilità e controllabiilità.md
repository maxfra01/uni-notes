# Definizioni introduttive
---

### Raggiungibilità

La proprietà di **Raggiungibilità** descrive la possibilità di modificare lo stato, a partire da uno stato prefissato, andando ad agire sulla funzione d'ingresso.
Ci dice dunque quali stati possiamo raggiungere a partire da uno stato zero, e agendo sull'ingresso.

Uno stato $x^*$ si dice **raggiungibile** (da uno stato $x_{0}$ al tempo $t^*$) se esistono un istante di tempo $t^*$, e una certa funzione di ingresso per la quale è possibile avere:
$$
x(t^*)=x^*
$$
Considerando tutti gli stati raggiungibili da $x_{0}$ al tempo $t^*$ si va a definire un **insieme di raggiungibilità** $X_{R}(t^*)$, e quest'ultimo costutisce un sottospazio lineare dello **spazio di stato $X$**.

![[Pasted image 20230403140521.png]]

Definiamo il **sottospazio di raggiungibilità** come l'insieme di raggiungibilità di dimensione maggiore:
$$
X_{R}=max[ X_{R}(t)]
$$
Un sistema si dice **completamente raggiungibile** solo se:
$$
X_{R}=X
$$
Per sistemi non completamente raggiungibili definiamo l'insieme di **non raggiungibilità**:
$$
X_{NR}=X_{R}^{\perp}
$$

### Controllabilità

Uno stato $x^*$ si dice **controllabile** (allo stato zero $x_{0}$ al tempo $t^*$) solo se esistono un istante temporale, appunto $t^*$, e un certa funzione di ingresso tale per cui si ottiene:
$$
x(t^*)=x_{0}
$$
Come per la raggiungibilità definiamo **l'insieme di controllabilità** come l'insieme degli stati controllabili:

![[Pasted image 20230403141603.png]]

Il **sottospazio di controllabilità** $X_{C}$ è l'insieme di controllabilità di dimensione massima: $$
X_{C}=max [X_{C}(t)]
$$Un sistema è **completamente controllabile** solo se:
$$
X=X_{C}
$$
Il **sottospazio di non controllabilità** è:
$$
X_{NC}=X_{C}^{\perp}
$$

```ad-note
Il concetto di stato zero, solitamente non corrisponde allo stato nullo: $x_{0} \neq 0$. 
Durante il corso per semplicità si assumera che esso coincida con lo stato nullo, e allo stesso modo posso assumere $t_{0}=0$.
```


### Sistemi LTI, $X_{R},X_{C}$

Per **Sistemi LTI a tempo Continuo** possiamo affermare che:
$$
X_{R}=X_{C}
$$
Mentre per **LTI a tempo Discreto** si hanno due casistiche:
$$
\begin{cases}
X_{R} \subseteq X_{C}  & \textrm{in generale} \quad \\
X_{R}=X_{C} & \textrm{se la matrice A non è singolare} \quad    
\end{cases}
$$
Questo ci porta a dire che **in generale** per LTI si ha:
$$
X_{R} \subseteq X_{C}
$$
Quindi nel caso di un sistema **completamente raggiungibile** allora esso è anche **completamente controllabile**!
Proprio per questo ci limiteremo a studiare la sola raggiungigbilità!


### Parte raggiungibile e non

Chiamiamo $r$ la dimensione del sottospazio di raggiungibilità.
Supponendo di avere un sistema LTI di dimensione $n$ allora parliamo di:
- **Parte raggiungibile** quando consideriamo $r$ degli $n$ autovalori della matrice $A$
- **Parte non raggiungibile** quando consideriamo $n-r$ degli $n$ autovalori della matrice $A$.

![[Pasted image 20230404145529.png]]

Dallo schema notiamo che l'ingresso influenza solo le **parti raggiungibili** mentre la parte non raggiungibile **può influenzare** quella raggiungibile.

### Esempio con sistema elettrico

![[Pasted image 20230404144444.png]]

Avendo il seguente sistema, ci chiediamo cosa riesca a pilotare la corrente in ingresso. Per quanto riguarda $x_{2}(t)$ notiamo subito che non è **controllabile da $u(t)$**.
Per quanto invece riguarda la maglia di sinistra notiamo che la variabile di interesse è perfettamente controllabile dall'ingresso.



# Analisi della raggiungibilità per sistemi dinamici LTI
---
Consideriamo un sistema dinamico a tempo Discreto, descritto dall'equazione alle differenze:
$$
x(k+1)=Ax(k)+Bu(k)
$$
Per semplicità supponiamo che l'ingresso sia unico e che la condizione iniazle sia nulla. Procediamo in modo iterativo fino al secondo $l$:
$$
\begin{bmatrix}
x(1)=Ax(0)+Bu(0)=Bu(0) \\
x(2)=Ax(1)+B u(1)=ABu(0)+Bu(1) \\
x(3)=Ax(2)+Bu(2)=A^2Bu(0)+ABu(1)+ Bu(2) \\
\dots
\end{bmatrix}
$$
Proseguendo, al tempo $l$ si otterrà l'equazione con la seguente forma:
$$
x(l)=A^{l-1}Bu(0)+\dots+ABu(l-2)+Bu(l-1)
$$
Dove ad ogni passo la potenza di $A$ diminuisce, mentre l'indice dell'ingresso aumenta.

Compattando in forma matriciale ottengo:
$$
x(l)=\begin{bmatrix}
B & AB & \dots & A^{l-1}B
\end{bmatrix} \begin{bmatrix}
u(l-1) \\
u(l-2) \\
\dots \\
u(0)
\end{bmatrix}=M_{R}(l) \cdot U(l)
$$
Abbiamo quindi trovato che l'espressione dello stato al tempo $l$ è il prodotto fra la **matrice di raggiungibilità** e la sequenza di ingresso.
Possiamo **determinare l'insieme di raggiungibilità** come lo spazio immagine $\mathcal{R}(M_{R}(l))$ generato dalle colonne della matrice di raggiungibilità.

Per **determinare il sottospazio di raggiungibilità** di fatto si sceglie l'istante temporale $l$ per il quale la matrice $M_{R}(l)$ ha rango massimo.
Ogni volta che aggiungiamo una colonna alla matrice $M_{R}(l)$ è possibile che il rango aumenti o rimanga costante: questo vale fino a quando le colonne della matrice non eguagliano la dimensione $n$ del sistema.

La dimensione del sottospazio di raggiungibilità è pari al rango della matrice di raggiungibilità:
$$
\textrm{dim}(X_{R}) =\rho (M_{R})=r 
$$
La condizione di completa raggiungibilità è modificata e diventa:
$$
\rho(M_{R})=n
$$

```ad-note
Per sistemi a tempo continuo non cambia assolutamente nulla, e nel caso più generale si ha che la matrice di raggiungibilità è nella forma:

$$
M_R=[B \quad AB \quad ... \quad  A^{n-b}B], \quad b=\rho(B)
$$
Su MatLab possiamo calcolare $M_R$ con l'operatore M_R=ctrb(A,B).
```

```ad-info
Negli esercizi, nel calcolo della matrice di raggiungibilità, quando bisogna scrivere espressioni del tipo $A^2B$, occorre per semplicità fattorizzare ad esempio come $A(AB)$ dove $AB$ è stata calcolata in precedenza (la colonna prima).
```


# Problema della realizzazione
---
Dato un sistema dinamico LTI a tempo continuo o discreto, esso può essere rappresentato con **variabili di stato** oppure con **funzione di trasferimento**.

Per passare dalle equazioni di stato alla funzione di trasferimento, basta svolgere i calcoli e la soluzione è unica.

Per il problema inverso, ovvero **da funzione di trasferimento a equazioni di stato** non esiste una soluzione unica.
Questo porblema va sotto il nome di **realizzazione**.

Consideriamo una funzione di trasferimento, **che deve essere strettamente propria** del tipo:
$$
H(s)=\frac{b_{n-1}s^n+b_{n-2}s^{n-1}+\dots+b_{0}}{a_{n}s^n+a_{n.1}s^{n-1}+\dots+a_{0}}+b_{n}
$$
Allora la **forma canonica di raggiungibilità** è costituita nel seguente modo:
$$
A= \begin{bmatrix}
0 & 1 & 0 & 0 \\
\vdots & \ddots & \ddots & 0 \\
0 & \dots & 0 & 1 \\
-a_{0} & -a_{1}  & \dots  & -a_{n-1}
\end{bmatrix} \quad B=\begin{bmatrix}
0 \\
\vdots \\
0 \\
1
\end{bmatrix}
$$
$$
C= \begin{bmatrix}
b_{0} & b_{1} & \dots b_{n-1} 
\end{bmatrix}
\quad D= \begin{bmatrix}
b_{n}
\end{bmatrix}
$$
Questa forma canonica ha le seguenti **proprietà**:
- La matrice A è nella forma compagna inferiore dunque il suo polinomio caratteristico è: $\lambda^n+a_{n-1}\lambda^{n-1}+\dots+a_{1}\lambda+a_{0}$
- Il sistema A,B,C,D è **sempre completamente raggiungibile**

### Esempio di calcolo

$$
H(s)=\frac{s^2+3s+4}{s^3+s^2+2}= \frac{b_{2}s^2+b_{1}s+b_{0}}{s^3+a_{2}s^2+a_{1}s+a_{0}}
$$
Notiamo che il polinomio è nella forma propria, il termine $b_{n}$ (il resto) è nullo, l'ordine di $H(s)$ è 3, dunque le matrici avranno al massimo una dimensione pari a 3.

Andiamo semplicemente a scrivere le matrici come le abbiamo definite sopra, ottenendo:
$$
A=\begin{bmatrix}
0 & 1 & 0 \\
0 & 0 & 1 \\
-2 & -1 & -1
\end{bmatrix}, \quad B=\begin{bmatrix}
0 \\
0 \\
1
\end{bmatrix}, \quad C=\begin{bmatrix}
4 & 3 & 1
\end{bmatrix}, \quad D=0
$$
Secondo questa forma canonica, abbiamo quindi trovato le 4 matrici della rappresentazione in variabili di stato.

