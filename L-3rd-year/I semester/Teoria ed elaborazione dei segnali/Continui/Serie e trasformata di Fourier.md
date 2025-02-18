# Funzione porta
---

Possiamo notare che un segnale continuo nel tempo può essere approssimato mediante una base di segnali costitutita da funzioni porta traslate nel tempo.
$$
x(t) \approx x'(t) = \sum_{n=-\infty}^\infty x(n\Delta T)p_{\Delta t}(t-n\Delta t)
$$
![[Pasted image 20221007094507.png]]

L'approssimazione ovviamente risulta migliorata tanto più quanto $\Delta t$ è piccolo, perciò passimao al limite.
$$
x(t)=\lim_{ \Delta t \to 0 }\sum_{n=-\infty}^\infty x(n\Delta t)p_{\Delta t}(t-n\Delta t) 
$$
Questo risultato sarà utile per il **teorema del campionamento**, tuttavia ora notiamo che disponiamo di un numero infinito di funzioni per rappresentare la 'base completa'

Ovviamente affinchè sia una base ortonormale, le funzioni devono essere normalizzate, quindi per una porta di ampiezza $\Delta t$ occorre che l'altezza di quest'ultima sia $\frac{1}{\sqrt{ \Delta t }}$.


# Delta di Dirac
---

E' una funzione speciale nel campo dei segnali, matematicamente è una distribuzione che può essere costruita a partire da funzioni porta:
$$
\delta(t)=\lim_{ \Delta t \to 0 } \frac{1}{\Delta t}p_{\Delta t}(t) 
$$
![[Pasted image 20221007095226.png]]

Nel corso rappresenteremo la delta come una freccia verso l'alto ad indicare un impulso unitario in quel preciso istante temporale.
Due importanti proprietà:
$$
\textrm{Area unitaria} \quad \int _{-\infty}^\infty \delta(t)dt =\lim_{ \Delta t \to 0 } \frac{1}{\Delta t}\int _{-\infty}^\infty p_{\Delta t}(t)\, dt=\lim_{ \Delta t \to 0 } \frac{1}{\Delta t}\Delta t=1
$$
$$
\textrm{Energia infinita} \quad E(\delta (t))= \int_{-\infty}^{\infty}(\delta(t))^2 \, dt = \dots =\infty 
$$
Elenchiamo ora altre due proprietà di natura più pratica e matimatica
$$
\int_{-\infty}^{\infty} x(t)\delta(t) \, dt = x(0)
$$
Perchè esplicitando la delta con la funzione porta centrata in zero, al limite si ottiene il valore con ascissa 0.
Analogamente se trasliamo la delta, ottenerremo il valore del segnale nel punto di traslazione.
$$
\int_{-\infty}^{\infty} x(t)\delta(t-t_{0}) \, dt =x(t_{0})
$$
Diciamo che l'integrale della moltiplicazione tra un segnale e la delta **campiona il segnale nella posizione della delta**.
Ovviamente è anche valido:
$$
x(t) \cdot\delta(t-t_{0})=x(t_{0}) \cdot \delta(t-t_{0})
$$
Il risultato è ancora una funzione delta, ch e ha l'altezza paria $x(t_{0})$.
Affinchè il segnale sia campionato occorre l'integrale!


# Serie di Fourier
---

Particolare rappresentazione di un segnale nel tempo attraverso una serie di **seni e coseni**.

Consideriamo la seguente base, si dimostra che è una **base completa** per tutti i segnali (anche complessi) **ad energia finita nell'intervallo** $[  -\frac{T}{2} \frac{T}{2}]$
$$
w_{n}(t)=\frac{1}{\sqrt{ T }}e^{j \frac{2\pi}{T} nt}
\quad \quad -\frac{T}{2}\leq t\leq \frac{T}{2}
$$
Possiamo dunque usare una combinazione lineare di esponenziali complessi per rappresentare un segnale sul supporto indicato qui sopra.
Se poi i segnali sono periodici basta sommare i termini traslandoli del periodo.

Si dimostra che qualunque segnale periodico di periodo $T$ si può scrivere come:
$$
x(t)= \frac{1}{\sqrt{ T }}\sum_{n=-\infty}^\infty c_{n}e^{j \frac{2\pi}{T}nt}
$$
Di conseguenza possiamo trovare i coefficienti (dimostrazione sulle slide) coerentemente dallo spazioe dei segnali:
$$
c_{n}=\frac{1}{\sqrt{ T }} \int_{-\frac{T}{2}}^{\frac{T}{2}}  x(t) \cdot e^{-j \frac{2\pi}{T}nt}\, dt =\langle x(t), w_{n}(t)\rangle
$$

### Coefficienti normalizzati
In questo corso useremo una notazione diversa: indicheremo i coefficienti dello sviluppo con $\mu_{n}$ che sostianzialmente tiene conto della normalizzazione del periodo.
$$
x(t)=\sum_{n=-\infty}^\infty \mu_{n}e^{j \frac{2\pi}{T} nt} \quad \quad \quad \mu_{n}=\frac{1}{\sqrt{ T }}c_{n}
$$
Possiamo calcolare **l'energia del segnale** sull'intervallo troncato al suo periodo e otteniamo:
$$
E_{T}(x)=T\sum_{n=-\infty}^\infty |\mu_{n}|^2
$$
Notiamo che sul periodo l'energia è finita, ma se consideriamo l'intero asse dei tempi allora l'energia è infinita.
Per la **potenza media** abbiamo:
$$
P(x)=\frac{E_{T}(x)}{T}=\sum_{n=-\infty}^\infty |\mu_{n}|^2
$$
Considerando quindi $n$ sinusoidi di frequenza $f_{n}=\frac{n}{T}$ allora possiamo dire che **il coefficiente n-esimo $\mu_{n}$ ci dice, per ciascuna componente a frequenza $f_{n}$ quanto essa è forte nel segnale $x(t)$**.


### Rappresentazione grafica

![[Pasted image 20221007111610.png]]

La frequenza $\frac{1}{T}$ è detta fondamentale, le altre 'armoniche' perchè sono multipli della fondamentale.
Si parla di **analisi spettrale**.
In particolare la **separazione spettale** fra le armoniche è data da:
$$
\Delta f=f_{n}-f_{n-1}=\frac{1}{T}
$$

# Trasformata di Fourier
---

Andiamo a considerare ciò che abbiamo visto fin'ora:
$$
x(t)=\sum_{n=-\infty}^\infty\left[ \frac{1}{T} \int _{-\frac{T}{2}}^{\frac{T}{2}} x(\theta)e^{-2j\pi \frac{n}{T}\theta}\, d\theta  \right]e^{2j\pi\frac{n}{T}t}
$$
Cerchiamo di estendere il significato della serie di Fourier a periodi fuori dall'intervallo considerato precedentemente, perciò
$$
T \to \infty \implies \frac{1}{T} \to df \, , \, \frac{n}{T} \to f \, , \, \sum \to \int 
$$
Andiamo a definire formalmente la **trasformata di Fourier** come:
$$
X(f)=\mathcal{F}(x(t))=\int_{-\infty}^{\infty} x(\theta)e^{-2j\pi f\theta} \, d\theta 
$$
E l'**antitrasformata** come:
$$
x(t)=\mathcal{F}^{-1}(X(f))=\int_{-\infty}^{\infty} X(f)e^{2j\pi ft} \, df 
$$
Definiamo poi la trasformata di segnali periodici nel tempo come:
$$
X(f)=\sum_{n=-\infty}^\infty \mu_{n} \cdot \delta(f-f_{n})
$$

# Proprietà della trasformata di Fourier
---

**TRASLAZIONE**
$$\mathcal{F}[x(t-\theta)]=\mathcal{F}[x(t)]e^{-2j\pi\theta f}$$
Il modulo non cambia nel dominio del tempo, ma compare un fattore esponenziale moltiplicato per la trasformata del segnale.

**MODULAZIONE DI AMPIEZZA**
$$
\mathcal{F}[x(t)e^{2j\pi f_{0}t}]=X(f-f_{0})
$$
![[Pasted image 20221013101116.png]]

Il contenuto spettrale della trasformata è shiftato sull'asse delle frequenza di una quantità $f_{0}$.

**SCALAMENTO**
$$
\mathcal{F}[x(Kt)]=\frac{1}{|K|}X\left(  \frac{f}{K} \right)
$$
![[Pasted image 20221013102836.png]]

Iniziamo a notare una dualità fra l'asse del tempo e quello delle frequenze.
Se nel dominio nel tempo un segnale risulta più compresso, allora nel dominio delle frequenze tenderà ad allungarsi, e viceversa.

**RELAZIONE DI PARITA'**

![[Pasted image 20221013103131.png]]

Il comportamento della parte reale e immaginaria seguono una **simmetria Hermitiana**.
Per i segnali reali, le frequenza negative non aggiungono significato, in quanto hanno la stessa informazione di quelle positive.

**CONVOLUZIONE E PRODOTTO**
$$
x(t)*y(t)=X(f)Y(f)
$$
Un prodotto di convoluzione nel dominio del tempo diventa un prodotto in quello della frequenza.

**DERIVAZIONE E INTEGRAZIONE**
Nel caso di derivata prima ho:
$$
\mathcal{F}[\dot{x}(t)]=2j\pi f \mathcal{F}[x(t)]=2j\pi f X(f)
$$
Mentre la regola generale è:
$$
\mathcal{F}[x^{(n)}(t)]=(2j\pi f)^n X(f)
$$
Per l'integrazione avrò:
$$
\mathcal{F}\left[ \int_{-\infty}^{t} x(r)  \, dr \right]= \frac{1}{2}X(0)\delta(f) + \frac{X(f)}{2j\pi f} 
$$
**DUALITA'**
Valgono le due seguenti proprietà:
$$
\mathcal{F}[X(f)]=x(-t) \quad \quad \mathcal{F}[X(t)]=x(-f)
$$

# Fourier come sviluppo su base (infinita)
---
Ricordiamo l'insieme di segnali $w_{f}(t)=e^{-2j\pi ft}$ , e ricordiamo che sono ortogonali.
Con quei segnali possiamo approssimare qualunque funzione.
Possiamo dunque usare trasformata e antitrasformata come uno sviluppo su una base ortogonale.
Si dimostrano le seguenti proprietà:
$$
E(x)=\int |x(t)|^2 \, dt = \int |X(f)|^2 \, df 
$$
Nota come **Uguaglianza di Parseval**
$$
\langle x(t),y(t)\rangle=\langle X(f),Y(f)\rangle
$$
Noto come **invarianza del profotto scalare**
$$
|\langle X(f), Y(f)\rangle | \leq ||X(f)| |\cdot | |Y(f) | |
$$
Nota come **Diseguaglianza di Schwarz**

Si dimostra anche che dalla dualità tempo-frequenza si ottiene:
- Se $x(t)$ è a supporto finito, allora $X(f)$ sarà a supporto infinito.
- Se $X_{f}$ è a supporto finito, allora $x(t)$ sarà a supporto infinito.

### Funzioni limitate nel tempo
Possiamo considerare una qualunque funzione limitata in $\left[ -\frac{T}{2}, \frac{T}{2} \right]$, quest'ultima possima sempre pensarla come una funzione illimitata ausiliaria e una porta di ampiezza $T$
$$
x(t)=p_{T}(t) \cdot x_{I}(t) \implies X(f)= X_{I}(f) * \frac{\sin(\pi fT)}{\pi f}
$$
Notiamo che qualunque sia l'andamento di $X_{I}(f)$ dato che moltiplichiamo per una sycn, allora questo prodotto darà luogo a un supporto infinito.
Ne deduciamo che **funzioni limitate nel tempo danno luogo a una trasformata di fourier illimitata in frequenza**.

### Analisi rigorosa della relazionte temporale
Definiamo due quntità:
$$
d^2=\int t^2 \frac{|x(t)|^2}{E(x)} \, dt 
$$
Detta **Estensione temporale**
$$
D^2=4\pi^2 \int f^2 \frac{|X(f)|^2}{E(x)} \, df 
$$
Detta **Estensione in frequenza**
Allora si dimostra che:
$$
d \cdot D \geq \frac{1}{2}
$$






[[Rappresentazione vettoriale dei segnali]].