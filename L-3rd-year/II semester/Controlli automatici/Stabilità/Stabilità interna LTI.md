# Stabilità interna di LTI TC
---
Consideriamo i soliti due movimenti nominali e perturbati e definiamo sempre la perturbazione come la differenza fra il segnale perturbato e quello nominale.

```ad-important
Nei due casi, perturbato e nominale l'ingresso è il medesimo, $u(t)$.
Ciò che cambia è la condizione iniziale nominale e perturbata.
```

Per capire come evolve la perturbazione andiamo a studiare la seguente equazione differenziale:
$$
\dot{\delta x}(t)=\frac{ \partial \delta x(t) }{ \partial t } =\dot{x}(t)-\dot{\widetilde{x}  }(t)=Ax(t)-A \widetilde{x}(t)=A(x(t)-\widetilde{x}(t))=A\delta x(t)    
$$
Con condizione iniziale non nulla: $\delta x(t=0) =\delta x_{0} \neq 0$
La soluzione di questa equazione è data da:
$$
\delta x(t)=e^{At}\delta x_{0} 
$$
Notiamo che **non c'è dipendenza dal movimento nominale $\widetilde{x}(t)$**, qualunque sia considerato.
Perciò possiamo parlare di stabilità di un intero sistema, non di un movimento; ma ciò vale solo per LTI.
Questo risultato qui sopra dipende da una combinazione lineare dei modi propri del sistemi nella forma:
$$
m_{i,\mu_{i}'}(t)=t^{\mu_{i}'-1}e^{\mathbb{Re}(\lambda_{i})t} \cos(\mathbb{Im}(\lambda_{i})t+\phi_{i})
$$
I modi avranno il seguente andamento:
- Esponenzialmente convergenti se $Re(\lambda_{i})<0$
- Limitati se $Re(\lambda)=0$ e $\mu'=1$
- Polinomialmente divergenti se $Re(\lambda)>0$ e $\mu'>1$
- Esponenzialemente divergenti $Re(\lambda)>0$

# Criteri di stabilità per LTI TC
---

![[Pasted image 20230328150909.png]]


# Stabilità interna di LTI TD
---
Studiando le perturbazioni del sistema nel medesimo modo troviamo:
$$
\delta x(k)=A^k\delta x_{0}
$$
Anche in questo caso non c'è dipendenza dal movimento nominale, dunque si può sempre fare riferimento alla stabilità interna dell'intero sistema.
I modi di cui è combinazione lineare l'equazione sono del tipo:
$$
m_{i,\mu_{i}'}(k)=k^{\mu_{i}'-1}|\lambda_{i}^k| \cos(arg(\lambda_{i})k+\phi_{i})
$$
![[Pasted image 20230328153642.png]]


# Criteri di stabilità LTI TD
---

![[Pasted image 20230328153740.png]]


# Esempi
---
Analizzare la proprietà di stabilità interna per gli autovalori $\lambda_{i}(A)$ della matrice di stato A:
$$
\lambda_{i}=\{2,0.4,0.2,0.1\}
$$
Dato che un autovalore è maggiore di 1 abbiamo instabilità.
