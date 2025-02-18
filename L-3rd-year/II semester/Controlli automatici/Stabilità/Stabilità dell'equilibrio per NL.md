# Stabilità dell'equilibrio di sistemi NL TC
---
Consideriamo un sistema MIMO, non lineare, stazionario:
Definiamo un **movimento nominale d'equilibrio** $\widetilde{x}(t)=\overline{x}$ ottenuto applicando un ingresso nominale d'equilibrio $\widetilde{u}(t)=\overline{u}$ al sistema in un stato inziale nominale $\widetilde{x}(t=0)=\overline{x}$.
Allora stesso modo definiamo il movimento perturbato $x(t)$ ottenuto appliacando lo stesso ingresso nominale d'equilibrio a uno stato iniziale differente $x_{0} \neq  \overline{x}$
Definiamo la perturbazione e studiamo la derivata.

Data la non linearità usiamo **il metodo indiretto di Lyapunov / metodo di linearizzazione**  per studiare la stabilità locale dell'equilibrio senza risolvere l'eq. differenziale.
Applicando la linearizzazione con Taylor troncato al primo ordine studiamo:
$$
\dot{\delta x}(t)=A \delta x(t), \quad \quad \delta x(t=0)=x_{0}- \overline{x}= \delta x_{0}
$$
Dove la matrice A è pari a:
$$
A=\frac{ \partial f(x, \overline{u}) }{ \partial x }|_{x= \overline{x}} \implies \textrm{Jacobiano di f rispetto a x} \quad   

$$

# Criteri di stabilità dell'equilibrio per sistemi NL TC
---
Considerando l'approssimazione della matrice A, allora possiamo definire una condizione **sufficiente affinchè il sistema sia (localmente) asintoticamente stabile**:
$$
\forall i: Re(\lambda_{i}(A)) <0
$$
Mentre una **condizione sufficiente per l'instabilità**:
$$
\exists i: Re(\lambda_{i}(A))>0
$$
Nel caso di autovalori pari a 0 non possiamo concludere nulla, e il sistema può risultare stabile o instabile.

In breve...

![[Pasted image 20230331181410.png]]


# Caso a tempo TD
---
Non cambia assolutamente nulla, quindi cerchiamo di risolvere le seguenti equazioni:
$$
\delta x(k+1)=A \delta x, \quad \quad \delta x(k_{0}=0)=\delta x_{0}
$$
$$
A= \frac{ \partial f(x, \overline{u}) }{ \partial x } |_{x= \overline{x}} \implies \textrm{Jacobiano di f rispetto a x} \quad  
$$

Mentre i criteri di valutazione sono:

![[Pasted image 20230331181500.png]]