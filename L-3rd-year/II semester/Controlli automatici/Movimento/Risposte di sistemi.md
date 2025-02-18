# Introduzione
---
Lo studio della risposta al gradino di un sistema dinamico è importante per due motivi:
- Permette di studiare il sistema da una situazione di equilibrio a un'altra.
- In alcuni casi è possibile ricavare la funzione di trasferimento del sistema.

Faremo riferimento solamente ai **sistemi a TC** dove la funzione di trasferimento è nella forma:
$$
H(s)=\frac{N_{H}(s)}{D_{H}(s)}
$$
Ricordando che il denominatore e numeratore non hanno radici comuni.

I sistemi **del primo ordine** hanno $D_{H}$ di primo grado, mentre se quest'ultimo è un polinomio di secondo grado allora si parla di **sistemi del secondo ordine**.

# Risposte del 1° ordine
---
Tratteremo come ingressi i casi del gradino e dell'impulso unitario:
$$
u(t)=\overline{u}\delta(t) \to^{\mathcal{L}} U(s)=\overline{u}
$$
$$
u(t)=\overline{u}\epsilon(t) \to^{\mathcal{L}} U(s)=\frac{\overline{u}}{s}
$$
Per conoscere la risposta del sistema è semplicemente necessario antitrasformare l'espressione dell'uscita nel dominio di Laplace:
$$
y(t)=\mathcal{L}^{-1}[Y(s)]=\mathcal{L}^{-1}[H(s) \cdot U(s)]
$$

### Teorema del valore iniziale
$$
\lim_{ t \to 0_{+} } = \lim_{ s \to \infty } sY(s)
$$
Esso vale solamente quando entrambi i limiti esistono e sono finiti ($<\infty$) e ciò significa che il grado del numeratore deve essere più piccolo del grado del numeratore.

### Teorema del valore finale
$$
\lim_{ t \to \infty } y(t)= \lim_{ s \to 0 } sY(s)
$$
Esso vale solo quando entrambi i limiti esistono e sono finiti, ciò significa che **$sY(s)$ deve avere tutti i suoi poli nel semipiano sinistro**.

### Stabilità esterna

Di fatto equivale alle definizione di BIBO, e ciò significa che per ogni ingresso $u(t)$ limitato si ha:
$$
|u(t)|<\overline{u}<\infty
$$
Allora la corrispondente uscita sarà limitata:
$$
|y(t)|< \overline{y}(t)<\infty
$$
Per capire se un sistema LTI è BIBO è sufficiente osservare se i **poli della f.d.t. ha tutti i poli con $\mathcal{Re(p)<0}$**.

### Ingresso a impulso

Consideriamo una funzione di trasferimento come:
$$
H(s)=\frac{K^*}{s-p}
$$
Dove il termine $K^*$ è il guadagno, mentre $p$ rappresenta il polo della f.d.t.
Per un **ingresso a impulso unitario** avremo:
$$
Y(s)=H(s)U(s)=K^*\overline{u}e^{pt}\epsilon(t)
$$
A questo funzione possiamo applicare il teorema del valore finale ed inziale:
$$
\lim_{ t \to 0 } y(t)=\lim_{ s \to \infty } sY(s)=s \cdot \frac{K^*\overline{u}}{s-p}=K^*\overline{u}
$$
Mentre per il valore finale:
$$
\lim_{ t \to \infty } y(t)=\lim_{ s \to 0 } sY(s)
$$
Possiamo dunque differenziando i valori del polo, tracciare gli andamenti della funzione $y(t)$:

![[Pasted image 20230322090745.png]]

### Ingresso a gradino unitario

$$
u(t)=\overline{u}\epsilon(t) \implies U(s)=\frac{\overline{u}}{s}
$$
$$
Y(s)=H(s)U(s)=\frac{K^*}{s-p} \cdot \frac{\overline{u}}{s}
$$
Nell'ipotesi di $p \neq 0$ allora possiamo riscrivere l'uscita con i fratti semplici:
$$
Y(s)=\frac{K^*}{-p} \overline{u}[1-e^{pt}]\epsilon(t)
$$
![[Pasted image 20230322091449.png]]

Quando il polo è strettamente **negativo** allora:
- Definiamo la **costante di tempo $\tau$** come:$$
\tau=|\frac{1}{p}|
$$
- Definiamo inoltre il valore finale $$
y_{\infty}=\frac{K^*}{-p}\overline{u}=\overline{K}u\quad ,\quad K=\frac{K^*}{-p}
$$
### Funzione di trasferimento

Quella presa in considerazione negli esempi sopra è la forma **zero-polo**, che evidenzia guadagno e il polo.
Esistono però altre forme di rappresentazione come quella di **Bode**:
$$
H(s)=\frac{K}{1+\tau s}
$$
### Parametri temporali

Il primo parametro che consideriamo è il **tempo di salita 10-90** ovvero il tempo che impiega l'uscita a passare dal 10 al 90% del valore a regime $y_{\infty}$: viene indicato con $t_{r}$.

Il **tempo di assestamente** è il tempo impiegato dall'uscita per assestarsi un intervallo intorno al $y_{\infty}$ e che non si discosti del $\epsilon \%$ da quest'ultimo.
Si indica con $t_{a,\epsilon\%}$

![[Pasted image 20230322093408.png]]

Inoltre notiamo che dopo una costante di tempo l'uscita ha raggiunto il **63%** del valore a regime.


# Risposte del 2° ordine
---
Consideriamo dei sistemi del secondo ordine con f.d.t.:
$$
H(s)=K \frac{\omega_{n}^2}{s^2+2\zeta \omega_{n}s+\omega_{n}^2}
$$
Affinchè si considerino solo sistemi BIBO stabili vogliamo che lo **smorzamento $\zeta$** sia una quantità compresa tra 0 e 1 strettamente positiva.
$\omega_{n}$ rappresenta la **pulsazione naturale**, $K$ il guadagno.
Notiamo che i poli devono avere necessariamente parte immaginaria negativa, quindi il sistema ammette $\tau$.
Posso calcolare la costante di tempo del sistema:
$$
\tau=\frac{1}{\zeta \omega_{n}}
$$
### Risposta al gradino unitario

Per un ingresso **a gradino unitario**, passando nel dominio di Laplace e antitrasfromando otteniamo:
$$
y(t)=\overline{u} K\left( 1- \frac{1}{\sqrt{ 1- \zeta^2 }}e^{-\zeta \omega_{n}t}\sin(\omega_{n}\sqrt{ 1-\zeta^2t }+\arccos(\zeta)) \right) \quad \forall t\geq 0
$$
Possiamo andare a definire il **valore di picco** di questa quantità, definito come:
$$
y_{max}=max_{t}(y(t))
$$
Per il teorema del valore finale possiamo calcolare anche $y_{\infty}$

![[Pasted image 20230324102334.png]]

Definiamo la **sovraelongazione massima $\hat{s}$** come il rapporto fra il massimo scostamento in ampiezza rispetto al valore a regime, e il valore di regime, dunque:
$$
\hat{s}=\frac{y_{max}-y_{\infty}}{y_{\infty}}
$$
Può eventualmente essere espressa in percentuale $\hat{s}_{\%}$
Il **Tempo di picco $\hat{t}$** è il tempo che corrisponde al valore di picco e quindi l'istante di tempo per cui vale:
$$
y(\hat{t})=y_{max}
$$
Si può dimostrare che $\hat{s}$ dipende solamente dallo smorzamento $\zeta$:
$$
\hat{s}=e^{\frac{-\pi\zeta}{\sqrt{ 1-\zeta^2 }}}=\frac{|\ln(\hat{s})|}{\sqrt{ \pi^2+\ln^2(\hat{s}) }}
$$
Mentre il tempo di picco $\hat{t}$ dipende sia da smorzamento che pulsazione:
$$
\hat{t}=\frac{\pi}{\omega_{n}\sqrt{ 1-\zeta^2 }}
$$
Altri parametri di interesse sono **Tempo di salita $t_{s}$ e tempo di salita 10-90 $t_{r}$**: nel primo caso si parla del primo istante temporale in cui si incontra $y_{\infty}$ mentre il secondo è il tempo già definito a seconda del 10% e 90% del valore di regime.
$$
t_{s}=\frac{1}{\omega_{n}\sqrt{ 1-\zeta^2 }}(\pi-\arccos(\zeta)) \quad \quad t_{r}=\frac{2.16\zeta+0.6}{\omega_{n}}
$$
Per il **tempo di assestamento $t_{a,\epsilon\%}$** si dimostra la seguente relazione:
$$
t_{a,\epsilon\%} \approx - \frac{\ln(0.01\epsilon)}{\zeta \omega_{n}}
$$


### Esempio

Consideriamo un sistema con 2 poli reali e distinti, con f.d.t. del tipo:
$$
H(s)=\frac{K^*}{(s-p_{1})(s-p_{2})}
$$
E supponiamo che $p_{1},p_{2} \neq 0$
Per questi sistemi ci limitiamo al caso di ingresso a gradino unitario.
L'uscita si calcola nel solito modo e otteniamo:
$$
Y(s)=\frac{K^*\overline{u}}{s(s-p_{1})(s-p_{2})}
$$
E antitrasformando otteniamo l'espressione dell'uscita.

La risposta al gradino rimarrà limitata solo nel caso i due poli siano strettamente negativi.
Per il teorema del valore finale abbiamo:
$$
y_{\infty}=\frac{K^*\overline{u}}{p_{1}p_{2}}
$$
Mentre la costante di tempo del sistema è dato dalla somma del contributo di ogni polo:
$$
\tau=\frac{1}{|p_{1}|}+ \frac{1}{|p_{2}|}
$$
