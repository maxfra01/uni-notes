# Elementi Base
---
Consideriamo un corpo **omogeneo ideale** di capacità termica $C$, l'equazione che descrive la sua temperatura è:
$$
C\frac{ \partial \theta(t) }{ \partial t } =p(t)
$$
Dove $\theta(t)$ è la **temperatura assoluta**, mentre $p(t)$ è la **potenza termica/portata di calore**.

Posso considerare un **conduttore termico ideale** caratterizzato da una **conduttanza termica $K_{ij}$**:

![[Pasted image 20230313140032.png]]

Allora la portata di calore che fluisce da un corpo all'altro è:
$$
p_{ij}(t)=K_{ij}(\theta_{i}(t)-\theta_{j}(t))
$$
### Scrittura delle equazioni dinamiche

Vanno scritte per ogni corpo ideale **non termostatato**, e seguono la forma:
$$
C_{i}\dot{\theta}_{i}(t)=\sum_{k}p_{k}^{est}(t)-\sum_{j}^{j \neq i}p_{k}^{int}(t)
$$
Dove le portate interne tengono conto dell'interazione fra corpi, mentre quelle esterne tengono conto del mondo esterno.

Il procedimento per ricavare la rappresentazione in variabile di stato è il medesimo per tutti gli altri sistemi, notando però:
1. I corpi termostatati vanno nel vettore degli ingresso
2. Le variabili si stato sono le temperature dei corpi non termostatati

# Esempio
---
Ricavare la rappresentazione in v.d.s. assumendo come variabile di interesse $\theta_{1}$

![[Pasted image 20230313140735.png]]

Per i due corpi 1,2 non termostatati:
- $C_{1}\dot{\theta_{1}}=p_{0}-K_{1i}(\theta_{1}-\theta_{i})-K_{12}(\theta_{1}-\theta_{2})- K_{1e}(\theta_{1}-\theta_{e})$
- $C_{2}\dot{\theta_{2}}=0-K_{12}(\theta_{2}-\theta_{1})-K_{2i}(\theta_{2}-\theta_{i})-K_{2e}(\theta_{2}-\theta_{e})$

Ora come variabili di stato e ingressi:
$$
x(t)=\begin{bmatrix}
\theta_{1}(t) \\
\theta_{2}(t)
\end{bmatrix}=\begin{bmatrix}
x_{1}(t) \\
x_{2}(t)
\end{bmatrix} \quad \quad u(t)=\begin{bmatrix}
p_{0}(t) \\
\theta_{e}(t) \\
\theta_{i}(t)
\end{bmatrix}=\begin{bmatrix}
u_{1}(t) \\
u_{2}(t) \\
u_{3}(t)
\end{bmatrix}
$$
E l'uscita:
$$
y(t)=\theta_{1}=x_{1}(t)
$$
Svolgendo i calcoli:

![[Pasted image 20230313141517.png]]

Classifichiamo il sistema come dinamico, dimensione finita (2), MIMO.
Se suppongo i coefficienti costanti nel tempo allora è LTI e ammette la rappresentazione matriciale.