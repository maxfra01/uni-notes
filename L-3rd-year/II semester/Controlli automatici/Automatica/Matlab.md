# Principali comandi
---
Consideriamo come esempio pratico lo schema a blocchi di un servo motore in corrente continua:

![[Pasted image 20230503083634.png]]

### Scrittura della fdt

Come primo obiettivo vogliamo **definire le fdt** dei componenti nello schema, e per fare ciò usiamo i seguenti comandi.
Per prima cosa definiamo la variabile complessa $s$:

```matlab
s=tf('s');
```
Poi se siamo interessati a calcolare la **fdt di un anello** allora usiamo il comando **feedback**.

```
#uso di feedback(ramo_diretto, ramp_retroazione)

FrIa=feedback(CIa*A/(L*s+Ra),Rs)
```

La **retroazione è considerata negativa** in modo automatico.

### Simulazione di un anello al gradino

Per simulare il comportamente di un sistema ad anello chiuso, **nota la fdt**, si usa il comando **step**:

```matlab
# uso di step(fdt, t_fine_simulazione);
W1=feedback(CIa*A/(L*s+Ra),Rs);
W2=feedback(...)
step(W1,5)
hold on
step(W1,5)
hold off
```

Tramite il codice sopra si fa il primo grafico della risposta al gradino, e poi la risposta della seconda fdt: il comando `hold on` permette di far si che i grafici che si generano dopo di esso vengano messi sugli stessi assi dei precedenti.
Dopo averlo usato, per tornare alla modalità normale, si usa `hold off`.


### Parametri temporali

Dall'interfaccia grafica è possibile ricavare i seguenti parametri:
- Risposta in regime permanente
- Errore di inseguimento
- Sovraelongazione massima
- Tempo di assestamento
- Tempo di salita (varie definizioni)

### Poli del sistema

Data una certa **fdt** per calcolare i poli della funzione possiamo usare il comando `damp` che restituisce i poli nella forma parte reale più parte immaginaria, con pulsazione naturale e smorzamento (damping).

```matlab
# E' nota W1

damp(W1);
```

L'output del seguente comando è nella forma:

![[Pasted image 20230503091017.png]]

In una lista di poli cerchiamo di determinare i **poli domananti**, ovvero per confronto, sono quelli che rendono la **costante di tempo più grande** e quindi più influente sul comportamento.
Ricordando che la costante di tempo $\tau$ dipende in modo inversamente proporziale dal polo allora nell'esempio sopra notiamo che il polo dominante è il primo (gli altri hanno $\tau$ di due ordini di grandezza più piccola).