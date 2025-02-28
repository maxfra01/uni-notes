# Formula di Lagrange
---
Suppongo di avere a che fare con un sistema dinamica LTI TC descritto dalle equazioni:$$ \begin{cases}
\dot{x}(t)=Ax(t)+Bu(t) \\
y(t)=Cx(t)+Du(t)
\end{cases}
$$Ricordando che A è quadrata nxn, B nxp, C qxm, D qxp.
La difficolta del risolvere un sistema del genere è determinato dalla risoluzione dell'equazione differnziale del primo ordine, nota la condizione iniziale $x(t=0_{\_})=x_{0}$ 

E' possibile calcolare $x(t)$ tramite una **formula di Lagrange**:
$$
x(t)=e^{At}x_{0}+\int_{0}^t e^{A(t-\tau)}Bu(\tau)\, d\tau = x_{l}(t)+x_{f}(t) 
$$Dove $x_{l}(t)$ è detto **movimento libero dello stato** (dipende solo da $x_{0}$), mentre il secondo termine $x_{f}(t)$ è detto **movimento forzato dello stato**, dipende solo da $u(t)$ (notare che il secondo termine è una convoluzione).

Per risolvere invece l'uscita di un sistema, basta andare a sostituire l'espressione di $x(t)$ nella formula finale:$$
y(t)=Ce^{At}x_{0}
+C\int _{0}^t e^{A(t-\tau)}Bu(\tau) \, d\tau \,+Du(t)=y_{l}(t)+y_{f}(t)$$dove analogamente, il primo termine è la **risposta libera**, mentre il secondo è la **risposta forzata**.

Tuttavia questo metodo è complesso per via degli integrali, quindi usiamo sempre **Laplace** per cambiare dominio.


# Dominio di Laplace
---
Nel corso usiamo la **trasformata di Laplace UNILATERA**:$$
F(s)=\mathcal{L}[f(t)]=\int _{0_{\_}}^\infty f(t)e^{-st}\, dt
$$dove la variabile $s$ è definita come $s=\sigma+j\omega$
Nel dominio di Laplace per caratterizzare un sistema occorre risolvere:$$
\begin{cases}
sX(s)-x(0_{\_})=AX(s)+BU(s) \\
Y(s)=CX(s)+DU(s)
\end{cases}
$$
```ad-hint
La trasformata di Laplace dell'esponenziale $e^{At}$ è la quantità $sI-A$ dove $I$ è la matrice identità
```

Per effettuare i seguenti calcoli si suppone che la quantità $sI-A$ sia una matrice **invertibile**:
$$
(sI-A) \space \textrm{ è invertibile} \quad \leftrightarrow \det A \neq 0  
$$
In particolare il determinante di $A$ è il **polinomio caratteristico** nella forma $s^n +\alpha s^{n-1}+\dots+\alpha_{0}$ i cui zeri sono gli **autovalori $\lambda_{i}$**. In sintesi le formule sotto valogono per ogni s tranne che per gli autovalori.

Risolvendo per $X(s)$ abbiamo che il **movimento dello stato**:$$
X(s)=(sI-A)^{-1} x(0)+ (sI-A)^{-1}BU(s)=H_{0}^x x(0)+ H_{f}^x U(s)
$$Il movimento **dell'uscita** è dato da: $$
Y(s)=C(sI-A)^{-1}x(0)+ [C(sI-A)^{-1}B+D]U(s)=H_{0}(s)x(0)+H(s)U(s)
$$Nel caso di sistema a **riposo** l'unica uscita è rappresentata dalla risposta forzata in quanto $x(0)$ è nullo.

$H(s)$ è detta **matrice di trasferimento del sistema**, oppure legame ingresso uscita.


# Funzione di trasferimento
---
### Forma polinomiale

Per un sistema SISO ($p=1, q=1$) possiamo esprimere la funzione di trasferimento nella forma:
$$
H(s)=\frac{N_{H}(s)}{D_{H}(s)}=\frac{b_{m}s^m+b_{m-1}s^{m-1}+\dots+ b_{0}}{s^n+a_{n-1}s^{n-1}+\dots+a_{0}} \quad \forall m\leq n
$$
Nel caso di:
- $m <n$ allora la fdt è strettamente **propria**.
- $m=n$ allora la fdt è non strettamente proprio (sistema improprio).
- Le radici di $N_{H}$ sono detti **zeri della funzione**, mentre quelli di $D_{H}$ sono i **poli**

### Forma zeri e poli
$$
H(s)=K_{\infty} \frac{(s-z_{1})(s-z_{2})\dots(s-z_{m})}{(s-p_{1})(s-p_{2})\dots(s-p_{n})}
$$
Il termine $K_{\infty}$ è detto **guadagno all'infinito** ed è pari a $$
K_{\infty}=\lim_{ s \to \infty } s^{n-m}H(s)
$$
### Radici complesse coniugate

Consideriamo due radici di un poliminio come ad esempio: $s_{1,2}=\sigma_{0}\pm j\omega_{0}$
Introduciamo una nuova rappresentazione basata su **pulsazione naturale**($\omega_{n}$) e **smorzamento**($\zeta$):

![[Pasted image 20230307145017.png]]

La pulsazione rappresenta la distanza dall'origine, mentre lo smorzamento l'inclinazione rispetto al semiasse immaginario positivo:
$$
\begin{cases}
\omega_{n}=\sqrt{ \sigma_{0}^2+\omega_{0}^2 } \\
\zeta=\sin \theta
\end{cases}
$$
Dai segni di queste due quantità deduciamo la posizione delle radici:
- Se $\zeta$ è positivo siamo nel semipiano sinistro
- Se $\zeta$ è negativo semipiano destro
- Se $\zeta$ è nullo abbiamo radici pure

E' possibile rappresentare un polinomio in funzione di pulsazione e smorzamento:
$$
s^2+2\zeta \omega_{n}s+\omega_{n}^2
$$
Usando questa quantità si ricava una nuova **rappresentazione**:

![[Pasted image 20230307145811.png]]



# Esempi
---
Faremo uso di scomposizione in fretti semplici e dei **residui**.
Avendo una $F(s)$ razionale e supponendo che ci siamo più poli che zeri andiamo ad ottenere una forma del tipo:
$$
F(s)=\frac{R_{1}}{s-p_{1}}+\frac{R_{2}}{s-p_{2}}+\dots +\sum_{i=1}^n \frac{R_{i}}{s-p_{i}}
$$
Dove le quantità $R_{i}$ sono detti residui e si calcolano (nel caso considerato) come:
$$
R_{i}=\lim_{ s \to p_{i} } (s-p_{i})F(s) 
$$
### Problema 1

Si consideri il sistema con equazione:
$$
\dot{x}(t)=\begin{bmatrix}
0 & 1 \\
-2 & -3
\end{bmatrix}x(t)+ \begin{bmatrix}
1 \\
0
\end{bmatrix}u(t)
$$
Con ingresso gradino alto 2, e condizioni iniziali $x(0)=2,2$
Passiamo nel dominio di Laplace trasformando l'equazione qui sopra.
Ricordiamo che $$
X(s)=(sI-A)^{-1}x(0)+(sI-A)^{-1}BU(s)
$$
$U(s)=\frac{2}{s}$ mentre le matrici si possono vedere dallo stato.
Il primo problema è calcolare l'inversa di $(sI-A)$ calcolabile come:
$$
(sI-A)^{-1}=\frac{1}{\det(sI-A)}Adj(sI-A)
$$
Dove con Adj intendiamo la matrice aggiunta composta dai complementi algebrici di $(sI-A)$: 

![[Pasted image 20230307151842.png]]

Il calcolo del movimento libero e forzato sono semplici moltiplicazioni, e sommando abbiamo:
$$
X(s)=\begin{bmatrix}
\frac{2s^2+10s+6}{s(s+1)(s+2)} \\
\frac{2s^2-4s-4}{s(s+1)(s+2)}
\end{bmatrix}
$$
Ora possiamo scomporre in fratti semplici lo stato e tornare nel dominio del tempo:
$$
x(t)=\begin{bmatrix}
3+2e^{-t}-3e^{-2t} \\
-2-2e^{-t}+6e^{-2t}
\end{bmatrix}\epsilon(t)
$$

### Fratti semplici con radici complesse

Nel denominatore devono comparire coppie del tipo $\sigma_{0} \pm j\omega_{0}$
A questo punto ci sarà qualcosa del tipo:
$$
F(s)=\frac{\dots}{(s-\sigma+j\omega)(s-\sigma-j\omega)(s-p_{3})\dots}
$$
Il procedimento rimane invariato, così come il calcolo dei residui, tranne che per il fatto che per coppie di radici coniugate si ha:
$$
R_{2}=R_{1}^*
$$
Ovvero il residuo della seconda radice è il coniugato del residuo della prima.
Quando antitrasformo ottengo:

![[Pasted image 20230307154810.png]]

Dove quest'ultima espressione è equivalente (usando le formule di Eulero) a:
$$
(R_{1}e^{(\sigma+j\omega)t}+R_{1}^*e^{\sigma-j\omega}t)\epsilon(t)=2|R_{1}|e^{\sigma t}\cos(\omega_{0}t+\textrm{arg} (R_{1})  )\epsilon(t)
$$


### Caso con radici multiple

Supponiamo che una funzione sia nella forma **strettamente propria** $F(s)=\frac{N}{D}$ dove N e D non hanno radici in comune.
Inoltre si richiede che il denominatore abbia $r (r<n)$ radici distinte ognuna con la sua molteplicità.

Indichiamo con $p_{i}$ i singoli poli del denominatore, ognuna con la sua molteplicità $\mu_{i}$ (se sommo tutte le $\mu_{i}$ ottengo il grado del denominatore); cerco di fattorizzare in modo da avere:
$$
F(s)=\frac{\dots}{(s-p_{1})^{\mu_{1}}(s-p_{2})^{\mu_{2}}\dots}
$$
Allora scomponendo in fratti semplici diventa:
$$
F(s)=\sum_{k=1}^{\mu_{1}} \frac{R_{1,k}}{(s-p_{1})^k}+ \sum_{k=1}^{\mu_{2}} \frac{R_{2,k}}{(s-p_{2})^k}+\dots=\sum_{i=1}^r \sum_{k=1}^{\mu_{i}} \frac{R_{i,k}}{(s-p_{i})^k}
$$
La formula per calcolare il residuo è la seguente:
$$
R_{i,k}=\lim_{ s \to p_{i} } \frac{1}{(\mu_{i}-k)!} \frac{d^{\mu_{i}-k}}{ds^{\mu_{i}-k}}[(s-p_{i})^{\mu_{i}}F(s)] \quad k=1,\dots,\mu_{i}
$$
E notiamo che nel caso $\mu_{i}=1$ si ottiene la formula dei residui nel caso più semplice.

### Problema 2

Si consideri il seguente LTI:$$
\dot{x}(t)=\begin{bmatrix}
0 & -16 \\
1 & -8
\end{bmatrix}x(t)+ \begin{bmatrix}
1 \\
2 \\

\end{bmatrix}u(t)
$$Con equazione di uscita $y(t)=[0 \quad 1]x(t)$, mentre lìingresso è una rampa di pendenza due, e condizioni inziali nulle. Cerchiamo il movimento dell'uscita.
Ricordiamo che 
$$
Y(s)=C(sI-A)^{-1}x(0)+ [C(sI-A)^{-1}B+D]U(s)=H_{0}(s)x(0)+H(s)U(s)
$$
Dato che le condizioni iniziali sono nulle esisterà solo la risposta forzata.
Possiamo passare al dominio di Laplace: 
$$
U(s)=\frac{2}{s^2}
$$
C è noto, D anche, U(s) anche, A anche, bisogna invertire $sI-A$:
$$
(sI-A)^{-1}=\frac{1}{\det(sI-A)}Adj(sI-A)
$$Svolgendo i calcoli otteniamo la matrice:
$$
(sI-A)^{-1}=\frac{1}{s^2+8s+16}  \begin{bmatrix}
s+8 & -16 \\
1 & s
\end{bmatrix}
$$
L'espressione della risposta forzata sarà data da:
$$
Y_{f}(s)=\begin{bmatrix}
0  & 1
\end{bmatrix} (sI-A)^{-1}\begin{bmatrix}
1 \\
2
\end{bmatrix} \frac{2}{s^2}=\frac{2(2s+1)}{s^2(s+4)^2}
$$
La risposta forzata ha due poli doppi in $s=0, s=-4$
La scomposizione in fratti semplici avrà la forma:
$$
Y(s)=\frac{R_{1,1}}{s}+ \frac{R_{1,2}}{s^2}+ \frac{R_{2,1}}{s+4} + \frac{R_{2,2}}{(s+4)^2}
$$
Calcolando i vari residui e antitrasformando:
$$
y(t)=(0.1875+0.125t-0.1875e^{-4t}-0.875e^{-4t})\epsilon(t)
$$

### Caso con F(s) non strettamente propria

Occorre procedere con la divisione polinomiale :(
Indichiamo con $K$ il quoziente mentre con $N'_{F}(s)$ il resto della divisione.
$$
F(s)=K+\frac{N'_{F}(s)}{D_{F}(s)}
$$
Essendo il secondo termine strettamente proprio possiamo applicare a questa nuova forma i metodi che abbiamo visto prima e procedere normalmente.
