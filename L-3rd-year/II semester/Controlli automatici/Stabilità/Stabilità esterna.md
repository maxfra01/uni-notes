# Relazione fra rappresentazione di sistemi
---
- La descrizione **in variabile di stato** (interna) è la più completa, che fornisce informazioni per calcolare osservabilità, raggiungibilità, stabilità interna ecc...
- La descrizione tramite **funzione di trasferimento** fornisce una caratterizzazione solo parziale del sistema, in quanto dipende solo dalla risposta forzata e quindi dalla parte osservabile e raggiungibile.

### Sistema in forma **minima** 

Un sistema dinamico LTI è detto in forma minima se e soltanto **se è completamente raggiungibile e completamente osservabile**.
Possiamo anche elencare le due proprietà:
- La rappresentazione interna di un sistema in forma minima contiene **sempre il numero minimo di variabili di stato**.
- La funzione di trasferimento di un sistema in forma minima **non prevede cancellazioni zero-polo**, ovvero gli autovalori della matrice di stato compaiono tutti come poli a denominatore della funzione


# Stabilità esterna
---
Un sistema dinamico, LTI, a dimensione finita, inizialmente a riposo è **BIBO stabile (esternamente stabile)** se la sua risposta forzata ad un qualsiasi ingresso limitato rimane sempre limitata nel tempo:
$$
\forall \overline{u} \in (0,\infty), \, \overline{\exists}y \in (0,\infty): \|u(t)\|< \overline{u}, \forall t \geq 0
\implies \|y(t)\|< \overline{y}, \, \forall t \geq 0 
$$

Condizioni per la BIBO stabilità:
| Sistema              | Condizione                                                                                                      |
| -------------------- | --------------------------------------------------------------------------------------------------------------- |
| A tempo **Continuo** | Dopo aver effettuato le cancellazioni zero-polo, tutti i poli devono avere parte reale strettamente minore di 0. |
| A tempo **Discreto** | Dopo aver effettuato le cancellazioni zero-polo, tutti i poli hanno modulo minore di 1.                                                                                                               |

Possiamo definire le seguenti implicazione:

| Condizioni                                                | Implicazione                 |
| --------------------------------------------------------- | ---------------------------- |
| Sistema asintoticamente stabile $\implies$                | Sistema esternamente stabile |
| Sistema esternamente stabile E in forma minima $\implies$ | Sistema asintoticamente stabile                             |


# Risposta in regime permanente
---
Consideriamo un sistema dinamico, tempo continuo, LTI descritta da:
$$
\begin{cases}
\dot{x}(t)=Ax(t)+Bu(t) \\
y(t)=Cx(t)
\end{cases}
$$
Allora il **movimento dello stato $x(t)$** può essere espresso come:
$$
x(t)=x_{omog}(t)+x_{part}(t)
$$
Dove $x_{omog}(t)$ è la soluzione dell'equazione omogenea associata all'equazione di stato (con $u(t)=0$) , mentre $x_{part}(t)$ è soluzione particolare dell'equazione di stato, che dipende da $u(t)$.
Possiamo allora scrivere la risposta del sistema come:
$$
y(t)=C[x_{omog}(t)+x_{part}(t)]=y_{omog}(t)+y_{part}(t)
$$

![[Pasted image 20230427112922.png]]

Come conseguenza dell'analisi modale sappiamo che $y_{omog}(t)$ è combinazione lienare dei modi propri del sistema (dipende dalla matrice $A$), e dunque nel caso di **sistema asintoticamente stabile** questa parte tende ad annullarsi all'infitito:
$$
\lim_{ t \to \infty } y_{omog}(t)=0 
$$
Ciò implica che **per tempi sufficientemente grandi** abbiamo $y(t) \approx  y_{part}(t)$ e cioè una **risposta permanente del sistema**.

La risposta in regime permanente $y_{part}(t)$ **dipende sempre dal particolare ingresso** applicato, inoltre nel caso di sistema asintoticamente stabile, quest'utlima corrisponde con l'uscita del sistema $y(t)$ (per tempi grandi, dato che l'uscita $y_{omog}(t)$ tende a 0).

### Ingresso costante

Nel caso di ingresso costante $u(t)=\overline{u} \cdot \epsilon(t)$ allora anche l'uscita è costante:
$$
y_{part}(t)= \overline{y} \cdot \epsilon(t)=-CA^{-1}B \overline{u} \cdot \epsilon(t)
$$
Il contributo costante si può calcolare tramite il teorema del valore finale:
$$
\overline{y}=\lim_{ t \to \infty } y(t)=\lim_{ s \to 0 } sY(s)=\lim_{ s \to 0 } s \frac{H(s)}{s} \overline{u} = H(0) \overline{u}
$$

### Ingresso sinusoidale

Considerando un ingresso del tipo: $u(t)=\overline{u} \sin(w_{0}t+ \theta_{0})\epsilon(t)$
Allora possiamo esprimere l'uscita come:
$$
\begin{cases}
y_{part}(t)=\overline{y}\sin(w_{0}t+\phi)\epsilon(t) \\
\overline{y}=|H(j\omega_{0})| \cdot \overline{u} \\
@\phi=\arg H(j\omega_{0})+\theta_{0}
\end{cases}
$$


