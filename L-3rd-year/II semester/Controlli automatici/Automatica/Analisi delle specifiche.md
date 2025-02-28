# Impostazione del progetto
---
Dato il sistema seguente progettare il controllore $C(s)$ che soddisfi le specifiche:
- Sulla risposta nel tempo a riferimenti assegnati (in regime permanente o nel transitorio)
- Sulla risposta in frequenza, sull'attività del comando, sulla robustezza e sull'attenuazione dei disturbi

![[Pasted image 20230519090720.png]]

### Forma del Controllore

La**funzione di trasferimento del controllore** da progettare, in generale, è nella forma:
$$
C(s)= \frac{K_{c}}{s^h} \cdot C'(s)
$$
Composta dai 3 elementi:
- $K_{c}$ rappresenta il guadagno stazionario, che richiamiamo calcolarsi come $K_{c}=\lim_{ s \to 0 }s^h C(s)$
- $h$ è il numero di poli in $s=0$, ovvero il tipo di sistema
- $C'(s)$ è una fdt di tipo zero avente **guadagno stazionario unitario**

Occorre dunque definire questi 3 elementi, in particolare $h$ e $|K_{c}|$ sono quelle che, una volta garantita l'asintotica stabilità in catena chiusa, soddisfano le **specifiche statiche**, ovvero le specifiche sull'errore e sulla reiezioni.

Il segno del guadagno stazionario $K_{c}$ viene scelto in modo da poter stabilizzare il sistema in catena chiusa mediante opportuna definizione di $C'(s)$

Per quanto riguarda la parte "dinamica", ovvero $C'(s)$ , va progettato in modo da garantire asintotica stabilità in catena chiusa, con buona robustezza e che soddisfi tutte le specifiche dinamiche quali:
- caratteristiche in transitorio alla risposta al gradino
- risposta in frequenza in catena chiusa
- inseguimento e reiezioni di disturbi sinusoidali

# Specifiche statiche
---

### Guadagno stazionario e numero di poli in 0

Prima cosa da vedere sono questi 3 vincoli:
- Precisione di inseguimento in regime permanente di riferimenti **polinomiali**
- reiezione e attenuazione.

Il primo passo è decidere $h$, ovvero il numero di poli nell'origine. Esso deve corrispondere al **più piccolo numero di poli $h$ che serve per soddisfare tutte le specifiche contemporaneamente**.

Il secondo passo è decidere $K_{c}$: procediamo con individuare tutti i vincoli sul guadagno e scegliamo **il guadagno stazionario minimo per garantirli tutti contemporaneamente**.

### Esempio

![[Pasted image 20230519092639.png]]

Con le specifiche:

![[Pasted image 20230519092653.png]]

Partiamo dalla scelta di $h$: dato che il sistema è di tipo 1 e il riferimento a rampa è di tipo 1 allora dato che sono uguali non è necessario aggiungere poli
Le altre due specifiche riguardano disturbi costanti, che **avendo sempre un effetto limitato sull'uscita** non richiedono l'aggiunta di poli.
Possiamo allora concludere che:  $h=0$

Per quanto riguarda il vincolo sul guadagno, guardando la tabella sugli errori allora troviamo che $e_{r,\infty}=\frac{K_{r}}{K_{G_{a}}}=\frac{K_{r}}{K_{c}K_{F}}$ 
Dato che l'errore in modulo deve essere più piccolo di 0.1 allora troviamo la prima condizione : $|K_{c}|\geq 8$
Per la seconda specifica, dato che $h=0$ allora non ho poli a monte, ma solo un polo a valle del disturbo: $y_{du,\infty}=|\frac{D_{u}}{K_{c}}|\leq 0.01$ e quindi invertendo avrò: $|K_{c}|\geq 10$
Prendiamo quella minima che le soddisfi tutte: $|K_{c}|\geq 10$

```ad-caution
Non è consigliabile inserire un polo in $s=0$, se non strettamente necessario per soddisfare le specifiche statiche.
Questo perchè un polo nell'origine porta a una perdita di fase di 90°, che ci costringerà a introdurre un elevato recupero di fase nella scelta di $C'(s)$
```


### Stabilizzazione del sistema

Per prima cosa dobbiamo studiare il segno del guadagno stazionario in modo da garantire l'asintotica stabilità in catena chiusa.
Applichiamo **il criterio di Nyquist per guadagno $K_{c}$ variabile sulla funzione $F(s)$**.
Ci chiediamo semplicemente se il punto critica sta sul semiasse reale positivo o negativo.
Solamente nel caso di una $\frac{F(s)}{s^h}$ che sia in grado di garantire stabilità regolare possiamo scegliere il guadagno positivo senza applicare Nyquist.

```ad-note
Cerchiamo di scegliere il valore $K_c$ in modulo minimo.
Qualora sia necessario **possiamo sempre aumentare il guadagno**, ma **mai diminuirlo**.
Nella pratica aumentiamo il valore minimo di un certo margine per tener conto di incertezze.
```

Consideriamo una funzione con 0 poli instabili sulla funzione ad anello: $n_{i,a}=0$
Allora andiamo a tracciare il diagramma di Nyquist:

![[Pasted image 20230519095234.png]]

Se prendo appunto $K_{c}$ positivo sono nel semiasse negativo, allora so che **per alcuni valori di $K_{c}$** sono in grado di assicurare $N=0$ e quindi di conseguenza anche $n_{i,c}=0$

Nei metodi classici ci si basa sull'ipotesi che il controllore $C(s)$ che si sta sviluppando che sia internamente stabile. Ci baseremo su questa ipotesi.
Tuttavia esiste una proprietà detta **p.i.p** che assicura l'esistenza.

# Specifiche dinamiche
---

### Risposta al gradino

Le specifiche sulla risposta al gradino impone vincoli sulla risposta in frequenza.
In particolare è possibile imporre un **vincolo sulla sovrelongazione massima** $\hat{s}$
che si traduce in un **vincolo sul picco di risonanza**: (usando il vincolo $1+\hat{s} \approx 0.9 M_{r}$)
$$
\hat{s} \leq \hat{s}_{max} \implies M_{r} \leq \frac{1+\hat{s}_{max}}{0.9}
$$
Dobbiamo però fare in modo che il vincolo di partenza sia garantito.

Un vincolo **sul tempo di salita** si traduce in un **vincolo sulla banda passate**.
Ricordando la relazione $\omega_{B} \cdot t_{s} \approx 3$ abbiamo:
$$
\begin{cases}
t_{s} \approx t_{s,des} \\
t_{s} \leq t_{s,max}
\end{cases} 
\implies
\begin{cases}
\omega_{B} \approx \frac{3}{t_{s,des}} \\
\omega_{B} \geq \frac{3}{t_{s,max}}
\end{cases}
$$
Ricordiamo che $t_{s}$ è il tempo che il sistema impiega per raggiungere il valore a regime per la prima volta.

```ad-note
Durante il corso non saranno considerate specifiche sul tempo di assestamento $t_a$.
Ci limiteremo a non inserire poli lenti nella fdt in catena chiusa.
```


### Specifiche sulla risposta in frequenza

**Un vincolo sul picco di risonanza** crea un vincolo **sul margine di fase $m_{\phi}$**
Ricordando la relazione $m_{\phi}M_{r} \approx 60°$ allora:
$$
M_{r}\leq M_{r,max} \implies m_{\phi}> = m_{\phi,min}
$$
Il margine di fase minimo **si legge sulla carta di Nichols**.

Un vincolo **sulla banda passante** si traduce in un **vincolo sulla pulsazione di taglio**:

![[Pasted image 20230519103745.png]]

### Specifiche su inseguimento di segnali sinusoidali

Abbiamo visto che per un riferimento di tipo sinusoidale $r(t)=\sin(\omega_{0}t)$ l'errore massima che si può commettere in modulo:
$$
E= |\frac{K_{r}}{1+G_{a}(j\omega_{0})}|
$$
Se ho una specifica sull'errore massimo che posso commettere è necessario garantire una pulsazione molto di crossover molto grande rispetto a quella del riferimento $\omega_{0}\ll \omega_{c}$.
Non è possibile con il nostro progetto di tradurre ciò in una specifica: si farà a posteriori un controllo sul comportamento del sistema.

Un analogo ragionamento si può fare con i disturbi sinusoidali:
$\omega_{c}$ dovrà essere sufficientemente grande o sufficientemente piccolo in base allea posizione del disturbo.


### Esempio 

![[Pasted image 20230519104955.png]]

Dalla prima $\omega_{B}t_{s} \approx 3 \implies \omega_{B} \approx \frac{3}{0.4} \implies \omega_{c}=4.7$
Dalla seconda ottengo ($1+\hat{s} =0.9 M_{r}$):
$$
M_{r}\leq \frac{1+0.25}{0.9} \to^{dB} 20 \log_{10} \frac{(1+0.25)}{0.9}=2.85
$$
Dalla carta di Nichols notiamo che questo si traduce in:
$$
m_{\phi,min} = 41° \implies m_{\phi,min} \in[43,45]°
$$
Il terzo impone che $\omega_{c}\gg 0.1$ e cioè $|G_{a}(j{0}.1)|\gg 1$
