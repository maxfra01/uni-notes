# Architettura di un oscilloscopio digitale
---
Come tutti i dispositivi di questa tipologia, il DSO presenta il seguente schema (generico) con le componenti 'base' che consentono l'elaborazione, la memorizzazione e la visualizzazione dl segnale.

![[Pasted image 20220929184736.png]]

Tra i componenti abbiamo:
- I circuiti di condizionamenti, utili ad esempio all'amplificazione del segnale o all'attenuazione a seconda del Full Range
- l'ADC, il convertitore
- La memoria di acquisizione che conserva temporaneamente o no i segnali
- un microcontrollore per permettere l'elaborazione
- una memoria video e dispostivo di output(CRT o LCD) per la visualizzazione finale


# Schema operativo 1
---
Un primo tipo si sistema DSO è rappresentato nel seguente schema che verrà trattato in dettaglio per ogni componente:

![[Pasted image 20220929185441.png]]

Notiamo che esistono subito diverse interfacce per la post-elaborazione a carico di un computer.
Nella parte superiore distinguiamo diversi parametri di interesse:
- Banda passante dai circuiti analogici (Hz)
- Incertezze delle misure in ampiezza
- Frequenza di campionamento (Sa/s)
- Profondità della memoria (Sa)

Ora andiamo in profondità con l'analisi dello schema:

### Blocco 1

![[Pasted image 20220929185823.png]]

La linearità e la stabilità dei componenti participano alla determinazione dell'incertezza.
Questo blocco agisce come un **filtro passa basso**, di cui ci viene fornita la banda passante $B$ a -3dB: di fatto la frequenza di taglio è superiore pari a $B$.
- Dunque un segnale con frequenza $B$ è attenuato del 30%.
Se si vuole ridurre l'attenuazione sull'ampiezza allora si applica la "regola del 5x" ovvero:
$$
B=5\cdot f_{max}
$$
dove $f_{max}$ è la più alta componente in frequenza del segnale in misura.
La banda pone anche un limite al **tempo di salita** secondo la relazione:
$$
t_{so}=\frac{K}{B}
$$
dove $K$ rappresenta una costante con valori fra $0.35$ e $0.45$.
Le modalità del selettore di ingresso sono:
- ground (GND)
- Coupling DC
- Coupling AC, in questa modalità la risposta del DSO agisce da filtro passa basso con $f_{t}$ dell'ordine di alcuni $Hz$.


### Processo di acquisizione

![[Pasted image 20220929190203.png]]

I parametri di interesse dell'ADC sono:
- frequenza di campionamento (ordine dei GigaSample/s)
- Numero di bit per rappresentare gli stati
La frequenza di campionamento è decisa dall'utilizzatore andando ad agire con il comando `time/div` nella sezione 'Base tempi'.

In corrispondenza del segnale di trigger inizia la conversione alla frequenza stabilita, in modo da ottenere il numero predefinito di campioni.
Una volta in memoria i campioni acquisiti sono conservato per un tempo indefinito **a meno di un altro evento di trigger**.
Questo distingue gli oscilloscopi digitali da quelli analogici (i **transitori** negli analogici non erano memorizzabili).

Distinuguiamo due tipi di campionamento:
- quello **in tempo reale** che avviene alla frequenza di campionamento
- quello **in tempo equivalente** che si usa solo per segnali ripetuti (periodici)

Questa seconda tecnica si attua attivando ripetutamente il trigger e far partire l'acquisizione con dei ritardi crescenti; dopo aver ottenuto un numero di campioni sufficienti (pari al record lenght) possiamo ricostruire il segnale.

![[Pasted image 20220929200832.png]]

Secondo questa tecnica si ottiene un campionamento a frequenza: $$f_{c_{eq}}=\frac{1}{\Delta t} > f_c$$
L'inconveniente di questa tecnica è l'elevata durata del processo di acquisizione.

### DSO multicanale 

Assumiamo ora di voler collegare più segnali al nostro oscilloscopio, ottenendo di fatto un DSO **multicanale**:
- Se vogliamo una soluzione a **basso costo** è utile usare un convertitore condiviso per tutti i canali, e $N$ circuiterie di ingresso, il tutto unito da un **multiplexer** che decide il canale di ingresso. La frequenza di campionamento nominale è di $f_{c_1}=\frac{f_c}{N}$.  In realtà la frequenza è minore di questa quantità per problemi di *settling time*.I campioni ottenuti non sono simultanei.

![[Pasted image 20221002190607.png]]
- La soluzione ad **alte prestazioni** prevede una catena completa (compresa di ADC) per ogni canale in ingresso (molto costoso).

### Capacità di trigger

Negli oscilloscopi moderni oltre alle classiche modalità di trigger (slope, source, coupling, level ) ci sono anche delle condizioni di **trigger multicanale** ovvero una sorta di AND logico fra i trigger singoli dei canali (come nell'esempio).

![[Pasted image 20221003212042.png]]

Lo schema finora realizzato permette di visualizzare il segnale solamente in **post-trigger**.

### Parte 'lenta' dello schema 1

Compito del microprocessore è l'elaborazione dei segnali e le seguenti funzionalità:
- ricostruzione del segnale in ingresso (interpolazione).
- stima dei parametri in ingresso
- DFT, analisi spettrale

I punti dell'oscillogramma sono trasferiti in memoria video, organizzata come una matrice di HxL elementi. 
Il pennello elettronico scorre da sinistra a destra e attiva i pixel corrispondenti alla matrice.
C'è corrispondenza fra la memoria video e il display effettivo dell'oscilloscopio.

### Modalità di acquisizione

- **Sample Mode**: i waveforms points sono costruiti a partire dai sample points, con periodo di campioanmento $t_{c}$  pari al periodo dei punti dell'oscillogramma.
$$
t_{c}=t_{w}=\frac{(TimeDiv)10}{N_{w}}
$$
Si noti che $N_{w}$ è definito dal costruttore e corrisponde al numero di waveforms points sullo schermo.

- **Average mode**: anche in questa modalità $t_{c}=t_{w}$ ma il campionamento non è unico, se ne fanno alcuni andando così ad agire sul problema della riduzione del rumore senza perdere banda passante. E' applicabile a segnali ripetitivi.

- **Peak-detect mode**: la frequenza di campionamento è la massima possibile indipendenti dal time/div. I punti del waveforms sono ottenuti dal minimo e dal massimo dei sample points ottenuti in due intervalli $t_{w}$. Così facendo è più probabile individuare **glitch** o picchi di segnale.

- **Envelope mode**: modalità simile al peak- detect. I minimi ed i massimi waveform points di acquisizioni successive sono utilizzati per costruire un oscillogramma che mostra gli inviluppi minimo e massimo del segnale in misura.

- **High resolution mode**: Simile al peak-detect. Ciascun waveform point è ottenuto come valor medio dei sample points corrispondenti ad un waveform interval tw. Questa modalità permette di ridurre il rumore e migliorare la risoluzione nel caso di segnali con variazioni lente rispetto alla frequenza di campionamento


# Schema operativo 2
---
Principali differenze rispetto allo schema 1:
-   ADC opera sempre in **free running** ovvero alla massima frequenza di campionamento.
-   l'avvio del processo di acquisizione non è più determinato dal trigger
-   A valle del ADC c'è un decimatore di campioni (in base a ciò che devo fare).
-   La memoria di acquisizione è un buffer circolare.

![[Pasted image 20221003214025.png]]

Nel buffer il puntatore in scrittura lavora alla velocità dell'ADC.
Il puntatore in lettura è usato per scandire i campioni da trasferire alla memoria interna.
Quest'ultimo puntatore è comandato dal trigger.
A seconda del puntatore in lettura si può ottenere una modalità di output post, pre e postpre trigger.

A causa del free-running non è possibile usare la tecnica del campionamento sequenziale.
Si usa al suo posto il **random-sampling** (in questa modalità si usa l'evento di Trigger come riferimento temporale per determinare la posizione dei campioni, in pratica se prima il ritardo era noto, ora viene calcolato).
A differenza del sequenziale, il random sampling non garantisce l'acquisizione di campioni diversi dalle varie ripetizioni del segnale. Nel caso limite prendo sempre lo stesso valore e costruisco una continua.

Come posso accorgermi di fenomeni di **aliasing**?
Osservando forme d'onda non congruenti al variare della configurazione della base tempi.

# Collegamento con cavo coassiale
---
Data la struttura del cavo coassiale (conduttore, dielettrico, conduttore) di fatto è come inserire un **condensatore** in parallelo che collega il circuito al DSO.

![[Pasted image 20221005124431.png]]

Valori tipici: $R_{IN}=1M\ohm$ e $R_{g}=50\ohm$
Quando collego e c'è una corrente che scorre ottengo di fatto un filtro passa-basso con la sua relativa frequenza di taglio (ricordando che per un filtro passivo si ha $f_{t}=\frac{1}{2\pi\tau}$):
$$
f_{t} \approx\frac{1}{2\pi \cdot R_{g} \cdot(C_{d}+C_{IN})}
$$
Ho rilevato effetti di carico che dipendono sia dal circuito in misura che dal DSO ma anche dal cavo stesso.

I cavi che useremo saranno del tipo **RG-58** caratterizzati da una resistenza di $50\ohm$
e capacità $100 \frac{pF}{m}$

# Collegamento con sonda compensata
---
Il problema del cavo coassiale è che si ha già attenuazione del segnale prima di arrivare all'oscilloscopio (collo di bottiglia), per ovviare a ciò cerchiamo di 'compensare' questa perdita tramite una sonda.

![[Pasted image 20221005124954.png]]

La sonda compensata si basa sul principio del **partitore compensato**.
Una relazione da verificare è quella dell'uguaglianza delle costante $\tau$ di tempo:
$$
R_{s} \cdot C_{s}=R_{IN} \cdot(C_{IN} +C_{d})
$$

Allora se quest'ultima è vera possiamo ottenere una sonda solamente 'resistiva' definita dalla relazione:
$$
\frac{V_{IN}}{V_{G}}=\frac{R_{IN}}{R_{IN}+ R_{S}}
$$
Come per il caso del cavo coassiale, il collegamento va a definire un filtro passa basso con frequenza di taglio pari a:
$$
f_{t}=\frac{1}{2  \pi R_{g} C_{eq}}
$$
Per regolare $C_{eq}$ occorre ricorrere all'onda quadra, modifichiamone il valore con una manopola fino a quando l'onda quadra appare senza distorsioni sullo schermo: a quel punt significa che la sonda sta compensando.

Un comando importante nel menu del canale è il comando **Probe**, con una serie di scelte annesse. Occorre selezionare la voce corretta in funzione del collegamento che si va a fare:
- scegliamo **x1** se usiamo un cavo coassiale
- nel caso di sonda compensata scegliamo **x10** (sonda x10)

Come mai preferiamo modulare la capacità della sonda, rispetto alla resistenza?
- si preferisce modifica la capacità perchè dalla resistenza dipende la compensazione
- La resistenza si usa per verificare il fattore 1:10 della somma.


# Scala di misura
---
Lo schermo dell'oscilloscopio è un rettanggolo da 10 divisioni in orizzontale e 8 in verticale.
Non è quadrato per movtivi storici: a causa dello spostamento del pennello.

![[Pasted image 20221005132403.png]]

Due possibilità per effettuare misure dall'oscilloscopio:
- Il menu **measures** selezionabile con pulsanti sul DSO, può selezionare parametri da misurare automaticamente; il problema è l'assenza delle incertezze associate
- Per misure a cui interessa l'incertezza usiamo la scala (ogni tacca 0.2 divisioni)

Esempio, misurazione della tensione picco-picco:

![[Pasted image 20221005132838.png]]

$y_{2}=3.6 div$     $y_{1}=-3.4div$  ottengo ($K_{v}=0.5$) $V_{pp}=3.5V$
Ora devo applicare la legge di propagazione
$V_{pp}=L_{pp}K_{v}=(y_{2}-y_{1})K_{v}$
$\delta V_{pp}=\frac{ \partial V_{pp} }{ \partial y_{2} }\delta y_{2}\dots$
Ottengo
$\delta V_{pp}=K_{v}(\delta y_{1}+\delta y_{2})+\delta K_{v}(y_{2}-y_{1})$
Risultato analogo se uso i casi notevoli e qualche passaggio algebrico.
Le incertezze sulle letture di $y_{1}$ $y_{2}$ $K_{v}$ è basato sulla risoluzione dell'oscilloscopio.
In condizioni normali (senza rumore o altro) è l'operatore se decide in quale metà della divisione va il segnale, dunque di norma teniamo la risoluzione di lettura sul display pari a 0.1.

Possimao dunque considerare l'**incertezza in lettura pari alla metà della risoluzione**.
Per l'incertezza sul fattore di taratura verticale, è il costruttore che la fornisce (nella forma ridotta)
$$
\epsilon_{K_{v}}=\frac{2}{100}Full Scale
$$
Faccio i calcoli inversi con la definizione dell'incertezza ridotta.

Altro esempio: misurazione della base tempi

![[Pasted image 20221005134454.png]]

Ragionamenti analoghi a prima

Altra esempio: misurazione di rapporti di tensione(intervalli di tempo)

![[Pasted image 20221005134538.png]]

Nell'esempio cerco il rapporto $\frac{x_{1}-x_{2}}{x_{2}-x_{3}}$ ovvero il **duty cicle**.
Come calcolarne l'incertezza?
Assumiamo come nel caso precedente che le incertezza siano uguali e pari a 0.05.
Non posso ricorrere ai casi notevoli, ovvero non posso sommare le incertezze, perchè il numeratore e denominatore dipendono da una stessa variabile, dunque non sono indipendenti.
Uso la regola delle derivate parziali.

[[Incertezze, modello deterministico]]