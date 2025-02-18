# Interrupt controller

Per gestire richieste I/O abbiamo due possibilità: il polling e l'**interrupt**.
Il polling è poco efficacie nel nostro caso perchè lavoriamo in bare metal e non è efficiente dal punto di vista energetico.

Gli **eventi** a cui dobbiamo rispondere sono categorizzati:
- **Infrequenti ma critici**: USAGE_FAULT, LOW_BATTERY ecc...
- **Sincronizzazione id I/O**: scatenare un interrupt quando il segnale su una porta cambia (es. premere un button)
- **Periodic Interrupts**: generati da un timer 
- **Data acquisition sample ADC**

### Polling

La tecnica si base su un flusso dalla CPU verso i periferici, dove si controlla lo **stato** di un periferico in modo periodico (contenuto in un registro di stato nel periferico). Se lo stato del periferico richiede intervento della CPU allora si passa a un certo handler per gestirlo.

In ogni caso il tempo per controllare le periferiche dipende da come ho fatto il controllo degli stati e dall'ordine.

E' una soluzione facile concettualmente, ma è molto scomodo dal punto di vista energetico. Inoltre è lento per l'attesa (latency) ed è inefficiente per richieste annidate.

### Interrupt

In questo caso sono le periferiche ad interagire con la CPU, il sistema è messo in low power mode: si risveglia quando ricevo un interrupt.
Le soluzioni attuali sono implementate come una IVT.

# Setup del sistema per gestire interrupt

Per prima cosa bisogna impostare al **boot time** l'inizializzazione delle strutture dati (contatori, puntatori, flag che causano interrupt, semafori ecc...).
Poi si passa alla configurazione del **Interrupt Controller (IC)**:
- Bisogna abilitare le sorgenti di interrupt
- Bisogna impostare la priorità di tutte le sorgenti

Durante il **RUNTIME** per ogni service routine bisogna fare le seguenti operazioni:
- **Accorgersi** (ACK), quindi vado a pulire i flag che indicano se l'interrupt è attivo
- **Salvare** i registri R4-R8,R10,R11 (definito da ABI AAPCS)
- **Comunicare** tramite variabili condivise

```ad-note
title: ARM vs 8086
Normalmente un IC ha delle opzioni per far ignorare alla CPU tutti gli interrupt (**Interrupt Enable**). In ARM questa cosa non è possibile. In x8086 invece sì.

```

Nella scheda usiamo `NVIC` come oggetto per accedere alla **Nested Vectored Interrupt controller**: potrò poi acceder a `ISER` per abilitare un certo interrupt e a `IP` per cambiare la priorità.
Con **Keil** è possibile accedere al NVIC con Peripherals > Core Peripherals > Nested Vectored Interrupt Controller

# Power Management

Informazioni utili all'interno del **system control registers**.
All'interno di quest'area sono fondamentali i registri **PCON** e **PCONP**.
I Primi due bit di PCON possono configurare diverse opzioni di risparmio energetico:

![[Pasted image 20231211122949.png]]

In **Sleep** mode il CLK viene fermato (tramite un blocco gate), ma tutti i periferici continuano a funzionare. Ci si risveglia ogni volta che c'è un interrupt.

In **Deep Sleep** il main oscillator entra in fase di power down: tutto il chip si ferma e il flag DSFLAG in PCON è settato. Per uscire è necessario l'intervento di componenti come il Watchdog timer.

# Timer

Nella nostra scheda c'è un **decreasing count**, dove impostiamo un registro MATCH VALUE a un certo valore: il timer parte da zero e poi appena arriva al match value si scatena generalmente un interrupt.

Per **trovare il valore da inserire nel registro**, sulla base del tempo che vogliamo aspettare, usiamo la seguente formula:
$$
\text{count} = \text{time}[s] \cdot \text{frequency}\left[ \frac{1}{s} \right]
$$
Con questa tecnica il massimo tempo aspettabile (considerando i registri su 8 bit) è circa 171 secondi.
Per attesa più lunghe esistono diverse soluzioni:
- Cascata di timer (HW)
- Prescaler (HW): modulo che va a modificare il clock che arriva al timer, andando ad allungarne il tempo
- Soluzioni SW

Nella scheda in particolare sono presenti diversi timer, tra cui quelli standard, quelli del sistema operativo e degli extra timers.

I registri base per usare un timer sono:

![[Pasted image 20231212092709.png]]

In particolare MCR ha 3 bit e sono configurabili come segue:

![[Pasted image 20231212093101.png]]

Il TCR è configurabile come segue:

![[Pasted image 20231212093258.png]]

