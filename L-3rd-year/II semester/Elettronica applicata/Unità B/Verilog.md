# Il linguaggio Verilog
---
Con il linguaggio **verilog** si possono definire modelli composti da elementi base e interconnessioni, inoltre è possibile analizzare i modelli sotto diversi punti di vista:
- a porte logiche
- strutturale
- dataflow
- comportamentale

Di natura Verilog è un linguaggio parallelo per sfruttare a pieno le potenzialità dell'hardware: ad esempio nella modellazione a livello di porte logiche tutte le istruzioni sono eseguite simultaneamente, mentre per la modellazione comportamentale si passa a modello sequenziale.

### Tipi di dati Verilog

Il primo tipo di dati è **wire**, necessario per definire le interconnessioni e si dichiara come: `wire <nome>`

Il secondo tipo è il tipo **variabile**, che memorizza un valore, ad esempio il tipo `reg`.

### Vettori di bit

Un vettore di bit in verilog è una rappresentazione di un'informazione parallela. E' possibile definirli con la seguente sintassi: `wire [0:15] a;`
In questo caso è un vettore di 16 bit di nome a e tipo wire; in questa specifico caso il bit 0 è il lsb, mentre il 15 è il msb, ma è possibile fare il contrario.
E' possibile **accedere a una cella del vettore con la tipica notazione a parentesi quadre**.
E' inoltre possibile **accedere a un intervallo come `b[8:11];`**

### Matrici di bit

Aggiungendo un'ulteriore dimensione, è possibile dichiarare un matrice di bit:

```
reg [7:0] a [1023:0];
```

Così facendo si è dichiarata una matrice a costitutia da 1024 elementi ciascuno da 8 bit. Possibile interpretare come 1024 righe e 8 colonne.
E' possibile accedere a un singolo vettore (intera riga) o a una cella specifica con le doppie parentesi quadre\[\]\[\].

### Valori logici in Verilog

I valori logici che possono assumere i bit sono di quattro tipi:
- Valore logico basso 0
- Valore logico alto 1
- Valore di alta impedenza Z/z
- Valore sconosciuto X/x

### Costanti e macro

Le costante si dichiarano nel seguente modo:

```
[dimensione]'[base] valore;
```

Come ad esempio `'16 AD3`

Le Macro sono molto simili al C:
`define <nome> <valore>` con la backtilde davanti a define.

### Moduli e definizioni

La struttura per dichiarare un modulo è la seguente:

![[Pasted image 20230322120417.png]]

Vediamo un esempio pratico

![[Pasted image 20230322120437.png]]

```verilog
module ALU(In1,In2,OpSel,Mode,Cln,COut,Result,Equal);

output [3:0] Result;
output COut;
output Equal;
input Cln;
input Mode;
input [3:0] In1;
input [3:0] In2;
endmodule
```

Oppure a livello di porte logiche modelliamo il seguente schema:

![[Pasted image 20230322120747.png]]

```verilog
module my_gate(in1,in2,out1);
input in1;
input in2;
output out1;
wire w;
and(w,in1,in2);
not(out1,w);
endmodule
```

Oltre a tutte le porte logiche di base esistono le porte tri-state e sono:

![[Pasted image 20230322122308.png]]

E' inoltre possibile notare che in una porta logica possono esserci più di due ingressi, ad esempio: `nand(y,in1,in2,in3,in4)`

### Ritardi

`xor #(2:3:4,5) (s,x,y)`
Nel caso sopra con # si specifica il ritardo di due tipi ($T_{plh},T_{pl}$), inoltre la sintassi a:b:c indica che in media il ritardo si aggira intorno a c ma non esce dall'intervallo $[a,c]$

Con il comando `timescale <n>ns` scelgo l'unità di misura dei tempi di ritardo.
