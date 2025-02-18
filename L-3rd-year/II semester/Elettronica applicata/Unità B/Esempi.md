# Esercizio Be1.1
---

![[Pasted image 20230314175833.png]]

Per il passo 1, fare la rete di pull-down ricordando che una serie diventa un parallelo e viceversa.

Per il passo 2.1 per calcolare t_rise e t_fall fissiamo delle soglie, tipicamente al 10% e al 90% dell'escursione complessiva di E.
Quindi il t_rise è il tempo per passare dal 10 al 90%
Il t_fall per passare dal 90 al 10%.
Occorre inoltre l'espressione analitica di E, serve dunque uno schema circuitale che modellizzi la porta logica.
L'inverter in uscita ha una capacità equivalente pari alla somma delle capacità dei due transistor che compongono l'inverter.
Questa capacità fungerà da carico e l'espressione analitica di E dipenderà proprio da questa carica/scarica.
Il tempo di scarica è rappresentata dal **percorso più lungo** che fa la corrente per scaricarsi a terra.
Il tempo di carica è rappresentato dal **percorso più lungo** dall'alimentazione al condensatore di carico.
Possiamo considerare le costanti di tempo per la salita e la discesa:
$$
\tau_{rise}, \tau_{fall}
$$
Per calcolare il tempo di salita e discesa  **SOLO PER RAMI DI ESPONENZIALE** è pari a:
$$
t_{rise}=\ln\left( \frac{90}{10} \right) \cdot \tau_{rise}
$$
Posso determinare il percorso più lungo e più corto per calcolare il tempo di salita massimo e minimo.

Per il punto 2.1 dobbiamo trovare i ritardi massimi e minimi associati al circuito, sapendo che per l'inverter si ha: $V_{T} \to (0.75,1,25)V$ e all'uscita OUT sono collegati altri 10 inverter in parallelo.
La capacità equivalente in uscita sarà data dalla somma di tutte le capacità per ogni inverter.


# Esercizio Be1.2
---
![[Pasted image 20230315113811.png]]

Normalmente vogliamo una funzione da realizzare che sia interamente negata, dunque per ricordurci a quella forma desiderata usiamo una doppia negazione e poi applichiamo de Morgan:
$$
U=A' \cdot B' +(C \cdot D)'=(A' \cdot B' +(C \cdot D)')''= ((A+B) \cdot C \cdot D)'
$$
A questo punto costruiamo la rete di pull-up e pull-down e colleghiamo il carico all'uscita.

Per calcolare i tempi di salita e discesa, costruiamo il modello con resistenze e interruttori. Poi dobbiamo fissare le soglie al 10 e 90%, determinare i percorsi più lunghi e più corti per il tempo massimo e minimo: calcoliamo la costante di tempo $\tau$ tenendo conto della resistenza equivalente che costituisce il percorso.
Infine calcoliamo il tempo di salita o discesa come:
$$
t_{rise}=\ln(9) \cdot \tau
$$


# Esercizio Be1.3
---
![[Pasted image 20230315120132.png]]

Per analizzare il ritardo del circuito sommiamo i ritardi che vanno a percorre il percorso più lento.
Inoltre dobbiamo tenere conto del correttto funzionamento dei Flip-flop.
$t_{porte}=3.15+2.2+1.05+2.1=8.5 ns$
$$
T=t_{porte}+t_{SU}=10ns \implies f_{max}=\frac{1}{10ns}=0,1GHz
$$


# Esercizio be1.4
---
![[Pasted image 20230315121452.png]]


# Esercizio Be1.5
---
![[Pasted image 20230315135413.png]]

Per calcolare la frequenza/periodo devo sommare i semiperiodi dei rami di esponenziale, dunque per il tratto in salita:
$$
V_{s_{2}}=V_{UH} +(Vs_{1}-V_{UH})e^{-T_{1}/\tau}
$$
Mentre per il tratt in discesa
$$
V_{S_{1}}=V_{UL}+(V_{s_{2}}-V_{UL})e^{-T_{2}/\tau}
$$
Dunque ricavo che:
$$
T'=\tau\ln\left[ \frac{V_{UL}-V_{S_{2}}}{V_{UL}-V_{S_{1}}} \right] \quad T'=\tau \ln\left[ \frac{V_{UH}-V_{S_{2}}}{V_{UH}-V_{S_{1}}} \right]
$$
Dopodichè semplicemente:
$$
T=T'+T'' \implies f=\frac{1}{T}
$$



# Esercizio Be2.1
---
![[Pasted image 20230405123018.png]]

Ricordiamo che il costo di un prodotto è la somma del costo unitario più un secondo contributo di costi non ricorrenti per pezzo:
$$
C_{P}=C_{u}+\frac{NRE}{N}
$$
L'azienda vuole guadagnare il 50% , quindi fissa il prezzo $P=2C_{P}$, perciò il costo del prodotto è pari a $C_{P}=450$.
Dato che $N$ è noto allora basta risolvere per NRE e trovo: $NRE=3500000\$$.

Se raddopia allora possiamo calcolare il costo del prodotto con la prima formula e ottengo $C_{P}=800$.
Quindi il nuovo margine di profitto è $99\$$ o in percentuale $1- \frac{C_{P}}{P}$


# Esercizio Be2.2
---
![[Pasted image 20230405124123.png]]

Per prima cosa noto che il minterm $def$ compare in due delle funzioni logiche, dunque sicuramente una lookup table costruirà quel minterm.
Dentro ad esempio alla seconda lookup table, avrò l'AND fra $d,e,f$, e il FF che ne esce non ci interessa dato che è una logica combianatori. Occorre anche chiudere le interconessioni in modo opportuno.

![[Pasted image 20230405124944.png]]

Nella prima LUT metterò il minterm $abc$, chiudente le interconnessioni necessarie.
Ora per fare l'OR e farlo arrivare su u, userò la LUT 4 che appunto come uscita ha u.

![[Pasted image 20230405125135.png]]


# Esercizio Be2.4
---
![[Pasted image 20230405133656.png]]

```verilog
module shift_register(q,clk,reset,ctrl,data);
	parameter N=8;
	input wire clk, reset;
	input wire [1:0] ctrl; //2 bit per realizzare 4 config di ctrl
	input wire [N-1:0] data;
	input wire [N-1:0] q;
	reg [N-1:0] s_reg, s_next;
	always @(posedge clk or posedge reset) begin
		if (reset):
			s_reg <=0;
		else if (clk)
			s_reg <= s_next
	end
	always @(*) begin
		case (ctrl):
			0: s_next= s_reg; //no operation
			1: s_next={data[N-1], s_reg[N-1]}; //right shift
			2: s_next={data[N-2:0], data[0]} //left shift
			3: s_next=data; //load data
		endcase
	end
	assign q=s_reg;
endmodule	
```

# Esercizio Be2.5
---
![[Pasted image 20230405134704.png]]

Per fare questo decoder, avrò 8 porte AND, ognuna per pilotare una wordline.
Per pilotare 8 wordline bastano indirizzi du 3 bit, quindi 3 bit di ingresso A0, A1,A2

![[Pasted image 20230405135021.png]]

Per determinare il massimo ritardo di attivazione della wordline si vanno a sommare i ritardi delle varie porte CMOS.
Per una qualsiasi worline abbiamo il contributo della porta inverter 1, poi nand e poi inverter 2(ogni wordline è collegata a 64 celle).

Per l'inverter 2 abbiamo che il ritardo associato a quella porta è:
$$
t_{inv_{2}}=0.69RC=0.69 (R_{ON}+R_{wl})(64*0.1)=3,27 ps
$$
Per la porta NAND abbiamo 3 transistor interni quindi 3 resistenze R_ON e invece la porta pilota l'inverter quindi vede due capcaità di gate:
$$
t_{NAND}=0.69*RC=0.69*(3*R_{ON})*(2C_{gate})
$$
Per il primo inverter avrò che piloto 8 gate:
$$
t_{inv_{1}}=0.69 * R_{ON }* (8C_{gate})
$$
```ad-info
Il fattore 0.69 è per la soglia di attivazione al 50%
```

Nel punto 2 occorre un predecoder analogo a quello visto prima, che pilota gli enable per i 4 banchi di RAM.
I segnali di Enable vengono mandati a tutti gli AND delle wordline, così solo un banco è attivato.

Per i ritardi ripetiamo i calcoli ma notando che è presente anche il contributo del pre decoder.

# Esercizio Be2.7
---
![[Pasted image 20230405142433.png]]
$$
I=C_{s} \frac{ \partial v }{ \partial t }  \implies I=C_{s} \frac{\Delta V}{\Delta t}
$$
E' nota $\Delta V$ dunque svolgiamo i calcoli e troviamo un tempo pari a:
$$
\Delta t=C_{s} \frac{\Delta V}{I}=80ms
$$

# Esercizio Be2.8
---
![[Pasted image 20230405142638.png]]

Vogliamo che le cariche memorizzate nel gate flottante restino per 10 anni.
Calcoliamo la variazione di carica, corrispondente a una tensione di soglia come:
$$
\Delta Q=C_{pp}\Delta V_{TH}=50aC
$$
A questo punto calcoliamo la corrente come segue, dove il tempo è pari a 10 anni in secondi:
$$
I=\frac{\Delta Q}{\Delta t}
$$

# Esercizio Be2.9
---
![[Pasted image 20230418163400.png]]

Se si fanno troppe cancellazioni si rovinano le celle flash.
Nel primo scenario non c'è wear leveling, dunque si cancellano e si programmano sempre i soliti 200 blocchi.

Mediamente ogni file viene scritto ogni 10 minuti, su 50 blocchi, in un totale di 200.
Mediamente dunque scrivo un blocco ogni $\frac{200}{50}\cdot 10$ minuti ovvero $40$ minuti
Il tempo che impiegheremo a raggiungere la soglia è $10^4 \cdot 40$ minuti che sono 278 giorni, dunque meno di un anno.

Con il Wear leveling invece abbiamo 4096 blocchi.
Mediamente un blocco viene scritto ogni $\frac{4096}{50} \cdot 10=819,2$ minuti.
A questo punto il tempo per far si che la memoria diventi inaffidabile è:
$819,2 *10000$ minuti ovvero 5689 giorni (> 15 anni).

```ad-note
- Calcolare il tempo medio ogni quanto viene scritto un blocco
- Moltiplicare per il numero di cicli
```

