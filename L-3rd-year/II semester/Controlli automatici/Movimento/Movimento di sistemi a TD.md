# Concetti base della Z-transform
---
La **trasformata zeta** della sequenza di valori reali $f(k)$ è la variabile complessa data da:
$$
F(z)=\sum_{k=0}^\infty f(k) z^{-k} \quad z:\mathbb{C}
$$
Tra le principali proprietà abbiamo:

- **linearità**
$$
\mathcal{Z}[c_{1}f_{1}(k)+c_{2}f_{2}(k)]=c_{1}F_{1}(z)+c_{2}F_{2}(z)
$$
- **ritardo di h passi**: 
$$
\mathcal{Z}[f(k-h)]=z^{-h}F(z)
$$
- **anticipo di h passi**:
$$
\mathcal{Z}[f(k+h)]=z^h\left( F(z)-\sum_{k=0}^{h-1} z^{-k}f(k) \right)
$$
- **prodotto e convoluzione**:
$$
\mathcal{Z}[f_{1}(k)*f_{2}(k)]=F_{1}(z) \cdot F_{2}(z)
$$

Valgono i principali due teoremi **del valore finale ed iniziale**:

- Sia supponga che il prodotto $(z-1)F(z)$ non abbia radici del denominatore con modulo maggiore o uguale ad uno, allora:l
$$
\lim_{ k \to \infty }f(k)=\lim_{ z \to 1 } (z-1)F(z) 
$$
- Teorema del valore iniziale:
$$
\lim_{ k \to 0 }f(k)=\lim_{ z \to \infty }F(z)  
$$

# Studio del movimento del sistema
---

### Formula di Lagrange

E' possibile, in modo iterativo, ricavare l'espressione della formula di Lagrange per calcolare il movimento libero e forzato del sistema.
$$
x(k)=A^kx(0)+\sum_{i=0}^{k-1}A^{k-i-1}Bu(i)
$$
Dove il primo termine $A^kx(0)$ è detto **movimento libero dello stato $x_{l}(k)$** mentre il secondo termine $\sum_{i=0}^{k-1}A^{k-i-1}Bu(i)$ è detto **movimento forzato $x_{f}(k)$** dello stato.

Analogamente per l'uscita si ricava:
$$
y(k)=y_{l}(k)+y_{f}(k)=CA^kx(0)+C \sum_{i=0}^{k-1}A^{k-i-1}Bu(i)+Du(k)
$$

### Calcolo nel dominio $\mathcal{Z}$

Possiamo partire dalla rappresentazione dello stato e andare nel dominio di zeta:
$$
\begin{cases}
x(k+1)=Ax(k)+Bu(k) \\
y(k)=Cx(k)+Du(k)
\end{cases} \implies
\begin{cases}
zX(z)-zx(0)=AX(z)+BU(z) \\
Y(z)=CX(z)+DU(z)
\end{cases}
$$
A questo punto possiamo ricavare le espressioni di $X(z)$ e sostituendo $Y(z)$, dunque:
$$

X(z)=z(zI-A)^{-1}x(0)+(zI-A)^{-1}BU(z) \\
=H_{0}^x(z)x(0)+H_{f}^x(z)U(z)
$$
$$
Y(z)=zC(zI-A)^{-1}x(0)+[C(zI-A)^{-1}B+D]U(z)=H_{0}(z)x(0)+H(z)U(z)
$$
Dove $H(z)$ è detta **matrice di trasferimento**.


### Esempio di soluzione

Consideriamo il seguente sistema descritto dalle seguenti equazioni:
$$
x(k+1)=\begin{bmatrix}
3 & 0 \\
-3.5 & -0.5
\end{bmatrix}x(k)+ \begin{bmatrix}
1 \\
2
\end{bmatrix}u(k)
$$
$$
y(k)=\begin{bmatrix}
1 & -1
\end{bmatrix}x(k)
$$
Determiniamo il movimento di $x(k)$ e $y(k)$ nel caso in cui l'ingresso sia un gradino di altezza 2, e le condizioni iniziali siano: $x(0)=[1 \space \space -2]^T$

Procediamo con la trasformata di $u(k)$ che è pari a: $U(z)=\frac{2z}{z-1}$
Ora scriviamo la matrice $zI-A$:
$$
zI-A=\begin{bmatrix}
z-3 & 0 \\
3.5 & z+0.5
\end{bmatrix}
$$
Invertire la matrice è semplice perchè è 2x2:
$$
(zI-A)^{-1}=\frac{1}{(z-3)(z+0.5)}\begin{bmatrix}
z+0.5 & 0 \\
-3.5 & z-3
\end{bmatrix}
$$
Possiamo quindi calcolare il movimento libero:
$$
X_{l}(z)=z\begin{bmatrix}
\frac{1}{z-3} \\
\frac{-2z+2.5}{(z-3)(z-0.5)}
\end{bmatrix}
$$
Mentre il movimento forzato è pari a:
$$
X_{f}(z)=z\begin{bmatrix}
\frac{2}{(z-3)(z-1)} \\
\frac{4z-19}{(z-3)(z-0.5)(z-1)}
\end{bmatrix}
$$
Sommando ed antitrasformando (vedi slide):
$$
x(k)=\begin{bmatrix}
x_{1}(k) \\
x_{2}(k)
\end{bmatrix}=\begin{bmatrix}
2\cdot3^k-1 \\
-2\cdot 3^k-5(-0.5)^k+5
\end{bmatrix}\epsilon(k)
$$
Per $y(k)$ non andiamo nel dominio della z perchè basta ricordare che $y(k)=Cx(k)$ dunque svolgiamo i conti e otteniamo:
$$
y(k)=(4 \cdot 3^k+5(-0.5)^k-6)\epsilon(k)
$$
