# Tipologie di segnale
---
- Analogico: le grandezze fisiche che misuriamo si esprimo come funzioni continue di variabili continue $$ x(t)=f(t)$$
- Digitale: le grandezze assumo solo quantità discrete di valori, si rappresentano dunque come funzioni discrete di variabili discrete $$ x[kt]=f[ kT_c ]$$
Perchè scegliere il segnale digitale rispetto all'analogico?
- Sono segnali più semplici da elaborare per un calcolatore
- sono facilmente memorizzabili in volumi ridotti
- facilmente trasmissibili a distanza


# Le fasi della conversione
---
Per convertire correttamente un segnale analogico in uno digitale distinguiamo 3 fasi:
- Il **campionamento 
- La **quantizzazione
- La **codifica** (non di grande interesse nel corso, usiamo il binario)

## Campionamento

Il campionamento è di fatto la raccolta dei dati a intervalli di tempo scanditi dalla **frequenza di campionamento**.
Di fatto il **Full Range** dei valori viene diviso in intervalli e vi si associa lo stato discreto più vicino.

![[Pasted image 20220928190103.png]]


Poichè il campionamento avviene in istanti discreti ben precisi facciamo uso di un treno di impulsi dipendente dalla frequenza di campionamento per tener conto solo dei valori che ci interessano: $$ s(t)=\sum_{k=-\infty}^{\infty}\delta(t-kT_c)$$
Basta poi semplicemente molitplicare i valori ottenuti del segnale per il treno di impulsi per ottenere il risultato desiderato: $$x[kT_c]=x(t)\sum_{k=-\infty}^{\infty}\delta(t-kT_c)$$
Ripetendo i passaggi nel dominio della frequenza si ottiene un risultato analogo: $$X_c(\omega)=X(\omega)*S(\omega)=\omega_c\sum_{k=-\infty}^{\infty}X(\omega-k\omega_c)$$
Come decidere la frequenza di campionamento?
La soluzione dipende dalla modalità di interpolazione e dalla precisione che vogliamo ottenere:
- se vogliamo un tipo di interpolazione **troncata (sync)** allora cerchiamo valori che rispettino la condizione $10>\frac{f_c}{f_{max}}>2.5$
- per l'interpolazione **lineare** allora $20>\frac{f_c}{f_{max}}>10$
- per la **dots mode** allora: $\frac{f_c}{f_{max}}>20$


## Quantizzazione
Processo per il quale si assegna uno stato codificato al segnale in ingresso: questo processo è applicato tramite la **caratteristica ingresso/uscita** di un **convertitore analogico digitale (ADC)**.
Un po' di definizioni:
- Tensione di Full Range: intero campo di misura (in pratica l'intero intervallo).
- Tensione di quantizzazione: Ampiezza di ciascun gradino del convertitore, indica anche la **risoluzione**.
- Transizione: punto dove il codice cambia
- Numero di Bit: caratteristica del convertitore (in pratica dagli 8 ai 12)
$$V_q=\frac{V_{FR}}{2^{N_{b}}}$$ 
Ora ci poniamo il seguente problema: come possiamo risalire a una tensione di ingresso dato il codice che la rappresenta?
La caratteristica del convertitore non è invertibile, dunque è possibile solamente stimarla. Prendiamo in considerazione la retta che minimizza gli scarti dalla caratterstica ideale dei gradini.
Possiamo dunque stimare la tensione in ingresso come: $$ \hat{V}_{in}=f^{-1}(n)=nV_q$$
Siccome non è possibile conoscere la tensione in ingresso vado a commettere un **errore di quantizzazione** pari a $$\delta e_q=0.5V_q$$
perchè nel peggiore dei casi sforiamo dall'intervallo a destra e sinistra per metà del suo valore.
L'errore relativo aumenta al diminuire della tensione in ingresso:$$\epsilon e_q=\frac{0.5V_q}{V_{in}}$$
**Conviene lavorare con una tensione di ingresso vicino alla portata massima del convertitore**

## Tempo di conversione
E' il tempo che intercorre tra l'istante in cui si preleva un campione del segnale e l'istante in cui il codice è disponibile all'uscita del convertitore.

Per questo tempo il valore di tensione dovrebbe rimanere costante, in pratica non deve oscillare di
$\pm\frac{V_q}{2}$
Per mantenere il segnale circa costante si fa uso di un circuito analogico **Sample-and-hold**

![[Pasted image 20220928192156.png]]



# Tipologie di errori
---
### Errore di fuori zero
offset presente tra la caratteristica nominale e quella reale dell'ADC.
Il costruttore indica in LSB il valore $\pm E_{0max}$ che corrisponde al massimo valore di fuori zero per tutta la famiglia di dispositivi.

Ne deriva un'incertezza corrispondente di: $$\delta V_{in}=E_{0max}V_q$$

![[Pasted image 20220929142256.png]]


### Errore di guadagno

E' l'errore che si commette a causa della differenza fra l'intervallo di quantizzazione reale $V_q'$ e quello nominale $V_q$ 
Come per l'errore di fuori zero, anche in questo caso il costruttore fornisce l'errore in LSB nella forma $\pm E_{gmax}$ per tutti i dispotivi della stessa famiglia.

Ne deriva un'incertezza nel caso di campo di misura unipolare pari a:
$$\delta V_{in}=E_{gmax}\frac{n}{2^{N_b}}V_q$$
E nel campo di misura bipolare pari a: $$\delta V_{in}=E_{gmax}\frac{n}{2^{N_b-1}}V_q$$

![[Pasted image 20220929142531.png]]


### Errore di linearità

Errore dovuto alla **non uniformità** degli intervalli di quantizzazione.
Facciamo una distinzione fra:
- errore di linearità integrale
- errore di linearità differenziale

Il primo tipo si usa nelle misure di tensione assoluta, mentre il secondo tipo lo si utilizza principalemente in contesto di trasferimento della riferibilità.
Per un generico codice $k$ l'errore di linearità integrale indica al differenza tra la tensione di transizione ideale e quella reale.
L'errore integrale è sempre fornito dal costruttore nella forma $\pm E_{Imax}$ e genera un errore:
$$\delta V_{in}=E_{Imax}V_q$$
L'errore di linearità **differenziale** indica la differenza tra l'intervallo di quantizzazione reale associato al generico codice $k$ e la tensione di quantizzazione ideale $V_q$
$$E_d(k)=\frac{A(k)-V_q}{V_q}$$
Le differenze fra i due sono:
- $E_d$ indica lo scostamento dalla linearità di un particolare gradino della caratteristica del convertitore analogico/digitale 
- $E_i$ rappresenta la somma (integrale) degli errori di linearità differenziale lungo la caratteristica del convertitore

![[Pasted image 20220929145116.png]]


### Numero di bit effettivi

Considerando la totalità degli errori possibili (vedere calcoli [[Pasted image 20220929145417.png]]) otteniamo un'incertezza del tipo: $$\delta V_{in}=(A \space  portata + B \space Lettura)V$$
Possimao misurare il numero di bit effettivi come il numero di bit per rappresentare gli $N$ livelli:
$$Nb^E=log_2\left(\frac{2^{N_b}}{2E_q'}\right)$$
dove $E_q'=E_q+E_g+E_0+E_I$ .

---

## Esempio di ADC reale

![[Pasted image 20220929150008.png]]