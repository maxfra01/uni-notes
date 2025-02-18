# Componenti standard
---
Ci sono diversi tipi di circuiti programmabili:
- **Memoria**, come ROM, RAM, PROM, EPROM, EEPROM
- **Funzioni logiche** realizzate con tabelle di **look-up**, multiplexe...
- **Celle di I/O**
- **Interconnessioni**

# Programmable Logic Array PLA
---
Sono composte da una matrice di invertitori (NOT), porte AND e OR.
Siamo quindi in grado di realizzazre qualsiasi funzione logica, ad esempio....

![[Pasted image 20230321172055.png]]


# Field Programmable Gate Aarray FPGA
---
Sono circuiti programmabili composti da molte celle logiche programmabili ceh possono realizzare numerose funzionni logiche:
Sono inoltre presenti celle di IO, e interconnessioni.

![[Pasted image 20230321172339.png]]

### Look-up Table LUT

Sono tabelle mantenute in memoria principale (RAM) dove è possibile ricavare istantaneamente il valore di funzioni logiche arbitrarie.

Per costruire una LUT prima individuiamo la funzione che vogliamo realizzare e ne descriviamo la tabella di verità, L'uscita della tabella va memorizzata in una celle di RAM, collegando un multiplexer e pilotandolo con gli ingressi della funzione logica stessa possiamo costruire la LUT:

![[Pasted image 20230321180908.png]]

Un **Tipico blocco di una FPGA** è formato dalle seguente componenti:

![[Pasted image 20230321181505.png]]

Il multiplexer finale occorre per decidere quale output prendere, se dal FF o dal latch.

### Architetture FPGA recenti

- **Interconnessioni migliorate** (routing)
- **Blocchi per funzioni speciali** come sommatori, moltiplicatori, Processori programmati con LUT, SoC, CPU dedicate, controllori e periferici.
- **Parametri elettrici programmabili**.

