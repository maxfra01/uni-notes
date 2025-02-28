# Elementi fondamentali
---

### Resistore, condensatore e induttori

Per un **resistore ideale** abbiamo l'equazione costitutiva $$
v_{R}(t)=Ri_{R}(t)
$$mentre nel dominio di Laplace abbiamo:$$
v_{R}(s)=Ri_{R}(s)
$$Notiamo che l'equazione costitutiva è di tipo statico!

Per un **condensatore ideale** abbiamo $$
i_{c}(t)=C\frac{ \partial v_{c}(t) }{ \partial t } 
$$mentre nel dominio di Laplace:$$
I_{c}(s)=sCV_{C}(s)-Cv_{C}(t=0_{\_})
$$
```ad-note
Consideriamo la trasformata di Laplace monolatera a partire da 0-
```

Per un **induttore ideale** di induttanza $L$ abbiamo:$$
v_{L}(t)=L\frac{ \partial i_{L}(t) }{ \partial t } 
$$mentre nel dominio di Laplace:$$
V_{L}(s)=sLI_{L}(s)-Li_{L}(t=0\_)
$$
# Rappresentazione in variabili di stato
---
Per applicare il metodo si seguono i seguenti passi:
1. Si scrivono le **equazioni costitutive** per i componenti con memoria
2. Si scrivono le **equazioni topologiche** con qualsiasi metodo di analisi di circuiti
3. Si introduce una **variabile di stato $x_{i}$** per ogni componente con memorie, scegliendo le correnti negli induttori e la tensione ai capi dei condensatori
4. Si associa una **variabile di ingresso $u_{j}$** ad ogni generatore di corrente o tensione

```ad-warning
Il punto 3 non si applica a reti degeneri, come maglie di condensatori con generatori di tensione, oppure tagli di induttori con generatori di corrente.
```

A questo punto è possibile ricavare le **equazioni di stato** che sono del tipo:
$$
\dot{x}_{i}(t)=\frac{ \partial x_{i}(t) }{ \partial t }=f_{i}(t,x(t),u(t)) 
$$
E le **equazioni di uscita** nella forma: $$
y_{k}(t)=g_{k}(t,x(t),u(t))
$$
# Esempio
---

![[Pasted image 20230228155438.png]]

Si scrivono le equazioni costitutive:
$v_{L}(t)=L \frac{ \partial i_{L}(t) }{ \partial t }$
$i_{C}(t)=C\frac{ \partial v_{C}(t) }{ \partial t }=C \frac{ \partial v_{2}(t) }{ \partial t }$
$v(t)=v_{1}(t)+v_{L}(t)+v_{2}(t)$, equazione alla maglia
$i_{L}(t)+i(t)=i_{2}+i_{C}$, equazione al nodo

Riconosciamo le variabili di stato come la corrente nell'induttore e la tensione ai capi del condensatore:$$
x(t)=\begin{bmatrix}
i_{L}(t) \\
v_{2}(t)
\end{bmatrix}=\begin{bmatrix}
x_{1}(t) \\
x_{2}(t)
\end{bmatrix}
$$Mentre le variabili di ingresso sono date dalla coppia:$$
u(t)=\begin{bmatrix}
v(t) \\
i(t)
\end{bmatrix}=\begin{bmatrix}
u_{1}(t) \\
u_{2}(t)
\end{bmatrix}
$$Ora occorre ricavare la vera rappresentazione di stato, quindi dobbiamo risolvere le equazioni assicurandoci che ogni rappresentazione di stato sia composta solo **da variabili di ingresso e variabili di stato**:
$$\dot{x}_{1}=\frac{di_{L}(t)}{dt}=\frac{v_{L}}{L}=\frac{v-v_{1}-v_{2}}{L}=\frac{v-R_{1}i_{L}-v_{2}}{L}=-\frac{R_{1}}{L}x_{1}+\frac{u_{1}}{L}-\frac{v_{2}}{L}$$
Similmente ricavo che:
$$
\dot{x}_{2}=\frac{1}{C}x_{1}-\frac{1}{R_{2}C}x_{2}+\frac{1}{C}u_{2}
$$
Ora devo ricavare le equazioni di uscita, finchè le uscite non dipendono solo da stato e ingressi:$$y_{1}=v_{1}=R_{1}i_{L}=R_{1}x_{1}$$e in maniera simile abbiamo:$$
y_{2}=v_{2}=x_{2}
$$Ottenendo così la rappresentazione in variabile di stato:$$
y(t)=\begin{bmatrix}
R_{1}x_{1} \\
x_{2}
\end{bmatrix}
$$
Possiamo classificare il sistema: è **dinamico**, a **tempo continuo**, **MIMO** (2 ingressi), dimensione pari agli stati, ovvero 2, quindi a **dimensione finita**.
Per sapere se è tempo invariante e lineare, devo sapere se C,L,R dipendono dal tempo, perciò finchè non lo so non posso dire nulla.
Se **assumo che R,L,C siano costanti** allora il sistema è **LTI**.
Non è presente un legame ingresso uscita istantaneo, quindi è **fisicamente realizzabile**.
Essendo LTI posso rapprensetare in maniera compatta la rappresentazione:
$$
\dot{x}(t)=Ax(t)+Bu(t) \quad y(t)=Cx(t)+Du(t)
$$Con $$
A=\begin{bmatrix}
-\frac{R_{1}}{L} & -\frac{1}{L} \\
\frac{1}{C} & -\frac{1}{R_{2}C} \\
\end{bmatrix} \space , B=\begin{bmatrix}
\frac{1}{L} & 0 \\
0 & \frac{1}{C} 
\end{bmatrix} \space , C=\begin{bmatrix}
R_{1} & 0 \\
0 & 1
\end{bmatrix} \space , D=\begin{bmatrix}
0 & 0 \\
0 & 0
\end{bmatrix}
$$
```ad-note
In sistemi fisicamente realizzabili possiamo sempre dire che la matrice D è sempre una matrice nulla!
```
