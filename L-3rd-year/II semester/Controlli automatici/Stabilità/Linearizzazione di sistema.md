# Linearizzazione di sistemi dinamici
---
Spesso preferiamo applicare proprietà e lavorare con sistemi lineari, quindi occorre modellare un sistema "approssimandolo" a lineare.

### Linearizzazione di funzione reale

Considerando una funzione $f(x): \mathbb{R} \to \mathbb{R}$, essa può essere sviluppata in serie di **Taylor** in un intorno di ampiezza $\delta x=x-x_{0}$ centrato in $x_{0}$ secondo la formula:
$$
f(x)=f(x_{0}+\delta x)=f(x_{0})+ \frac{ \partial f(x) }{ \partial x } |_{x=x_{0}}
$$
Consideriamo un troncamento della funzione come sviluppo di Taylor arrestato al primo ordine, detto $h(x)$.
Quest'ultima funzione è tanto migliore quanto è piccolo l'intorno $\delta x$ vicino a $x_{0}$.

### Linearizzazione di un sistema

E' possibili servirsi di **modelli linearizzati**, ovvero approssimazioni in un intorno prefissato (punto di equilibrio).

Dato un sistema dinamico, MIMO, a tempo continuo, **non lineare**, se ne considerino due diverse evoluzioni temporali:
- Un primo movimento **nominale** $\widetilde{x}(t)$ ottenuto mettendo in ingresso un ingresso nominale $\widetilde{u}(t)$, in uno stato nominale $\widetilde{x}_{0}$, a cui corrisponde un uscita nominale $\widetilde{y}(t)$.
- Un movimento **perturbato** $x(t)$, dato dall'applicazione di un **ingresso differente** (perturbato) $u(t)$, in uno stato iniziale perturbato $x_{0}$, a cui corrisponde un'uscita perturbata $y(t)$.

Le differenze fra i due movimenti vanno a creare le **perturbazioni del sistema**, e si parla di:
- Perturbazioni sullo **stato**, $\delta x(t)=x(t)-\widetilde{x}(t)$
- Perturbazioni sull'ingresso, $\delta u(t)=u(t)-\widetilde{u}(t)$
- Perturbazioni sull'uscita, $\delta y(t)=y(t)- \widetilde{y}(t)$
A questo punto ci chiediamo come calcolare $x(t)$ per ogni istante tempporale, e per fare ciò studiamo la perturbazione e come essa varia nel tempo:
$$
\dot{\delta x}(t)=\frac{ \partial \delta x(t) }{ \partial t } =\frac{ \partial (x(t)-\widetilde{x}(t)  ) }{ \partial t }=\dot{x}(t)- \dot{\widetilde{x}  }(t)   
$$
Possiamo andare a sviluppare secondo membro con Taylor:
$$
\dot{\delta x}(t)=f(x(t),u(t))- f(\widetilde{x}(t),\widetilde{u}(t)    )
$$
Dove il termine $f(x(t),u(t))$ è approssimabile come:
$$
f(x(t),u(t))= f(\widetilde{x}(t),\widetilde{u}(t)    )+ \frac{ \partial f(x,u) }{ \partial x } |_{x=\widetilde{x},u=\widetilde{u}    }\space \delta x(t) +\frac{ \partial f(x,u) }{ \partial u } |_{x=\widetilde{x},u=\widetilde{u}    }\space \delta u(t)  
$$
Andando a sostituire questo termine nell'equazione differenziale abbiamo riscritto l'equazione, poi possiamo fare l'analogo con l'uscita per ottenere le seguenti equazioni risolutive:
$$
\begin{cases}
\dot{\delta x}(t)=A(t)\delta x(t)+B(t)\delta u(t) \\
\dot{\delta y}(t)=C(t) \delta x(t)+D(t)\delta u(t)
\end{cases}
$$
Dove le matrici dipendendi dal tempo sono nella forma:
$$
A(t)=\frac{ \partial f(x,u) }{ \partial x } |_{x=\widetilde{x},u=\widetilde{u}    }=\begin{bmatrix}
\frac{ \partial f_{1} }{ \partial x_{1} }  & \dots & \frac{ \partial f_{1} }{ \partial x_{n} } \\
\vdots &  &  \vdots \\
\frac{ \partial f_{n} }{ \partial x_{1} }  & \dots & \frac{ \partial f_{n} }{ \partial x_{n} } 
\end{bmatrix}
$$
Questa matrice è una jacobiana, in cui ogni riga è il gradiente di una singola componente della funzione$f(x,u)$.

In sintesi, l'evoluzione temporale può essere espressa in funzione delle perturbazioni secondo:

![[Pasted image 20230321152129.png]]

Per il caso **a tempo discreto TD** le equazioni risultano essere:

![[Pasted image 20230321152659.png]]

```ad-note
Nel caso si scegliesse come movimento nominale un **punto di equilibrio**, allora le matrici non dipenderebbe dal tempo.
In ogni caso, qualunque movimento nominale si scelga esso è tanto migliore quanto le perturbazioni sono piccoli rispetto a quel movimento.
```

### Esempio di calcolo 

![[Pasted image 20230321153214.png]]

Dobbiamo calcolare le matrici A,B,C,D e non dipendono dal tempo in quanto ci siamo posti nel punto di equilibrio:
$$
A=\frac{ \partial f(x,u) }{ \partial x } |_{x=\widetilde{x},u=\widetilde{u}    }=\begin{bmatrix}
\frac{ \partial f_{1} }{ \partial x_{1} } & \frac{ \partial f_{1} }{ \partial 2 } \\
\frac{ \partial f_{2} }{ \partial x_{1} } & \frac{ \partial f_{2} }{ \partial x_{2} }   
\end{bmatrix}=\begin{bmatrix}
0 & 1 \\
\frac{2k_{i}\overline{u}}{M \overline{x}_{1}^3} & 0
\end{bmatrix}
$$
$$
B=\frac{ \partial f(x,u) }{ \partial u } =\begin{bmatrix}
0  \\
\frac{-2k_{i}\overline{u}}{M\overline{x}_{1}^2}
\end{bmatrix}
$$
$$
C=\frac{ \partial g(x,u) }{ \partial x } =\begin{bmatrix}
1 & 0
\end{bmatrix}
$$
$$
D= \frac{ \partial g(x,u) }{ \partial u } =0
$$
