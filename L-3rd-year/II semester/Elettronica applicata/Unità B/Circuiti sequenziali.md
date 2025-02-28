# Registri
---
Sono costituiti da **insieme di flip-flop** a comandi comuni.
Possono essere di tipo **latch, o edge triggered**.

Nel caso di registri paralleli abbiamo il seguente schema:
![[Pasted image 20230308120719.png]]
Si parla di sistemi PIPO (Parallel-in Parallel-out).

Per i registri a scalamento, le uscite dei flip-flop sono collegati agli ingressi del successivo FFD.
In sostanza abbiamo una cascata di FF tipo D dove $Q_{n}=D_{n+1}$
Nel caso SISO (Serial In- Serial Out) abbiamo lo schema:
![[Pasted image 20230308121259.png]]

Abbiamo poi registri a scalamento di tipo SIPO:
![[Pasted image 20230308121403.png]]

Nei sistemi PISO, esiste un nuovo parametro (**parallel load**) che permette di caricare in parallelo i valori nei FF.

Esiste il registro a scalamento **completo** che implementa sia ingresso e uscita parallelo e seriale.
Ogni FF ha in ingresso un multiplexer che tramite un select decide se prendere input seriale o parallelo.
![[Pasted image 20230308122848.png]]
E'  dunque possibile prelevare l'uscita in parallelo o in serie dall'uscita Q(0).


# Contatori
---
Ulteriormente divisi in contatori **sincroni e asincroni**.

### Divisore modulo 2/4

![[Pasted image 20230308123549.png]]

Ogni stadio divide la frequenza di clock modulo 2.

### Contatore Asicrono

![[Pasted image 20230308124503.png]]

E' possibile realizzare un contatore asincrono a partire dai FF JK:

![[Pasted image 20230308124602.png]]


# Macchine a stati finiti
---
Lo stato del sistema è determinato dal valore dei FF; ad ogni stato corrisponde una combinazione delle uscite.
La rappresentazione più comoda è il diagramma degli stati (grafo).

In generale, la struttura di una macchina a stati è composta da:
- Una batteria di FF, che compone un registro, che memorizza lo stato.
- Una rete di stato di futuro
- Una rete di uscita

![[Pasted image 20230308132616.png]]

In particolare la rete di uscita dipende sicuramente dallo stato in cui si trova il sistema ed eventualmente:
- da nien'altro (FSM di **Moore**)
- dagli ingressi a quell'istante (FSM di **Mealy**)

In una FSM la **massima frequenza di funzionamento** è determinata dal tempo di setup dei vari flip flop.
Inoltre si tiene conto anche del ritardo della logica combinatoria:
$$
t_{LC,MAX}=max(t_{IN-OUT},t_{IN-FS},t_{SP-OUT},t_{SP,SF})
$$
mentre teniamo conto anche del ritardo minimo: $T_{LC,min}$
Il **minimo periodo di clock** è pari a 
$$
T_{ck}=t_{ck,q}+T_{LC,max}+t_{su}
$$
Inoltre va verificata la condizione:
$$
t_{a}>t_{r} \implies t_{ck,q}+t_{LC,min} >t_{h}
$$
