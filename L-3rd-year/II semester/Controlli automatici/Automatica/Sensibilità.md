# Sensibilità parametrica
---
L'obiettivo è quello di valutare l'influenza di variazioni dei parametri sulla risposta al sistema controllato.
Per la robustezza della risposta avevamo introdotto i margini.
Qui invece valutiamo la **fedeltà della risposta**.

Le funzione di sensibilità formalizzano **la reattività del sistema alle variazioni** come il rapporto fra le variazioni relative della grandezze e del parametro stesso.

La **funzione di sensibilità** della fdt in catena chiusa rispetto a un parametro $p$ è:
$$
S_{p}^{W_{y}}=\frac{ \partial W_{y}(s) }{ \partial p } \cdot \frac{p}{W_{y}(s)} 
$$
A seconda dei valori assunti dalla funzione possiamo vedere quanto il sistema è sensibile alle variazioni di $p$:

| Valori di $S_{p}$ | Conseguenza  |
| ----------------- | ----------------------------------- |
| $\ll 1$           | Sistema poco alle variazioni di $p$ |
| $>1$ o $\gg 1$    | Sistema pesantemente sensibile alla variazioni di $p$ |

Per un generico sistema, descritto dal solito schema a blocchi possiamo trovare l'espressione della funzione di sensibilità rispetto alla funzione ad anello:

![[Pasted image 20230526101032.png]]

Da questa espressione posso far risaltare come la sensibilità della catena chiusa rispetto alla funzione ad nello indica come le variazioni di $G_{a}$ vadano ad impattare sulla catena chiusa.
Queste variazioni possono riguardare:
- Il controllore, per contenere la sensibilità andrò ad agire sulla qualità realizzativa
- la $F(s)$, allora in quel caso non posso fare nulla

Posso calcolare la **sensibilità** della catena chiusa rispetto all'anello come:
$$
S(s)=\frac{1}{1+G_{a}(s)}
$$

# Andamento di $S(s)$
---
Per inseguire correttamente i riferimenti costanti e annullare i disturbi costanti allora $G_{a}$ ha almeno un polo nell'origine:
$$
G_{a}(j\omega) \to \infty \implies S(j\omega) \to 0
$$
In bassa frequenza per $\omega \ll \omega_{c}$ averò:
$$
|G_{a}(j\omega)|\gg 1 \implies |S(j\omega)|=\frac{1}{|G_{a}(j\omega)|}
$$
ecc...

Sulla base di queste considerazioni possiamo tracciare i DdB della funzione di sensibilità $S(s)$:

![[Pasted image 20230526102506.png]]


# Implicazioni sul progetto
---
La più semplice specifica richiede che la sensibilità sia strettamente minore di 1 per limitare le influenze dei parametri:
$$
|S(j\omega)|\ll 1 \quad \forall \omega<\omega_{M}
$$
Per soddisfare questo tipo di specifica ci si riconduce a un requisito di banda passante minima.
Il vincolo che si ottiene è che la pulsazione di crossover desiderata sia pari a:
$$
\omega_{c,des} = 1.5 \cdot \omega_{M}
$$


