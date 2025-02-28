# Definizioni introduttive

### Parallelismo

Si parla di **DATA LEVEL PARALLELISM** e **TASK LEVEL PARALLELISM**, rispettivamente DLT e TLP.
Esiste anche **INSTRUCTION LEVEL PARALLELISM** (ILP) che risiede più a livello hardware ed è legato al concetto di **pipeline**.

### Consumo
Per il consumo di potenza possiamo distinguere in due categorie:
- Consumo **statico**, richiesto al dispositivo per funzionare ad esempio in stand by
- Consumo **dinamico**, legato a particolari esigenze
Possiamo modellare i consumi con le seguenti formule:
$$
P_{d} = \frac{1}{2} \cdot V^2 \cdot f \quad \quad P_{s} =V \cdot I 
$$
### Affidabilità
Un altro concetto fondamentale è **l'affidabilità** di un sistema.
L'affidibilità è minacciata da bugs nel design del hardware, nel software, difetti di costruzione dell'hardware...
Per valutare l'affidabilità si usano 3 parametri: **MTTF (mean time to failure)** oppure **FIT (failure in time)** valutato su 1 miliardo di ore.
Un altro parametro è **MTBF (mean time between failures)** e **MTTR (mean time to repair)**:
$$
MTBF = MTTF + MTTR
$$

![[Pasted image 20231003091558.png]]

### Performance
Dal punto di vista dell'utente la performance corrisponde al **response time**, ma per il sistema la performance **corrisponde al throughput**.
Tramite il comando 'time' possiamo valutare **User CPU Time**, **System CPU time**, **Elapsed time**, **CPU time / elapsed time**
Per misurare le performance ci serviamo dei **benchmarks**.

Gli indici per misurare la performance sono il **tempo totale di esecuzione**, ma anche la sua **media** e **media pesata**.

### Legge di Amdahl
Possiamo calcolare il miglioramento della prestazione in base al miglioramento di un parametro funzionale:
$$
\text{speedup} = \frac{\text{performance with enhancement}}{\text{performance without enhancement}}
$$
La $\text{fraction}_{\text{enhanced}}$ corrisponde a quanto del nostro programma stiamo cercando di migliorare.
la $\text{speedup}_{\text{enhanced}}$ rappresenta il miglioramento della parte in questione.
Possiamo quindi scrivere la legge di Amdahl come:
$$
\text{speedup}_{\text{overall}} = \frac{1}{(1-\text{fraction}_{\text{enhanced}})+ \frac{\text{fraction}_{\text{enhanced}}}{\text{speedup}_{\text{enhanced}}}  }
$$
In un esempio dobbiamo scegliere tra due soluzioni:

![[Pasted image 20231003102158.png]]

Possiamo calcolare la speedup overall per entrambe le soluzioni: nel primo caso la fractions corrisponde a 0.2 e lo speedup a 10.
Nel secondo caso fraction a 0.5 e speedup a 2.
Calcolando Amdahl la **soluzione 2** è quella che presenta overall speedup maggiore, perciò la migliore.

### Equazione delle performance CPU
$$
\text{CPU time} = ( \sum_{i=1}^n CPI_{i} \cdot IC_{i} ) \cdot \text{Clock time cycle}
$$
Il termine $CPI_{i}$ (clock cycle per instruction) indica il **numero di colpi di clock** richiesti dall'istruzione $i$.
Il termine $IC_{i}$ indica **il numero di volte** in cui l'istruzione $i$ è eseguita.
Il Clock time cycle è l'**inverso della frequenza del sistema**.

Proprio per il parallelismo, tutti questi parametri variano a seconda dell'organizzazione del processore, dalla sua architettura, dal compilatore e dalla pipeline.