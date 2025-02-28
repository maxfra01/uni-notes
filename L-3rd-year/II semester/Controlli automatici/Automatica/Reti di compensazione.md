# Loop Shaping
---
Una volta determinata la parte statica del controllore, la parte dinamica $C'(s)$ viene costruita in modo che:
$$
\begin{cases}
\omega_{c} \approx \omega_{c,des} \\
m_{\phi} \geq m_{\phi,min}
\end{cases}
$$
Si parla proprio di **loop shaping** perchè $C(s)$ viene modellata in modo da rispettare le specifiche idonee.

Per la costruzione della funzione ad anello si parte dalla sua forma based:
$$
G_{a1}(s)=\frac{K_{c}}{s^h}F(s)
$$
Si procede valutando modulo e argomento di $G_{a1}(j\omega_{c,des})$ e calcolo in particolare la **variazione di fase necessaria per ottenere il margine di fase desiderato**, e la variazione **di modulo per portare la pulsazione di crossover a quella desiderata**.
$$
\Delta m_{dB}=-|G_{a1}(j\omega_{c,des})|_{dB}
$$
Solitamente per la fase si richiede di anticipare, mai di ritardare ($\Delta \phi\geq 0$)
Per ottenere l'anello completo tramite il loop shaping, si fa uso di **reti di compensazione**:
$$
G_{a}(s)=C'(s) \cdot G_{a1}(s)
$$
Ogni rete che introduciamo è tale da apportare una certa modifica in modulo o fase all'anello finale.
Questa fase di progetto è detta **sintesi per tentativi**, in quanto servono numerose correzioni successive e occorre verificare ogni volta ciò che si è ottenuto.


# Reti anticipatrici
---
Una rete **anticipatrice o derivativa** è descrita da una fdt in questa forma:
$$
R_{d}(s)=\frac{1+\tau_{d}(s)}{1+\frac{\tau_{d}}{m_{d}}s} \quad \tau_{d}>0, m_{d}>1
$$
Il polo ha una frequenza $m_{d}$ volte maggiore rispetto a quella del polo.
La rete ha i seguenti diagrammi di Bode:

![[Pasted image 20230519111907.png]]

**Posso usare questa rete per far recuperare la fase** in corrispondenza della pulsazione di taglio desiderata $\omega_{c,des}$, compensando il margine desiderato. **Come svantaggio** però ci sarà un **aumento del modulo**.
Usaremo queste reti ogni volta che dobbiamo **anticipare fase**.

Se abbiamo bisogno di molto anticipo di fase, è consigliabile usare due reti con $m_{d}$ piccolo (in genere minore di 16) piuttosto che un'unica rete con $m_{d}$ grande.

Uso perfetto di queste reti: quando devo recuperare fase e devo aumento il valore di $\omega_{c}$ ovvero aumentare il modulo di $G_{a}(s)$

### Progettazione di $R_{d}(s)$

Si sceglie $m_{d}$ in moda da garantire il **massimo recupero di fase $\phi_{max}$** consentito dalla rete:
$$
\phi_{max}=\arcsin\left( \frac{m_{d}-1}{m_{d}+1} \right)
$$
Perciò posso isolare $m_{d}$ ottenendo:
$$
m_{d}=\frac{1+\sin(\phi_{max})}{1-\sin(\phi_{max})}
$$
Si determina poi $\tau_{d}$ dai DdB imponendo che $\phi_{max}$ sia raggiunto proprio in $\omega_{c,des}$:
$$
\omega_{c,des} \cdot \tau_{d} = \sqrt{ m_{d} }
$$
Considerando di dover recuperare una quantità di fase pari a $\Delta \phi$ distinguiamo due casistiche:
- Se il **recupero di fase non è troppo elevato** (ovvero più piccolo di $60°$) allora possiamo ricorrere all'uso di **una sola rete anticipatrice** con $m_{d}=\phi_{max}$
- Se il recuper di fase **è maggiore di $60°$** allora occorre usare due o più reti scelti in modo da far si che la somma dei recuperi forniti da ogni rete sia pari alla fase da recuperare: $\Delta \phi = \sum_{i} \phi_{max,i}$

Ogni volta che uso una rete anticipatrice si ha un aumento del modulo: devo veridicare che sia compatibile con le richieste: ovvero voglio che **l'aumento di modulo sia inferiore o al massimo uguale al margine di guadagno $\Delta m_{dB}$**

Se il modulo aggiunto è minore del margine, allora basta aumentare il guadagno stazionario per ottenere la pulsazione di crossover che fa passare la funzione ad anello per 0 dB.

### Esempio

![[Pasted image 20230522131943.png]]

Partiamo dalle specifiche statiche: il sistema è di tipo uno, mentre l'errore finale è di tipo 1: non sono richiesti poli nell'origine.
$$
|e_{r,\infty}| = |\frac{K_{r}}{K_{c}K_{F}}| \leq 0.2 \implies |K_{C}|\geq 4
$$
Il sistema (già visto negli esempi prima) è a stabilità regolare quindi il segno del guadagno stazionario è positivo.

Per la seconda specifica dinamica abbiamo $t_{s}=0.4$ con risposta al gradino:
$$
w_{B}=\frac{3}{t_{s}}=7.5 \implies \omega_{c}=0.63\omega_{B}=4.7
$$
Per la specifica sulla sovraelongazione abbiamo:
$$
M_{r}\leq 1.39 \implies m_{\phi,min}=[43,45]
$$
Ora considera la funzione: $$
G_{a1}=\frac{40}{s(s+2)(s+4)}
$$
![[Pasted image 20230522133154.png]]

Voglio imporre che in quella pulsazione $\omega_{c}=4.7 \frac{rad}{s}$ ci sia il crossover delle frequenze: ovvero devo imporre che il guadagno passi per 0 dB, inoltre voglio imporre di risalire di 70° perchè 26 sono per raggiungere i -180 e gli altri sono quelli del margine di fase che devo garantire.

Devo recuperare fase e modulo, quindi il perfetto uso delle reti anticipatrici.

Proviamo ad usare due reti con $m_{d}=4$ in quanto questo valore permette di far recuperare a ogni rete un valore di fasi pari a 36.9
Per $m_{d}=4$ si legge dai diagrammi che ogni rete recupererà 6dB sul modulo.
$$
w_{c} \cdot \tau_{d} = \sqrt{ m_{d} } \implies \tau_{d}=0.425
$$

Abbiamo quindi una rete anticipatrice descritta da:
$$
R_{d}(s)=\frac{1+0.425s}{1+ \frac{0.425}{4}s}
$$
Il **controllore dinamico** costituito da due reti anticipatrici è allora nella forma:
$$
C'(s)=R_{d}^2
$$
E il controllore generale sarà:
$$
C(s)=K_{c} C'(s) \implies G_{a}=C(s)F(s)
$$
Ora posso testare le specifiche sui margini sulla funzione ad anello.
Se una **specifica dinamica non è soddisfatta** allora devo riprogettare correggendo il controllore dinamico.
Se una **specifica statica non è soddisfatta** allora c'è un errore progettuale!


### Progettazione alternativa

In questo secondo caso voglio **contenere l'aumento di modulo**: per farlo al posto che usare il massimo recupero di fase possibile della rete, uso una rete con $m_{d}$ maggiore in modo da avere l'aumento di fase di cui necessito, prima di arrivare al massimo.
A parità di recupero di fase (fra questo e il metodo classico) ho aggiunto meno modulo. 
Per farlo userò un ascissa normalizzata in cui ho aumento di modulo desiderato e incremento di fase che devo introdurre:
$$
w_{c,des} \cdot \tau_{d} = x_{d}
$$
Dove $x_{d}$ è l'ascissa che vedo dai grafici (tabelle fornite) in grado di avere il recupero di fase che mi interessa e un aumento di modulo compatibile.

### Esempio

![[Pasted image 20230522140829.png]]

La nuova specifica impone che la pulsazione di crossover desiderata sia pari a $\omega_{c}=3.8 rad/s$

![[Pasted image 20230522140908.png]]

Notiamo che dobbiamo recuperare 7dB in modulo e 60 gradi in fase: possiamo usare reti anticipatrici.

Se usassimo due reti con $m_{d}=3$ otterremo un recupero di fase pari a 60 gradi circa MA 9 dB in modulo, quindi non è possibile procedere per questa via.
Questo avviene nell'ascissa pari a $x_{d}=\sqrt{ m_{d} }$

La soluzione ideale prevede due reti con $m_{d}=3$ ed $m_{d}=4$ entrambe valutate in ascisse $x_{d_{1}}, x_{d_{2}}$ minori del punto di massimo recupero.
Se prendo $x_{d_{1}}=1.3$ sono in grado di recuperare 28.9 gradi e 3.55 dB, mentre per la seconda rete in $x_{d_{2}}$ ho un recupero di 33 gradi e 3.38 dB.
In totale ho recuperato circa 62 gradi e 7db

![[Pasted image 20230522141421.png]]

Allora dalle relazioni $\omega_{c,des} \cdot \tau_{d}=x_{d}$ ricavo i due tau e ho individuato le due reti.
Posso procedere con la verifica dei requisiti sulla funzione ad anello.
Poi passerò alla verifica delle specifiche in catena chiusa.


# Reti attenuatrici
---
Una rete **Attunuatrici, o integrativa** è una rete descritta dalla funzione di trasferimento:
$$
R_{i}(s)= \frac{\left( 1+\frac{\tau_{i}}{m_{i}}s \right)}{1+\tau_{i}s} \quad \quad \tau_{i}>0, m_{i}>1 
$$
Ovvero abbiamo un polo in $-1/\tau_{i}$ e ad una pulsazione $m_{i}$ volte più grande abbiamo lo zero in $-\frac{m_{i}}{\tau_{i}}$

I DdB della rete in question sentià prima l'effetto del polo e successivamente trova l'effetto dello zero.

![[Pasted image 20230523145154.png]]

L'**attenuazione massima in modulo** che fornisce la rete è proprio pari al valore $m_{i}$:
$$
|R_{i}(j\omega)|= - |m_{i}|_{dB}
$$
Ovviamente questo vale per frequenze maggiori di quello zero.

Usiamo le reti integrative **quando dobbiamo ridurre il modulo della funzione ad anello** e conseguentemente otterremo anche una **perdita di fase**, di entità massima nell'intervallo di valori compreso fra la pulsazione del polo e quella dello zero. Si annulla poi per frequenze maggiori a quello zero.

Possiamo usare SOLO reti attenuatrici solo quando è necessario ridurre $\omega_{c}$

### Progettazione della $R_{i}(s)$

Data $G_{a}'(s)$, ovvero la funzione ad anello ottenuta **prima di mettere la rete attenuatrice**, si sceglie $m_{i}$ pari all'attenuazione di modulo necessaria per imporre la frequenza di crossover desiderata.

Scegliamo un punto di ascissa che soddisfi la relazione:
$$
\omega_{c,des} \cdot \tau_{i} = x_{i}
$$
Non possiamo però prendere un $x_{i}$ troppo elevato (attenuazione di modulo, 0 fase persa in pratica) in quanto andiamo ad aumentare il tempo di assestamento del sistema.


# Reti integro-derivative
---
Combinazione dei due tipi di rete, con fdt nella forma:

![[Pasted image 20230523151711.png]]

Il suo effetto è quello di unire le caratteristiche delle reti di cui è formata, in contemporanea:
- Recupero di fase
- Attenuazione del modulo

Si utilizza questa rete quando:
- La fase è insufficiente a garantire il margine di fase richiesto
- Il modulo è maggiore di 1 (0dB) o è poco inferiore.

### Progettazione

Si **progetta sempre prima la rete anticipatrice**, altrimenti gli effetti negativi della attenuatrice causerebbero dei problemi per il progetto dell'anticipatrice.

# Attività sul comando
---
Quando scegliamo la procedura alternativa di progetto di una rete derivatrice dobbiamo tenere conto dell'**attività sul comando** che si genera.
Consideriamo perciò il solito sistema da controllare, con un riferimento a gradino (in quanto il più critico da inseguire, perchè imponiamo un valore che dovrebbe istantaneamente essere assunto dall'ingresso).

![[Pasted image 20230524085853.png]]

Per il **teorema del valore iniziale** sappiamo che:
$$
\lim_{ t \to 0 } u(t) = \lim_{ s \to \infty } \{s \cdot u(s)\}=\lim_{ s \to \infty } \left( s \cdot \frac{C(s)}{1+G_{a}(s)} \cdot \frac{1}{s} \right )= \lim_{ s \to \infty } C(s)
$$
Allora se consideriamo un $C(s)$ **privo di poli nell'origine**, composta dalla produttoria di reti antipatrici e attenuatrici, e caratterizzata da un guadagno $K_{c}$ allora l'**attività sul comando** risulta pari a:
$$
\lim_{ t \to 0 } u(t)=K_{c} \cdot \frac{\prod_{k} (m_{d,k})}{\prod_{j}(m_{i,j})}
$$
Quindi **più reti anticipatrici inseriamo più l'attività sul comando risulta essere elevata**. Invece l'effetto delle rete attenuatrici è quello di diminuire l'attività sul comando.

Nella pratica per ottenere il valore del comando posso simulare in matlab usando i seguenti comandi:

```MATLAB
W_u=K_r*feedback(1,G_a);
figure,step(W_u)
```

Dopodichè posso leggere il valore del comando al tempo $0$.
Se è il valore massimo posso usare `characteristics > peak response`
In simulink dopo aver applicato un ingresso a gradino possiamo mettere un oscilloscopio dopo il controllore.

Nel caso di un $C(s)$ che contiene **almeno un polo nell'origine** allora l'attività sul comando massimo è raggiunta per un tempo $t>0$.
In questo caso prima simulo la risposta al gradino e noto quando il transitorio si esaurisce, poi lancio `step(W,tx)` dove $t_{x}$ è l'istante in cui finisce il transitorio: così facendo si ottiene più precisione sul grafico.


# Altra rete di compensazione
---
Se **Nel controllore ho messo io un polo nell'origine**, e devo **garantire anche un buon anticipo di fase** allora è conveniente aggiungere anche uno **zero**:

![[Pasted image 20230526092612.png]]

La fdt complessiva è pari a:
$$
C_{PI}(s)=\frac{1+\tau s}{s}
$$
La rete così formata è di tipo **proporzionale integrale**.

### Progettazione

Una volta che ho fissato il $K_{c}$ al solito modo, vado a determinare la collocazione di questo zero in base al recupero di fase (sostituisce di fatto la rete anticipatrice).