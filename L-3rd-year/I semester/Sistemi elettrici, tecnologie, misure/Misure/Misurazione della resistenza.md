# Metodo volt-amperometrico
---
Cerchiamo di costruire punto per punto la caratteristica V-I di un bipolo.
Sostanzialmente consiste nell'applicazione della legge di Ohm.
Necessito di un voltmetro e amperometro.
Abbiamo un bipolo con resistenza incognita, cerchermo di misurarla seconda la relazione
$$
R=\frac{V}{I}
$$
A seguire lo schema della misurazione: 

![[Pasted image 20221005140111.png]]

Il modo più rapido per misurare l'incertezza è dato da:
$$
\epsilon_{r}=\epsilon_{v}+\epsilon_{i}
$$
che rappresentano l'incertezza relativo del voltmetro e l'amperometro.
Ma dobbiamo tenere conto dell'effetto del carico strumentale.

## Effetto del carico strumentale

Lo schema visto sopra diventa, in termini reali assume due diverse forme, a seconda di posizionare il voltmetro a valle dell'amperometro o a valle.

### Voltmetro a valle

![[Pasted image 20221005140624.png]]

Consideriamo il caso del voltmetro a valle (dopo l'amperometro), quest'ultimo misura veramente la tensione ai capi del bipolo, ma la corrente invece corrisponde alla somma di quella del bipolo e quella del ramo del voltmetro.
Svolgendo i calcoli quello che misuro dall'amperometro è la quantità del parallelo tra la resistenza incognita e quella del voltmetro.

In sostanza misuro una quantità sbagliata seppur di poco ($R_{v}$ è molto grande):
$$
r=R \cdot \frac{R_{v}}{R+R_{v}}
$$
Se ne calcolo l'incertezza assoluta dovuta all'effetto sistematico:
$$
\delta_{CS_{v}}=r-R=-\frac{R^2}{R+R_{v}}
$$
ed in valore relativo:
$$
\epsilon_{CS_{v}}=\frac{\delta_{CS_{v}}}{R} \approx -\frac{R}{R_{v}} \quad R_{v}\gg R
$$
### Voltmetro a monte

![[Pasted image 20221009211325.png]]

Abbiamo prima il voltmetro e poi l'amperometro a valle (che misura esattamente la corrente nel bipolo).
Misurero effettivamente $i=I$ ma $v=V+V_{A}=V+R_{A}\cdot I$
Dunque otterrò la misurazione di una resistenza pari a $$
r=\frac{V+R_{A}I}{I}=\frac{V}{I}+R_{A}=R+R_{A}
$$
Ne calcolo l'incerteza come prima:
$$
\delta_{CS_{A}}=r-R=R_{A} \quad \epsilon_{CS_{A}}=\frac{\delta_{CS_{A}}}{R}=\frac{R_{A}}{R}
$$
### Come minimizzare l'effetto di carico?

Se mettiamo a confronto gli errori relativi possimao valutare quando essi sono uguali (in valore assoluto) per ottenere una misura di riferimento, poi potrò valutare quando è più conveniente misurare con il voltmetro a valle o a monte.

La condizione di uguaglianza dà come risultato: $$
R=\sqrt{ R_{A} \cdot R_{v} }
$$
Ora se ricadiamo nel caso di $R$ minore di quella quantità conviene **avere il voltmetro a valle dell'amperometro**, nel caso opporto servirà a monte.

Dalle formule precedenti posso ottenere le formule per la correzione dell'effetto di carico:
$$
r=\frac{v}{i-\frac{v}{r_{v}}} \quad 
$$

# Resistenze di contatto e del cavo
---
Dobbiamo anche tenere conto della resistenza dei cavi $R_{w}$ e alle resitenze di contatto $R_{c}$ , ovvero quelle che nascono nei punti di contatto di, ad esempio, due fili in un circuito.
Quest'ultime dipendono da numerosi fattori, quali area del contatto, sporcizia, ossidazione, saldatura, forza con cui si mantiene il contatto ecc...

I contatti che terremo in considerazione (fatto bene)  ha resistenza di contatto non inferiore a decine di millohm.
L'incertezza è da valutare confrontandola non con la resistenza da misurare ma bensì con l'incertezza su quest'ultima.
Quando incontro un'incertezza non trascurabile abbiamo due strade:
- Correzione matematica per compensare l'errore
- Modifiche hardware per minimizzarlo
Si preferisce questa seconda opzione con la tecnica dei **quattro morsetti**:

![[Pasted image 20221010090521.png]]

Essendo il voltmetro dotato di una resistenza interna molto alta, allora c'è una compensazione delle resistenze di cavi e contatti.

Le resistenze campioni sono spesso costruiti a quattro morsetti, quelli più grandi ad uso amperometrico e quelli piccoli ad uso voltmetrico.
Se vogliamo invece realizzare noi i collegamenti a quattro morsetti, laddove non è previsto, allora si aggiunge un'incertezza dovuta all'operatore, in quanto la riproducibilità dell'esperimento è complessa.


# Altri contributi di incertezza
---
Si ricorda di fornire specifiche riguardo lo stato dell'esperimento, ovvero equivale a fornire informazioni sulla corrente di prova e sulla temperatura.

Vedi anche:
[[Incertezze, modello deterministico]]