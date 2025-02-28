# Equilibrio di sistemi dinamici
---
Per **equilibrio** di un sistema dinamico si intende un generico movimento costante di un sistema quando:
- l'ingresso è costante $u(t)=\overline{u}$
- lo stato del sistema è costante nel tempo e pari al valore iniziale: $x(t)=x(t=0)=\overline{x}$
- l'uscita del sistema è costante nel tempo $y(t)=\overline{y}$

Definiamo il **punto di equilibrio** come la coppia di valori $(\overline{x},\overline{u})$

### Equilibrio per sistemi a TC

Dato un sistema MIMO, a tempo continuo, non lineare, stazionari esso è in equilibrio solamente se:
$$
\dot{x}(t)=f(\overline{x},\overline{u})=0
$$
Nel caso di sistemi **LTI** è possibile usare la rappresentazione matriciale, dunque la condizione di equilibrio può essere riscritta come:
$$
A \overline{x}=- B \overline{u} \quad \quad y=C \overline{x}+D \overline{u}
$$

In particolare se la **matrice A è invertibile** allora lo stato di **univoco** ed è pari a:
$$
\overline{x}=-A^{-1}B \overline{u}
$$
A cui corrisponde a una sola uscita.

Se la matrice $A$ è **singolare** ($\det A=0$) allora possono esistere infiniti stati di equilibrio.

### Esempio

Considero la seguenti matrici: $$
A=\begin{bmatrix}
1 & 0 \\
1 & 0
\end{bmatrix} \quad B=\begin{bmatrix}
1 \\
0
\end{bmatrix}
$$
Voglio determinare l'espressione degli stati di equilibrio, dunque noto che la matrice $A$ è singolare, allora per cercare gli stati impongo l'equilibrio:
$$
\dot{\overline{x}}=0=A \overline{x}+B \overline{u} \implies \begin{cases}
0=\overline{x_{1}} + \overline{u} \\
0=\overline{x_{1}}
\end{cases} \implies \begin{cases}
\overline{x_{1}}=- \overline{u} \\
\overline{x_{1}}=0
\end{cases}
$$
Nel caso di $\overline{u}=0$ allora esistono infiniti stati di equilibrio, in quanto il vincolo è imposto solo sulla prima componente:
$$
\overline{x}=\begin{bmatrix}
0 \\
c
\end{bmatrix}, c: \mathbb{R}
$$
E si parla in questo caso di **equilibrio non isolato**.
Nel caso di ingresso non nullo non esistono stati di equilibrio.


### Equilibrio per sistemi a TD

Nel caso generale ci troviamo di fronte a equazioni alle differenze dove vogliamo, per l'equilibrio, che non appaia la dipendendenza da $k$, ad esempio $x(k+1)=x(k)=\overline{x}$ perciò le soluzioni del sistema sono del tipo:
$$
f(\overline{x},\overline{u})=\overline{x}
$$

Nel caso di un sistema a tempo discreto LTI, dove è possibile usare la rappresentazione matriciale abbiamo:
$$
x(k+1)=x(k)=\overline{x}=A \overline{x}+ B \overline{u}
$$
E dunque le soluzioni del sistema sono nella forma:
$$
(I-A) \overline{x}=B \overline{u}
$$
Possiamo disntinguere due casi a seconda dell'invertibilità di $(I-A)$:
- Se essa è invertibile ($\det \neq 0$) allora esiste un solo stato di equilibrio: $$
\overline{x}=(I-A)^{-1} B \overline{u}
$$
- Se essa non è invertibile gli stati di equilibrio o non esistono o sono infiniti.


### Esempio

Considerare il sistema e studiare l'equilibrio a TD:
$$
A=\begin{bmatrix}
1 & 0 \\
1 & 0
\end{bmatrix} \quad B=\begin{bmatrix}
1 \\
0
\end{bmatrix}
$$
Imponiamo la condizione:
$$
\overline{x}=A \overline{x}+ B \overline{u} \implies \begin{cases}
\overline{x_{1}}= \overline{x_{1}}+ \overline{u} \\
\overline{x_{2}}=\overline{x_{1}}
\end{cases} \implies \begin{cases}
\overline{u}=0 \\
\overline{x_{2}}=\overline{x_{1}}
\end{cases}
$$
Perciò per ingressi nulli abbiamo infiniti stati di equilibrio non isolati del tipo:
$$
\overline{x}=\begin{bmatrix}
c \\
c
\end{bmatrix} \quad c: \mathbb{R}
$$
Mentre per ingressi non nulli non esistono stati di equilibrio

```ad-note
Per equilibrio **isolato** si intende una situazione in cui preso un punto di equilibrio e considerato un intorno a piacere, non è possibile trovare un altro punto di equilibrio
```


# Esempio di calcolo dell'equilibrio
---

![[Pasted image 20230320140349.png]]

Imponiamo la condizione di equilibrio: $\dot{x}(t)=\dot{\overline{x}}=0$
Otteniamo in questo modo:
$$
\begin{cases}
\overline{x_{2}}=0 \\
g-\frac{\left( \frac{k_{i}}{M} \right)u^2}{x_{1}^2}=0
\end{cases} \implies \begin{cases}
\overline{x_{1}}= \sqrt{  \frac{k_{i}}{Mg} } |\overline{u}| \\
\overline{x_{2}}=0
\end{cases}
$$
L'unica uscita calcolabile è pari a:
$$
y(t)=\overline{y}=\overline{x_{1}}= \sqrt{  \frac{k_{i}}{Mg} } |\overline{u}|
$$
