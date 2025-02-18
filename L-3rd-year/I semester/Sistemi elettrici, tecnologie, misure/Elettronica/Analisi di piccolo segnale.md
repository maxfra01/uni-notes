# Transistor in applicazioni analogiche
---
Considero il circuito in figura

![[Pasted image 20221026133349.png]]

Esso è composto da un [[Transistor]] nMOS, una resistenza R, un tensione di alimentazione $V_{DD}$ e collegati al terminale di drain due **generatori di polarizzazione** di tensione, il primo genera tensione, il secondo il segnale.
All'ingresso della porta di gate risulta:
$$
v_{GS}=v_{in}+V_{GS}
$$
Mentre all'uscita, applicando la legge di Kirchoff sulla maglia di uscita:
$$
v_{DS}=V_{DD}-R \cdot i_{D}
$$

Se mi trovo nella regione di saturazione, la corrente di drain non è costante ma è data da $i_{D}=\frac{1}{2}\beta (v_{GS}-V_{TH})^2$ 
Perciò andiamo a esaminare la transcaratteristica del transistore (supponiamo che $v_{in}$ sia un segnale sinusoidale).

![[Pasted image 20221026134415.png]]

A sinistra notiamo il valore costante di $V_{GS}$ che si può scostare in base al segnale sinusoidale e ricaviamo il valore della corrente del drain.
Perciò idealmente la corrente di drain dipende solamente dalla tensione in ingresso al gate.
Per vedere quanto vale la $v_{DS}$ costruisco il grafico con la relazione di uscita, a destra.

```ad-note
Il punto $Q$ in cui è centrato il segnale sinusoidale è detto **punto di quiescenza** o **punto di lavoro**, e si può determinare applicando al circuito solamente la tensione di indice ($V_{GS}$). In tal modo troviamo solamente un punto sul grafico.
```

```ad-note
I termini di tensione scritti in maiuscolo e minuscolo sono quelli completi.
Quelli solo maiuscoli indicano la componente continua (componente di polarizzazione).
Quelli solo minuscoli indicano il segnale (variazioni).
```

Avendo una dipendenza quadratica fra la corrente e la tensione in ingresso, produciamo anche la seconda armonica e quindi del rumore.
Per ovviare a ciò introduciamo il concetto della **Linearizzazione** che cerca di sostituire alla relazione quadratica la tangente in un intorno del punto di lavoro.
Ovviamente più il segnale oscillante ha un'ampiezza minore, più l'intorno è piccolo e quindi meglio l'approssimazione funziona.

# Analisi di piccolo segnale
---
L'approccio con metodo di Taylor è applicabile su ogni elemento circuitale che presenza caratteristiche non lineari.
Consideriamo un elemnto non lineare con entrata:
$$
x_{IN}(t)=X_{IN}+x_{in}(t)
$$
E uscita:
$$
y_{OUT}(t)=Y_{OUT}+ y_{out}(t)
$$
Allora ne posso eseguire lo sviluppo di Taylor centrato in $X_{IN}$ troncato al primo oridne:
$$
y_{OUT}(t)=f(X_{IN})+\frac{ \partial f }{ \partial x }|_{x=X_{IN}}\quad x_{in}(t) +o(|x_{in}|) = Y_{OUT}+y_{out}(t)
$$
Quindi il primo termine corrisponde al punto di lavoro/quiescenza, mentre la seconda parte diventa un sistema linearizzato (grazie alla derivata).
Facendo ciò abbiamo spezzato il sistema non lineare in due parti, uno non lineare statico e uno lineare per studiare le variazioni.

![[Pasted image 20221026141719.png]]

Andando a linearizzare si ottiene:

![[Pasted image 20221107085927.png]]

Ricordando che la corrente di gain è nulla in ingresso otteniamo che $i_{d}$ è pari a :
$$
i_{d}=g_{m}v_{gs} + g_{o}v_{ds}
$$
![[Pasted image 20221107090131.png]]

>Abbiamo individuato il **circuito equivalente** che corrisponde a un generatore di transconduttanza, naturalmente vogliamo $g_{o}$ molto piccola! 

Resta dunque da determinare il valore di $g_{0}$ e $g_{m}$
Ricordiamo che in regione di saturazione la corrente di drain è esprimibile come:
$$
i_{D}=\frac{1}{2}\beta(v_{GS}-V_{TH})^2(1+\lambda v_{DS})
$$
Notiamo ora:
$$
i_{d}=i_{D} -I_{D}=\frac{ \partial i_{D} }{ \partial v_{GS} }|_{Q}v_{gs} +\frac{ \partial I_{D} }{ \partial v_{DS} }|_{Q}v_{ds}  \quad \quad Q=(V_{GS},V_{DS}) 
$$
Dunque quei termini con le derivate corrispondono alle nostre conduttante:
$$
g_{m}=\frac{ \partial i_{D} }{ \partial v_{GS} }|_{Q}=\beta(V_{GS}-V_{TH})(1+\lambda V_{DS})=\frac{2I_{D}}{V_{GS}-V_{TH}} \approx \sqrt{ 2I_{D}\beta } 
$$
$$
g_{o}=\frac{ \partial i_{D} }{ \partial v_{DS} }|_{Q}=\frac{\lambda \beta}{2}(V_{GS}-T_{TH})^2 \approx \lambda I_{D} 
$$

# Validità dell'analisi
---
Non è sempre possibile effettuare l'analisi di piccolo segnale, in particolare dobbiamo accertarci che la variazione del segnale sia abbastanza piccola. 
Questa variazione è accettabile se si rimane nella regione di saturazione e dunque così facendo si definisce la dinamica!
Il **punto di lavoro** è scelto dal costruttore per massimizzare quest'ultima.
Altrimenti si da origine al cosiddetto **errore di distorsione (non linearità)** di tipo deterministico.

![[Pasted image 20221107094934.png]]


# Procedimento
---
1. Determinare il punto di lavoro $Q$ spegnendo tutte le componenti eccetto quelle in continua.
2. Si costruisce il circuito equivalente per il piccolo segnale. Si tengono accese solamente le componenti variabili nel tempo.
3. Si risolve il sistema con l'analisi circuitale e si valutano le richieste (Bode, grandezze in uscita, ecc...)