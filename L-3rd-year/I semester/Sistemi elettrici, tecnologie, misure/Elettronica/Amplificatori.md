# Introduzione
---
Considerando l'amplificatore come un blocco funzionale, desideriamo il seguente effetto:

![[Pasted image 20221018083814.png]]

Dal punto di vista applicativo possiamo assegnare all'amplificatore diversi usi:
- L'amplificatore può essere il primo elemento di una catena di acquisizione, con lo scopo di aumentare la dinamica di un segnale (**LOW NOISE AMPLIFIER (LNA)**)
- L'amplificatore è un sotto-blocco per funzioni analogiche più complesse
- L'amplificatore può aumentare la potenza dei segnali al fine di pilotare gli attuatori (**AMPLIFICATORI DI POTENZA**).


# Classificazione Amplificatori
---
Possiamo distinguere gli amplificatori in base alle grandezze in ingresso e in uscita.

| IN: Tensione, OUT: Tensione                       | IN: Tensione, OUT: Corrente                     | IN: Corrente, OUT: Corrente                       | IN: Corrente, OUT: Tensione      |
| ------------------------------------------------- | ----------------------------------------------- | ------------------------------------------------- | -------------------------------- |
| Amplificatore di Tensione                         | Amplificatore di Transconduttanza               | Amplificatore di Corrente                         | Amplificatore di Transresistenza |
| $v_{out}(t)=A_{v}v_{in}(t)$                       | $i_{out}(t)=g_{m}v_{in}(t)$                     | $i_{out}(t)=A_{i}i_{in}(t)$                       | $v_{out}(t)=R_{m}i_{in}(t)$      |
| $A_{v}$ amplificazione di tensione, adimensionato | $g_{m}$ transconduttanza, $[g_{m}]=\Omega^{-1}$ | $A_{i}$ amplificazione di corrente, adimensionato | $R_{m}$ transresistenza, $[R_{m}=\Omega]$                                 |

Idealmente i quattro amplificatori corrispondono ai generatori controllati lineari!

Nel caso di amplificatori con tensione in ingresso:
- Ci aspettiamo che l'amplificatore non perturbi la tensione in ingresso.
- L'ingresso è la tensione (eq. Thevenin nel caso lineare)
- Nella realtà un po' di corrente è assorbita dell'amplificatore, infatti $R_{in}$ è finita (non infinita come nel caso ideale).

Nel caso di ingresso di corrente:
- L'ingresso è la corrente erogata da un bipolo chiuso in corto circuito (Norton).
- Ci aspettiamo che la corrente in ingresso non venga perturbata.
- La porta d'ingresso si deve comportare come un cortocircuito.
- Nella realtà $R_{in}$ non è nulla, quindi c'è una piccola ricaduta di tensione.

Nel caso di uscita in **tensione**:
- Ci si aspetta la tensione prevista, indipendentemente dal carico collegato in uscita.
- L'uscita deve comportarsi come un generatore ideale di tensione.

Nel caso di uscita in corrente:
- L'uscita si deve comportare come un generatore di corrente ideale, ma la corrente nella realtà dipende anche dalla tensione di carico
- Ci si aspetta la corrente prevista, indipendentemente dal carico collegato in uscita.

### Amplificatori: resistenze di ingresso e d'uscita
Tutti gli amplificatori reali sono caratterizzati dai valori $R_{in}$ ed $R_{out}$.
Applicando una corrente di test che viene applicata all'ingresso o all'uscita (dopo aver spento i generatori indipendenti) per misurare questi due valori:
$$
R_{in}=\frac{v_{in}}{i_{T}} \quad \quad R_{out}=\frac{v_{out}}{i_{T}}
$$
# Effetto di carico su Amplificatori
---

### Amplificatore di tensione
Consideriamo nel primo caso un **amplificatore di tensione**:
- Ingresso della tensione $v_{s}$
- Resistenza interna $R_{s}$
- Uscita pilota sul carico $R_{L}$

![[Pasted image 20221018093916.png]]

Nel caso ideale dovremmo avere $R_{in} \to \infty ;R_{out} \to 0$
Per ridurre l'effetto di carico dobbiamo verificare le due condizioni:
- $R_{s}\ll R_{in}$
- $R_{out}\ll R_{L}$
Nei casi ideali ovviamente non è richiesto, perchè già garantito.
L'espressione della tensione in uscita:
$$
v_{out}=\frac{R_{in}}{R_{in}+R_{s}} \cdot \frac{R_{L}}{R_{L}+R_{out}}A_{v}v_{s}
$$
Se le due condizioni si verificano le due frazioni tendono a $1$.

### Amplificatori di Corrente

![[Pasted image 20221018095042.png]]

Nel caso ideali mi aspetto che: $R_{in} \to 0 ; R_{out} \to \infty$
L'uscita attesa considerando l'effetto di carico è.:
$$
i_{out}=\frac{R_{s}}{R_{s}+R_{in}} \cdot \frac{R_{out}}{R_{out}+R_{L}}A_{i}i_{s}
$$
Ovviamente se minimizziamo l'effetto, le due frazioni tenderanno a $1$:
- $R_{in} \ll R_{s}$
- $R_{L}\ll R_{out}$

### Amplificazione di Transconduttanza

![[Pasted image 20221018095408.png]]
Nel caso ideale vorremmo $R_{in} \to \infty ; R_{out} \to \infty$
Consideriamo l'uscita dell'amplificatore:
$$
i_{out}=\frac{R_{in}}{R_{in}+R_{s}} \cdot \frac{R_{out}}{R_{L}+R_{out}}g_{m}v_{s}
$$
Le frazioni tendono a $1$ per:
- $R_{s}\ll R_{in}$
- $R_{L}\ll R_{out}$

### Amplificatore di transresistenza

**![[Pasted image 20221018101820.png]]

Condizioni di idealità: $R_{in} \to 0, R_{out} \to 0$
L'espressione dell'uscita è pari a:
$$
v_{out}=\frac{R_{s}}{R_{in }+ R_{s}} \cdot \frac{R_{L}}{R_{L}+R_{out}}R_{m}i_{s}
$$
Le frazioni vanno a $1$ per:
- $R_{in}\ll R_{s}$
- $R_{out}\ll R_{L}$


# Amplificatori di Potenza
---
Consideriamo un amplificatore di tensione ideale

![[Pasted image 20221018102435.png]]

La potenza assorbita dalla porta in ingresso è:
$$
P_{in}=v_{in}i_{in}=0
$$
Dato che il ramo aperto fa si che non passi corrente.
Per il ramo di uscita abbiamo invece:
$$
P_{out}=\frac{v_{out}^2}{R_{L}}= \frac{A_{v}^2v_{in}^2}{R_{L}}$$
E notiamo che per $R_{L} \to 0$ abbiamo che la potenza erogata dalla porta di uscita tende all'infinito.
Possiamo definire l'amplificazione di potenza come:
$$
A_{P}=\frac{P_{out}}{P_{in}} \to \infty
$$
### Potenza generata dall'alimentazione
Dobbiamo dedurre che l'amplificatore genera potenza dal nulla? Certamente no, infatti la potenza in uscita è sempre presente a causa dell'**alimentazione** che deve sempre esserci anche se non indicata. L'amplificatore genera **potenza di segnale** a partire dell'alimentazione.

![[Pasted image 20221018103212.png]]
L'informazione del segnale è associata all'ingresso dell'amplificatore.
Possiamo definire la potenza legata all'alimentazione:
$$
P_{AL}=V_{AL}I_{AL}
$$
Normalemente l'amplificatore è passivo ovvero $P_{out}<P_{AL}+P_{in}$
Dunque posso definire l'**efficienza energetica**, pari a:
$$
\eta=\frac{P_{out}}{P_{AL}+P_{in}} \approx \frac{P_{out}}{P_{AL}}
$$
Spesso questo termine negli amplificatori di segnale è molto basso (minore del 20%) ma negli amplificatori di potenza è fondamentale averlo elevato.

### Potenza nel caso reale

![[Pasted image 20221018104217.png]]
$$
P_{in}=\frac{v_{in}^2}{R_{in}}; P_{out}=\frac{R_{L}A_{v}^2v_{in}^2}{(R_{out}+R_{L})^2}
$$
Possiamo definire l'amplificazione di potenza come:
$$
A_{P}=\frac{P_{out}}{P_{in}}=\frac{R_{L}R_{in}}{(R_{out}+R_{L})^2}A_{v}^2
$$
Se cerco il valore massimo di $P_{out}$ allora ne faccio la derivata ($\frac{ \partial P_{out} }{ \partial R_{L} }=0$) e noto che c'è un massimo nella condizione $R_{L}=R_{out}$ spesso impostati a $50\Omega$.


# Limitazioni di dinamica
---
L'intevallo dei valori che un segnale può assumere prende il valore di **Dinamica**.
I sistemi elettronici posso ricevere segnali in ingresso entro una certa dinamica, e posso fornire un uscita entro una certa dinamica.

**La dinamica del segnale dev'essere compatibile con la dinamica del sistema**

Anche nel caso degli amplificatori devo rientrare in una certa dinamica.

![[Pasted image 20221018105450.png]]

Valori delle dinamiche di ingresso:
- Amplificatore di Tensione:
$$
\frac{V_{out,min}}{A_{v}}<v_{in}(t)< \frac{V_{out,max}}{A_{v}}
$$
- Amplificatore di Corrente:
$$
\frac{I_{out,min}}{A_{i}}<i_{in}(t)< \frac{I_{out,max}}{A_{i}}
$$
- Amplificatore di Transconduttanza:
$$
\frac{I_{out,min}}{g_{m}}<v_{in}(t)< \frac{I_{out,max}}{g_{m}}
$$
- Amplificatore di Transresistenza:
$$
\frac{V_{out,min}}{R_{m}}<i_{in}(t)< \frac{V_{out,max}}{R_{m}}
$$

# Magagna di Slew Rate
---
Esiste una limitazione sulla derivata temporale della tensione d'uscita, si parla di **slew rate**. In altre parole la tensione d'uscita non può variare troppo velocemente.
$$
SR^- <\frac{ \partial v_{out} }{ \partial t } < SR^+ 
$$
Il segnale in uscita non può avere pendenza superiore allo slew rate.


# Rumore
---
Il segnale che consideriamo è sovrapposto del **rumore**, variazione a cui non è associata informazione.
Altre fonti di rumore sono le interferenze di altri dispositivi.
Il rumore fuori dalla banda del segnale deve essere filtrato.

Definiamo il **SIGNAL-to-NOISE RATIO (SNR)** come rapporto fra la potenza del segnale e di rumore su un dato carico.
$$
SNR=\frac{P_{signal}}{P_{noise}}=\frac{\overline{v}^2}{\overline{n}^2}
$$
In un amplificatore ideale, dove entra un segnale del tipo $x +n_{0}$ otteniamo un'uscita pari a $y=Ax+An_{0}$ e ci aspettiamo la seguente relazione:
$$
SNR_{out}=SNR_{in}
$$
Mentre in un amplificatore reale otteniamo:
$$
SNR_{out}<SRN_{in}
$$

Essendo l'amplificatore un circuito elettrico, allora esso aggiunge rumore alla sua uscita.
Se considero diversi amplificatori collegati in cascata:

![[Pasted image 20221024085953.png]]

Calcolando l'uscita possiamo stimare un rumore equivalente al circuito:
$$
n_{eq}=\frac{n_{1}}{A_{1}}+ \frac{n_{2}}{A_{1}A_{2}}+ \frac{n_{3}}{A_{1}A_{2}A_{3}}$$
Ovvero per ogni stadio il rumore è diviso per la relativa amplificazione e per tutte quelle che lo precedono.

Considerando le amplificazione $\gg 1$ allora solamente il primo stadio è critico in quanto gli altri sono trascurabili.
Il primo amplificatore per questo è definito **LNA Low Noise Amplifier**.


# Offset
---
E' presente un errore di fuori zero $\Delta X$ dovuto alle tolleranze di fabbricazione. Anch'esso viene amplificato "sporcando" il segnale.

Oltre al fuori zero, c'è una tolleranza di fabbricazione del 20% su circa tutti i parametri dell'amplificatore (non accettabile).

Per ottenere amplificatori più precisi $\implies$ **principio della retroazione**.


# Distorsione non-lineare
---
Anche rispettando le condizioni per le limitazioni di dinamica la caratteristica ingresso-uscita dell'amplificatore non è lineare.
Si parla dunque di **distorsione non lineare**, e per quantificarla possiamo approssimarla come uno sviluppo di taylor di ordine $>1$ nell'intorno del punto di lavoro.
$$
y=Ax+A_{2}x^2+A_{3}x^3+\dots
$$
Il primo termine corrisponde all'amplificazione lineare, mentre da lì in poi di parla di $d(x)$, errore dovuto alla linearità.

![[Pasted image 20221024092632.png]]

Per i segnali sinusoidali, la non linearità di ordine $n$ da luogo ad errore a frequenza $n\omega_{0}$

Per realizzare nella pratica un amplificatore si fa uso di [[Transistor]]. 
