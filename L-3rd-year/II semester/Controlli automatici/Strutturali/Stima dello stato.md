# Stimatore asintotico dello stato
---
Per controllare un sistema tramite retroazione statica dello stato è necessario che lo stao $x(t)$ sia misurabile, ma in generale per un sistema dinamico è nota solo l'uscita, che fornisce informazioni parziali.

Pertanto per controllare il sistema cerchiamo di **approssimare $x(t)$ con una sua stima $\hat{x}(t)$**. Successivamente si tratterà come usare questa stima per realizzare una **retroazione statica**:
$$
u(t)=-K \hat{x} +\alpha r(t)
$$
Per semplicità tratteremo un sistema **LTI TC SISO**, ma i risultati valgono anche per TD, e LTI MIMO.

### Stimatore dello stato

Per stimare lo stato si sfrutta l'**osservabilità** del sistema, e cercheremo di realizzare un **ricostruttore, o stimatore** dello stato:

![[Pasted image 20230421084656.png]]

Quest'oggeto prende come ingresso l'ingresso del sistema da considerare e l'uscita e come output fornisce una stima dello stato.

Possiamo definire l'**errore sulla stima** come la differenza in qualsiasi istante fra la stima dello stato e lo stato effettivo:
$$
e(t)=\hat{x}(t)- x(t)
$$
Uno stimatore per il quale **l'errore tende a zero con l'aumentare del tempo** è detto **stimatore asintotico dello stato**:
$$
\lim_{ t \to \infty } \|(t)\|=\lim_{ t \to \infty } \| \hat{x}(t)-x(t)\|=0
$$
```ad-note
La norma che consideriamo è quella quadratica:
$$
\| e \|=\sqrt{\sum_i e_i^2}
$$

```

### Primo modello di stimatore

Una prima soluzione per costruire uno stimatore è **costruire un clone del sistema dove forziamo una certa condizione iniziale**:

![[Pasted image 20230421085637.png]]

Partendo dallo stato possiamo calcolare l'errore di stima per questo modello:
$$
\begin{cases}
\hat{\dot{x}}(t) = A \hat{x}(t)+Bu(t)  \\
\dot{e}(t) = \hat{ \dot{x}(t)}-x(t)
\end{cases} \implies
 \dot{e}(t)=A( \hat{x}(t)-x(t))=Ae(t)
$$
Risolvendo questa eq. differenziale imponendo come condizione iniziale $e(t=0)=\hat{x}_{0}- x_{0}$ allora otteniamo come soluzione:
$$
e(t)=e(0) \cdot \exp(At) 
$$
Ora ci chiediamo in quale caso questo errore si smorza nel tempo:
- Se banalmente l'errore iniziale è nullo
- Tutti i modi della matrice $A$ sono convergenti e quindi il sistema sia **asintoticamente stabile**
Questo dunque **rappresenta un limite**.

### Secondo modello di stimatore

Per superare quest'ultimo occorre **usare l'uscita** e quindi **aggiungere un termine correttivo** ottenendo un'equazione del tipo:
$$
\hat{ \dot{x}}(t)=A \hat{x}(t)+Bu(t) -L( \hat{y}(t)-y(t))
$$
Dove $L$ è una matrice dei guadagni dello stimatore con $L \in \mathbb{R}^{n \times 1}$:

![[Pasted image 20230421090907.png]]

Tramite i calcoli analoghi a quelli del primo modello ricavo che l'errore della stima è pari a:
$$
e(t)=e(0) \cdot \exp[(A -LC)t]
$$
Perciò per ottenere uno stimatore asintoticamente stabile occorre che l'errore si smorzi nel tempo: questo avviene se **gli autovalori della matrice $A-LC$ sono asintoticamente stabili** perciò riduciamo il problema al **calcolo della matrice $L$**.

(Considerazioni sul calcolo di $L$, slide $15,16,17$)

Essendoci ricondotti al calcolo di una matrice tale che abbia certi autovalori possiamo procedere nel seguente modo:
- Verifico la **completa osservabilità** del sistema
- Produco il polinomio caratteristico desiderato
- Pongo l'uguaglianza fra il polonomio caratteristico vero e quello desiderato
- Determino i coefficienti della matrice $L$

### Esempio di calcolo

Considero il sistema:
$$
\begin{cases}
\dot{x}(t)= \begin{bmatrix}
0 & 1 \\
-1 & 2
\end{bmatrix} x(t)+ \begin{bmatrix}
0 \\
1
\end{bmatrix}u(t) \\
y(t)= \begin{bmatrix}
0 & 1
\end{bmatrix}x(t)
\end{cases}
$$
Calcoliamo i coefficienti di $L$ in modo da avere uno stimatore asintoticamente stabile con autovalori $\lambda_{1,des}=-10$ e $\lambda_{2,des}=-20$

Prima di tutto verifichiamo la completa osservabilità:
$$
M_{O}=\begin{bmatrix}
C \\
AC
\end{bmatrix}=\begin{bmatrix}
0 & 1 \\
-1 & 2
\end{bmatrix} \implies \rho(M_{O})=2=n
$$
Ora scriviamo il polinomio desiderato:
$$
p_{des}(\lambda)=(\lambda -(-10))(\lambda-(-20))=\lambda^2+30\lambda+200
$$
Mentre il polinomio vero della matrice $A-LC$ è pari a:
$$
p_{A-LC}(\lambda)= \det \begin{bmatrix}
0 & 1-l_{1} \\
-1 & 2-l_{2}
\end{bmatrix} = \lambda^2 + (l_{2}-2)\lambda + 1-l_{1}
$$
Da cui otteniamo 
$$
\begin{cases}
l_{2}-2=30 \\
1-l_{1}=200
\end{cases} \implies L= \begin{bmatrix}
l_{1} \\
l_{2}
\end{bmatrix}= \begin{bmatrix}
-199 \\
32
\end{bmatrix}
$$

```ad-note
Su Matlab si usano gli stessi comandi che abbiamo già visto per il calcolo della matrice dei guadagni $K$:
- Autovalori con molteplicità unitaria: `L=place(A,C,p);`
- Autovalori con molteplicità maggiore di 1: `L=(A,C,p);`
```


# Regolatore dinamico
---
Una volta noto lo stato stimato è possibile implementare una retroazione statica, creando quello che si chiama **regolatore dinamico**:

![[Pasted image 20230421095116.png]]

Questa particolare struttura di controllo ha 3 elementi che la compongono:
- Sistema **da controllare**
- **Stimatore** dello stato
- **Legge di controllo**

### Progettazione del regolatore

Le equazioni che descrivono il regolatore dinamico sono:

![[Pasted image 20230421100918.png]]

Pertanto considerando $n$ variabili di stato del sistema da controllare ed $n$ variabili di stato per lo stimatore asintotico, abbiamo un sistema con $2n$ incognite con $2n$ equazioni.

Introduciamo come vettore di stato l'espressione e conseguentemente le equazione del sistema:
$$
x_{tot}^{I}(t)=\begin{bmatrix}
x(t) \\
\hat{x}(t)
\end{bmatrix} \implies \begin{cases}
\dot{x}^I_{tot}(t)=A^I_{reg}x_{tot}^I(t)+B^I_{reg}r(t) \\
y(t)=C_{reg}^I x^I_{tot}(t)+ D_{reg}^Ir(t)
\end{cases}
$$
Dove le matrici valgono rispettivamente:
$$
A_{reg}^I= \begin{bmatrix}
A & -BK \\
LC & A-BK-LC
\end{bmatrix} \quad B_{reg}^I=\begin{bmatrix}
B \\
B
\end{bmatrix}\alpha \quad
$$
$$
 C_{reg}^I=\begin{bmatrix}
C & -DK
\end{bmatrix} \quad D_{reg}^I=\begin{bmatrix}
D
\end{bmatrix}\alpha
$$
Purtroppo in questa forma la matrice $A_{reg}^I$ non permette di evidenziare in modo immediato l'influenza delle matrice $K,L$ sugli autovalori del sistema.

Prendiamo invece come vettore di stato la quantità:
$$
x_{tot}^{II}(t)=\begin{bmatrix}
x(t) \\
\hat{x}(t)- x(t)=e(t)
\end{bmatrix} \implies \begin{cases}
\dot{x}^{II}_{tot}(t)=A^{II}_{reg}x_{tot}^{II}(t)+B^{II}_{reg}r(t) \\
y(t)=C_{reg}^{II} x^{II}_{tot}(t)+ D_{reg}^{II}r(t)
\end{cases}
$$
Con rispettavemente le matrici che valgono:
$$
A_{reg}^{II}= \begin{bmatrix}
A-BK & -BK \\
0_{n \times n} & A-LC
\end{bmatrix} \quad B_{reg}^{II}=\begin{bmatrix}
B \\
0_{n \times 1}
\end{bmatrix}\alpha \quad
$$
$$
 C_{reg}^{II}=\begin{bmatrix}
C-DK & -DK
\end{bmatrix} \quad D_{reg}^{II}=\begin{bmatrix}
D
\end{bmatrix}\alpha
$$
Si nota che la matrice $A^{II}_{reg}$ risulta **triangolare a blocchi**, pertanto i suoi autovalori sono immediatamente riconducibi all'unione fra gli autovalori di $A-BK$ e quelli di $A-LC$, ovvero la **proprietà di separazione**:
$$
\lambda(A_{reg}^{II})=\{\lambda(A-BK) \,  \cup \, \lambda(A-LC)\}
$$
Questa proprietà ci permette di **studiare individualmente** la matrice dei guadagni $K$ e quella dei guadagni dello stimatore $L$ in modo analogo a quello già visto.

### Matrice di trasferimento

Si dimostra che la matrice di trasferimento del sistema dinamico controllato tramite regolatore dinamico è pari a:
$$
H(s)=\alpha\{(C-DK)[sI-(A-BK)]^{-1}B +D\}
$$
Per calcolare $\alpha$ ci si avvale della stessa espressione usata per determinare la retroazione statica:
$$
\alpha=[-(C-DK)(A-BK)^{-1}B+D]^{-1}
$$

### Esempio di progetto del regolatore dinamico

Un esempio di calcolo è nelle slide ( a partire dalla slide $53$).
Considero il sistema:
$$
\begin{cases}
\dot{x}(t)=\begin{bmatrix}
0 & 1 \\
900 & 0
\end{bmatrix}x(t)+\begin{bmatrix}
0 \\
-9
\end{bmatrix}u(t) \\
y(t)=\begin{bmatrix}
600 & 0
\end{bmatrix}x(t)
\end{cases}
$$
Vogliamo progettare il regolatore dinamico che ha:
- Per lo stimatore $\lambda_{1,des}=\lambda_{2,des}=-100$
- Per la legge di controllo autovalori complessi e coniugati con $\omega_{n,des}=45$ e $\zeta_{des}=0.2$

Per prima cosa verifichiamo completa raggiungibilità e osservabilità:
$$
M_{R}=\begin{bmatrix}
B & AB
\end{bmatrix}=\begin{bmatrix}
0 & -9 \\
-9 & 0
\end{bmatrix} \quad M_{O}=\begin{bmatrix}
C \\
CA
\end{bmatrix}=\begin{bmatrix}
600 & 0 \\
0 & 600
\end{bmatrix}
$$
Entrambe le matrici hanno rango massimo e coincidente con la dimensione del sistema perciò abbiamo sia completa raggiungibilità che controllabilità.

Partiamo da $K$, calcoliamo la matrice $A-BK$, il polinomio desiderato e quello reale e lo uguagliamo:
$$
A-BK=\begin{bmatrix}
0 & 1 \\
900+9k_{1} & 9k_{2}
\end{bmatrix} \quad p_{A-KB}=\lambda^2-9k_{2}\lambda -900 -9k_{1}
$$
Da cui ricaviamo che $k_{1}=-325$ e $k_{2}=-2$

Per la matrice $A-LC$ stesso procedimento da cui ricaviamo:
$$
L=\begin{bmatrix}
l_{1} \\
l_{2}
\end{bmatrix}=\begin{bmatrix}
0. \overline{3} \\
18.1 \overline{6}
\end{bmatrix}
$$

