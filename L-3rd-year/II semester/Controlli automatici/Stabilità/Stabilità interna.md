# Stabilità interna 
---
Vogliamo valutare quanto il sistema sia vulnerabile a perturbazioni su stato, ingressi e sui parametri in gioco nelle varie equazioni descrittive.
In particolare quando si parla di **stabilità interna** facciamo riferimento alle sole variazioni sullo stato inziale, assumendo costanti i parametri e gli ingressi.

Un sistema di definisce **stabile** se e solo se è poco sensibile a perturbazioni sullo stato inziale, in caso contrario è detto **instabile**.

# Stabilità per sistemi TC
---
Consideriamo un sistema dinamico, a dimensione finita, MIMO, a TC, non lineare e stazionario, allora consideriamo due possibili scenari:
- Un **movimento nominale $\widetilde{x}(t)$**
- Un **movimento perturbato $x(t)$**
Definiamo il concetto di **perturbazione** come la differenza fra questi due movimenti $\delta x(t)=x(t)-\widetilde{x}(t)$ e perciò:
$$
x(t)=\widetilde{x}(t) +\delta x(t)  
$$
Ora, applicando una perturbazione sull'ingresso nominale diversa da zero, possiamo definire i seguenti scenari per il movimento nominale $\widetilde{x}(t)$:
- è **stabile** solamente se $\delta x(t)$ è limitata in ampiezza.
- è **instabile** se $\delta x(t)$ non resta limitato nel tempo.
- è **asintoticamente stabile** se $\delta x(t)$ si mantiene stabile nel tempo e inoltre al limite per $t \to \infty$ la perturbazione si annulla
- è **semplicemente stabile** se si mantiene limitato nel tempo ma al limite esso non va a zero.
- è **globalmente asintoticamente stabile** se per **qualsiasi** perturbazione imposta essa si mantiene limitata e converge a 0 nel tempo.

### Movimento stabile

Matematicamente parlando definiamo il concetto di **stabilità**:
$$
||\delta x(t)||=||x(t)-\widetilde{x}(t)  || < \epsilon \quad \forall t\geq 0
$$
Ovvero per ogni quantità $\epsilon >0$, esiste un $\gamma>0$ tale per cui, per ogni stato si verifichi la relazione indicata qui sopra.

![[Pasted image 20230327133431.png]]

### Movimento instabile

Per il concettò di **instabilità** abbiamo che:
$$
\exists t\geq 0: ||\delta x(t)||=||x(t)-\widetilde{x}(t)  ||>\epsilon
$$

![[Pasted image 20230327133824.png]]

### Movimento asintoticamente stabile

Devono essere verifiche le due seguenti condizioni:
$$
\begin{cases}
||\delta x(t)||=||x(t)-\widetilde{x}(t)  || \leq \epsilon \quad \forall t\geq 0 \\
\lim_{ t \to \infty } ||x(t)-\widetilde{x}(t)  ||=0
\end{cases}
$$

![[Pasted image 20230327134126.png]]

### Movimento globalmente stabile

L'unica differenza con l'asintoticamente stabile è che la condizione vale per ogni stato considerabile:
$$
\begin{cases}
||\delta x(t)||=||x(t)-\widetilde{x}(t)  || \leq \epsilon \quad \forall t\geq 0 \\
\lim_{ t \to \infty } ||x(t)-\widetilde{x}(t)  ||=0 \quad \forall x_{0} \in X
\end{cases}
$$

```ad-note
I concetti di stabilità per sistemi a tempo discreto non variano da quelli a tempo continuo, l'unica differenza sta nel tempo che evolve per intervalli di tempo discreti.
```


# Stabilità dell'equilibrio
---
Si parla di **stabilità dell'equilibrio** quando un certo movimento nominale corrisponde a uno stato di equilibrio, legato a un certo ingresso di equilibrio. Si parla dunque di **stabilità locale**, intorno a un certo punto di funzionamento.

Ad ogni stato di equilibrio asintoticamente stabile è associata una **regione di attrazione (regione di asintotica stabilità)**, costituita da quegli stati  
iniziali che danno origine a movimenti perturbati convergenti asintoticamente allo stato d’equilibrio.

![[Pasted image 20230327140721.png]]
