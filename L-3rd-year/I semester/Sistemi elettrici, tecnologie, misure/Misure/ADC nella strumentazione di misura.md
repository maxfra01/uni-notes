# Convertitore parallelo (spot)
---
Anche detti **flash converter** sono molto diffusi negli oscilloscopi digitali.
Il principio di funzionamento rispecchia la descrizione del processo di quantizzazione ([[Conversione Analogico-Digitale]]).
Le componenti principali sono:
- un partitore resistivo
- una schiera di comparatori di tensione
- una rete di codifica

![[Pasted image 20221010093127.png]]

Necessito di un campo di misura bipolare, e considero quantizzazione uniforme (quindi resistori tutti guali).
Le uscite dei partitori le metto a confronto con i comparatori di tensione (hanno uscite digitali, o 0 o 1), infine il tutto va alla rete di codifica che resitituisce l'uscita a 3 bit.

Principale vantaggio: alta velocità, con tempo di conversione pari a quello del comparatore (più la codifica), otteniamo valori vicini al GSa/s.

Svantaggi: molto costoso, complesso (per 8 bit ho 256 ingressi)

# Convertitori ad approssimazioni successive (spot)
---
Molto diffuso nelle schede a microcontrollore.
Il valore a cui convertire il campione è ottenuto tramite una serie di approssimazione successive.
Componenti:
- un convertitore digitale/analogico (DAC)
- un comparatore di tensione 
- un'unità logica di controllo 

![[Pasted image 20221010094528.png]]

L'unità logica manda al DAC tutti i codici possibili fin quando non si termina, ovvero quando $V_{x} \approx V_{E}$ 
Nel caso unipolare si parte da una tensione $V_{E}=\frac{P}{2}$ , dopodichè si decrementa o incrementa nell'intervallo $\left( 0, \frac{P}{2} \right)$ seguendo una ricerca dicotomica.

Principale vantaggio: elevata risoluzione a **basso costo**
Svantaggi: velocità più bassa rispetto al convertiitore flash, inoltre $V_{x}$ dev'essere mantenuto costante mentre cerco il codice quindi mi occorre il circuito di sample and hold.


# Convertitori a doppia rampa (integrazione)
---
Principio di funzionamento: la tensione ignota $V_{x}$ in ingresso carica un condensatore (fase di **run-up**) dopodichè il condensatore si scarica tramite tensione nota $V_{rif}$ (fase di **run-down**).
Si stima la tensione ignota tramite il rapporto fra i tempi delle due fasi.

![[Pasted image 20221011084453.png]]

### Run-up
Innanzitutto consideriamo una tensione in ingresso costante nel tempo, e dobbiamo garantire che $v_{I}(0)=0$ dunque prima di ogni misurazione si chiude l'interruttore in parallelo al condensatore.
Nella fase di run-up si integra per un tempo fisso $T_{C}$:
$$
v_{I}(t)=-\frac{1}{RC} \int V_{x} \, dt             =- \frac{V_{x}}{RC} \cdot T_{C}
$$
### Run-down
Nella fase di scarica si integra per un tempo ignoto $T_{x}$ finchè non raggiungo la condizione iniziale $v_{I}(T_{x})=0$
Per il bilancio delle cariche ottengo:
$$
v_{I}(T_{c})=- \frac{1}{RC} \int V_{rif} \, dt 
$$
### Misurazione della tensione ignota
Bilanciando i due contributi ottengo l'espressione per la tensione incognita:
$$
V_{x}=V_{rif} \cdot \frac{T_{X}}{T_{c}}
$$
In particolare $T_{c}$ è un multiplo del periodo del clock, dunque la sua determinazione è **sincrona**, mentre per il tempo $T_{X}$ non è possibile (non sempre) che ricada esattamente sul periodo del clock, dunque la sua determinazione è asincrona.
$$
T_{C}=N_{c} \cdot t_{c} \quad \quad T_{X}=(N_{X} \pm 1)t_{c}
$$
Dunque riprendendo la formula di prima abbiamo (eliminando l'incertezza sul clock):
$$
V_{x}=V_{rif} \cdot \frac{N_{X}t_{c}}{N_{C}t_{c}}=V_{rif} \frac{N_{X}}{N_{C}}
$$
E la relativa incertezza relativa:
$$
\epsilon_{V_{X}}=\epsilon_{V_{rif}} + \frac{1}{N_{X}} + \epsilon_{Non\_id}
$$
### Tempo di misurazione
Notiamo che $T_{C}$ è fisso e deciso dall'operatore, $T_{X}$ dipende dalla tensione in misura dunque:
$$
t_{m}=T_{C}+T_{X}=(N_{C}+N_{X})t_{c}
$$
Solitamente prendiamo $V_{rif}=P$ dove $P$ è la portata del convertitore, dunque il tempo in misurazione massimo è pari a $2 \cdot N_{C} \cdot t_{c}$ (che si verifica se misuro una tensione $V_{X}$ pari a $P$)

### Risoluzione di misura 
Considero l'incertezza assoluta sulla tensione ignota allora con i seguenti passaggi trovo la risoluzione:
$$
\delta V_{X}= \frac{V_{rif}}{N_{C}}= \frac{V_{X}}{N_{X}} = \frac{P}{N_{XMAX}}
$$
Quindi la risoluzione è determinata dalla portata del convertitore e dal numero massimo di conteggi.
Inoltre esplicitiamo la relazione fra tempo di misura e risoluzione:
$$
\delta V_{X}= \frac{P}{N_{XMAX}}=\frac{P}{N_{C}}= \frac{2t_{c}P}{t_{mMAX}}
$$
Piccoli valori di risoluzione richiedono lunghi tempi di misurazione.

### Funzionamento in presenza di disturbi

![[Pasted image 20221011091510.png]]

Notiamo che se ho un disturbo alternato di periodo $T_{D}$ l'integrazione fa si che nel caso di $T_{C}=k \cdot T_{D}$ non ci sia alcuna sovrapposizione del segnale con il disturbo.
Invece il massimo effetto del disturbo lo si ha se prendo un numero dispari di periodi, ovvero se $T_{C}=\frac{(2k +1)T_{D}}{2}$

Un disturno impulsivo di area $A$ produce un effetto pari a $\frac{A}{T_{C}}$.

Per impostare il periodo di integrazione possiamo usare **NPLC: Number of Power Line Cycle** ovvero il numero di periodi della tensione di rete.


# Convertitori multi-rampa
---
L'unica modifica è la fase di run-down, dove abbiamo più resistenze per la scarica.

![[Pasted image 20221011092827.png]]
