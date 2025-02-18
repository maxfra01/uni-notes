![[Pasted image 20230516182210.png]]

# Circuiti di protezione
---
Devono evitare che delle scariche elettrostatiche danneggino i circuiti.
Normalmente vengono impiegati **diodi** che non permettono di limitare la tensione di ingresso:

![[Pasted image 20230516182805.png]]

La resistenza che è presente nei circuiti limita la corrente nella fase di protezione.

# Amplificazione
---
Si può usare un amplificatore single ended non invertente:

![[Pasted image 20230516183355.png]]

Il guadagno è pari a $A_{v}=1+\frac{R_{1}}{R_{2}}$

Una seconda opzione è quella di usare un amplificazione differnziale, molto iù protetto dai disturbi.
Vogliamo in particolare amplificare il modo differenziale ma attenuare il modo comune: vogliamo introdurre allora il parametro **CMRR**:
$$
CMRR=\frac{A_{d}}{A_{c}}
$$
Vogliamo perciò che il Common Mode Rejection Ratio deve essere tipicamente elevato. Spesso è espresso in Decibel.

![[Pasted image 20230516184005.png]]

Affinchè venga mantenuta la simmetria delle resistenze occorre disaccoppiare il generatore da quello che sta a valle e per farlo usiamo dei voltage follower:

![[Pasted image 20230516184649.png]]

E' possibile poi inserire delle resistenze per far avere ai follower un guadagno positivo che agisce solo sul segnale differenziale (e non sul modo comune).

# Filtro Anti Aliasing
---
![[Pasted image 20230517114624.png]]

Supponiamo di avere una frequenza massima pari a 12 kHz, scegliamo di campionare a 50kHz, anche se per il criterio di Nyquist sono sufficienti 24kHz.
$$
f_{B}=12kHz \quad f_{s}=50kHz
$$
Voglio perciò imporre attenuazione a partire da 12 kHz e si ha attenuazione massima in $f_{s}-f_{B}=38kHz$ 

Vogliamo imporre una attenuazione pari al rapporto segnale rumore di aliasing: $SNR_{A}$
Più poli $P$ presenta la fdt del filtro, maggiore è l'attenuazione che riusciamo ad ottenere. In particolare si scende di 20dB per ogni decade di frequenza.
Per ottenere una stima del numero di poli possiamo calcolare:
$$
P=\frac{SNR_{A}}{A_{p}}=\frac{SNR_{A}}{20\log_{10}\frac{f_{s}-f_{B}}{f_{B}}}
$$
### Funzione di trasferimento

Possiamo solo ottenere approssimazioni che realizzano circa il taglio di un filtro ideale.
Mettendo in cascata celle del primo e del secondo ordine possiamo ottenere qualsiasi polinomio a coefficienti reali.

![[Pasted image 20230517120657.png]]

### Tipologie di filtri

Possono essere distinti in due grandi famiglie:
- Analogici, come filtri passici (LC (R)), filtri attivi (realizzati con operazionali e celle RC) e filtri a capacità commutate
- Digitali, che richiedono convertitore ADC e DAC e richiedono inoltre capacità di calcolo e memorie esterne. Sono realizzate con FF, FPGA $\mu$P

### Tipi di approssimazione

Realizziamo la funzione di trasferimento come rapporto fra 2 polinomi.
Gli schemi mostrano a sinistra l'andamento della banda passante, mentre a destra c'è la banda attenuata.

Una prima approssimazione è il **tipo Bessel** che come caratteristiche ha:
- Fase lineare
- Non ci sono distorsioni
- Non ci sono ondulazioni in banda passante

![[Pasted image 20230517121122.png]]

Un secondo tipo di approssimazione è tipo **Butterworth**:
- E' presente distorsione
- Nessuna ondulazione in banda passante

![[Pasted image 20230517121207.png]]


Un terzo tipo è **Chebicheff** :
- Ondulazione in banda passante
- Più ripido di tutti (forti attenuazioni in banda)

![[Pasted image 20230517121327.png]]

```ad-note
Numerosi tool online per il progetto del filtro, a partire da specifiche: FILTERCAD, reperibile a www.linear.com
```

### Filtri SC

Sono filtri che facendo uso di condensatori emulano il funzionamento delle resistenze.

![[Pasted image 20230517123827.png]]

Lo switch commuta con una frequenza $f$ nota, quindi facendo questo procedimento periodicamente andiamo a simulare la corrente all'interno di una resistenza.

Volendo realizzare un filtro con la tecnica SC:

![[Pasted image 20230517124222.png]]

Posso sostituire la resistenza con la struttura con interruttore vista sopra, ottenendo un circuito che è un passa basso.
Vantaggio: il filtro con SC non dissipa potenza in modo statica, perchè non c'è corrente nelle resistenze.

