i
# Principi fisici di funzionamento
---
I sistemi **elettromeccanici** operano una conversione di energia:
- Conversione di energia elettrica in moto, ovvero i **motori elettrici**.
- Conversione dell'energia meccanica in energia elettrica, ovvero i **generatori o dinamo elettriche**

### Forza di Lorentz

Un conduttore elettrico di lunghezza $l$ percorso da corrente $i(t)$ e immerso in un campo magnetico $B(t)$ abbiamo:
$$
F(t)=l i(t) \wedge B(t)
$$
Ora se immaginiamo di prendere 4 conduttori lineare e disporsi come in un rettangolo otteniamo una spira:

### Coppia di Lorentz

Una spira conduttrice di area $A$ immersa in un campo magnetico $B(t)$ e percorsa da corrente $i(t)$ allora essa è sottoposta a una **coppia di Lorentz**:
$$
T(t)=i(t)AB(t)\sin(\theta(t))
$$
Il principio di un motore elettrico è quello di avere tante spire in un motore, ed alimentare solamente quelle con cui ottengo la coppia massima.

### Legge dell'induzione elettromagnetica

Considere un conduttore che forma un circuito chiuso e concatena un flusso di un campo magnetico $\Phi(t)$, allora ho quello che è detta **f.e.m. indotta**:
$$
e(t)=-\frac{ \partial \Phi(t) }{ \partial t } 
$$

# Motore DC
---
Un motore elettrico è sempre costituito da due parti, rotore e statore.

![[Pasted image 20230310105101.png]]

Lo **statore** è la parte inamovibile, esterna e non rotante, responsabile della generazione del campo magnetico.
Esso può essere costituito da:
- Magneti permanenti semplici (pesanti, ingombranti, non regolabili)
- Serie di avvolgimenti alimentati a corrente continua, detti **circuito di eccetazione** (soluzione più comoda ed efficiente)

Il **rotore** è la parte interna e mobile costituito da un cilindro di materiale ferromagnetico su cui sono posizionati gli avvolgimenti che formano il **circuito di armatura**.
Questi avvolgimenti sono quelli sui avviene la selezione di alimentazione.

Il **collettore a spazzole** è un interruttore rotante che permette di alimentare solo alcuni avvolgimenti del motore: tramite le spazzole la corrente entra nelle maglie, e abbiamo continuità omhica solo se chiudiamo il circuito, andando ad agire sul selettore (collettore).

Infine l'**albero** trasmette il movimento e dato il suo momento di inerzia è di solito stabilizzato da **cuscinetti a sfera**.

### Modello matematico del motore DC

![[Pasted image 20230310110407.png]]

Il circuito di armatura è descritto dall'equazione:
$$
v_{a}=R_{a}i_{a}+L_{a}\frac{ \partial i_{a}(t) }{ \partial t } +e(t)
$$
Mentre quello di eccetazione è descritto da:
$$
v_{e}=R_{e}i_{e} +L_{e} \frac{ \partial i_{e}(t) }{ \partial t }
$$
Per la parte meccanica l'equazione del moto è:
$$
J\ddot{\theta}(t)=T_{m}(t)-T_{r}(t)-\beta(\dot{\theta})
$$
IL fenomeno della conversione viene descritto da:
$$
e(t)=K \Phi(t)\omega(t) \quad \quad T_{m}(t)=K \Phi(t)i_{a}(t)
$$
Il flusso generato può seguire due comportamenti:
- se uno dei magneti allora il flusso sarà costante
- se invece uno delle spire, il flusso sarà una funzione non lineare di $i_{e}(t)$ dovuto al circuito di eccetazione
![[Pasted image 20230310112146.png]]

Tuttavia, si usa un valore di corrente che si aggira attorno a un **punto di  funzionamento**, allora cerco di linearizzare il flusso intorno a quell'intorno.

### Motore DC con comando di armatura

In questa situazione il flusso magnetico è mantenuto **costante**, mentre il comando è rappresentato dalla tensione $v_{a}$
Il flusso magnetico è quindi costante, dunque anche la corrente sarà costante, dunque la caratteristica della maglia di eccitazione sarà:
$$
v_{e}(t)=R_{e}i_{e}+0=R_{e}i_{e} \quad \forall t 
$$
Dunque le equazioni alla maglia di armature sono:
$$
v_{a}(t)=R_{a}i_{a}(t)+L_{a}\frac{ \partial i_{a} }{ \partial t }+ K\Phi \omega(t) 
$$
Mentre l'equazione del moto è pari a:
$$
J \ddot{\theta}(t)=J \dot{\omega}(t)=K\Phi i_{a}(t)-T_{r}(t)-\beta \omega(t)
$$
Sceglieremo come varibiali di stato quelle di cui compare la derivata, quindi $i_{a}, \theta, \omega$.
Definiamo dunque ingressi e variabili di stato:
$$
x(t)=\begin{bmatrix}
i_{a}(t) \\
\theta(t) \\
\omega(t)
\end{bmatrix} \quad \quad u(t)=\begin{bmatrix}
v_{a}(t) \\
T_{r}(t)
\end{bmatrix}
$$
Ora è possibile ottenere la rappresentazione in variabile di stato (vedi slide).


### Motore DC con comando di eccitazione

Qui la corrente di armatura $i_{a}(t)$ nel rotore è mantenuto **costante**, mentre il comando è la tensione $v_{e}$ della maglia di eccitazione.
A questo punto per la maglia di armatura:
$$
v_{a}(t)=R_{a}i_{a}+L_{a}\frac{ \partial i_{a} }{ \partial t } +K\Phi(t) \omega(t)=R_{a}i_{a}+K\Phi(t)\omega(t)
$$
Abbiamo così ottenuto un equazione di tipo statico.
Le equazioni dinamiche sono allora:
$$
v_{e}(t)=R_{e}i_{e}(t)+L_{e}\frac{ \partial i_{e}(t) }{ \partial t } 
$$
$$
J \ddot{\theta}(t)=J\dot{\omega}(t)=K\Phi(t)i_{a}-T_{r}(t)-\beta \omega(t)$$
L'espressione di $\Phi(t)$ non è lineare, perciò tendiamo ad approssimare la caratteristica non lineare:
$$
\Phi(t)=i_{e}(t)K_{e}
$$

![[Pasted image 20230313133119.png]]

Andando a sostituire l'approssimazione di $\Phi(t)$ nell'equazione del moto, e inoltre chiamando $K^*=KK_{e}i_{a}$ abbiamo che l'equazione del moto ottenuta è:
$$
J \ddot{\theta}(t)=J\dot{\omega}(t)=K^* i_{e}(t)-T_{r}(t)-\beta \omega(t)
$$
Ora specificando le variabili di stato e gli ingresso possiamo ricavarne una rappresentazione in variabile di stato.

# Esempio
---

![[Pasted image 20230313133904.png]]

Caratterizziamo la maglia di armatura e gli organi in rotazione:
1. $v_{a}=R_{a}i_{a} + L_{a}\frac{ \partial i_{a} }{ \partial t }+K\Phi \omega_{1}(t)$
2. $J_{1}\ddot{\theta_{1}}(t)=T_{m}-\beta_{1}\omega_{1}(t)-\beta_{12}(\omega_{1}-\omega_{2})- K_{12}(\theta_{1}-\theta_{2})$
3. $J_{2}\ddot{\theta_{2}}=-T_{d}-K_{12}(\theta_{2}-\theta_{1})-\beta_{12}(\omega_{2}-\omega_{1})$
Notando che $T_{m}=K\Phi i_{a}$

Ora scelgo le variabili di stato e indico gli ingressi:
$$
x(t)=\begin{bmatrix}
i_{a}(t) \\
\theta_{1}(t) \\
\theta_{2}(t) \\
\omega_{1}(t) \\
\omega_{2}(t)
\end{bmatrix}=\begin{bmatrix}
x_{1}(t) \\
x_{2}(t) \\
x_{3}(t) \\
x_{4}(t) \\
x_{5}(t)
\end{bmatrix} \quad \quad u(t)=\begin{bmatrix}
v_{a}(t) \\
T_{d}(t)
\end{bmatrix}=\begin{bmatrix}
u_{1}(t) \\
u_{2}(t)
\end{bmatrix}
$$
Se siamo interessati alla posizioni angolare in uscita avrò $y(t)=x_{3}(t)$
Dunque vado a scrivere le equazioni di stato:

![[Pasted image 20230313135115.png]]

Classifico il sistema come dinamico, a dimensione finita (5), MIMO, se suppongo che tutti i coefficienti siano fissi nel tempo ho che è LTI, è fisicamente realizzabile.