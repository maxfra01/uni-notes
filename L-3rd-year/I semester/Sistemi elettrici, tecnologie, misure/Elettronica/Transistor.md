 
# Transistori MOS
---
Oltre a questa tipologia esiste anche il **BJT (Bipolar Junction Transistor)**.

### Concetti generali
Nella pratica un **Trans**fer Res**istor** è un doppio bipolo non lineare unidirezionale, in cui individuaimo una porta di controllo e una porta controllata.
Corrisponde alla minima unità funzionale in ogni sistema elettronico.

![[Pasted image 20221024094429.png]]

### pMOS vs nMOS

![[Pasted image 20221024094840.png]]

Distinguiamo sempre 4 terminali nei transistori MOS:
- SOURCE
- DRAIN
- GAIN
- BULK
Il terminale BULK è solitamente cortocircuitato al SOURCE oppure collegato a una tensione costante; in ogni caso nel corso consideriamo il cortocircuito e quindi ci limitiamo a 3 terminali.

### Caratteristiche statiche
Possiamo individurare due porte, **Source-gate** e **Drain-Source**.
La gain-source (GS) è la porta di ingresso ed $v_{gs}$ è detta variabile indipendente.
Quella dipendente è la corrente che scorre nella porta di uscita $i_{d}$.

Per la porta di ingresso abbiamo la seguente caratteristica:
$$
i_{G}=i_{G}(v_{GS},v_{DS})=0 \quad \forall v_{GS}, \forall v_{DS}
$$
Da cui deduciamo che la potenza assorbita per la porta in ingresso è sempre nulla.
$$
P_{in}=v_{GS} \cdot i_{G}=0
$$
### Caratteristiche d'uscita
Sappiamo che l'uscita è dipendendente dalla variabile di ingresso $v_{GS}$

Fintanto che la tensione $v_{GS}$ non supera una certa soglia, il transistore mos si comporta come un circuito aperto e non dissipa potenza.
L'area in cui si verifica questo fenomeno è detta **Regione di interdizione**.

Mentre quando è rispettata la relazione $v_{GS} > v_{TH}$ AND $v_{DS}<v_{GS} - v_{TH}$
Allora posso dire che mi trovo nella **Regione di triodo** e la relazione all'uscita del transistore è:
$$
i_{d}=\beta \cdot v_{ds} \cdot\left( v_{gs}-v_{th}-\frac{v_{ds}}{2} \right)
$$
![[Pasted image 20221025090157.png]]

Notiamo che se $v_{gs}-v_{th} \gg v_{ds}$ allora la relazione è quasi lineare e si comporta come un resistore:
$$
R_{ON}=\frac{1}{\beta(v_{gs}-v_{th})}
$$
L'ultima regione del grafico è detta **Regione di saturazione**:
$$
I_{d}=\frac{\beta}{2} (v_{gs}-v_{th})^2(1+\lambda v_{ds})
$$
![[Pasted image 20221025090651.png]]

### Safe Operating Area (SOA)
Un transistor può andare incontro a un danneggiamento se le tensioni nelle porte eccedono un certo valore, oppure se la corrente che scorre supera un certo valore, o ancora se la potenza dissipata (significativa in saturazione) supera una soglia.

Definiamo allora la SOA, che è una piccola area del grafico analizzato di sopra.

![[Pasted image 20221025092535.png]]

### Transcaratteristica statica
Rappresentiamo la transcaratteristica fra $v_{gs}$ e $i_{D}$

![[Pasted image 20221025093119.png]]

Versione del pMOS su slide.
