# Comparatore
---
E' un blocco funzionale con un **ingresso analogico** e **uscita digitale**.
In particolare se il segnale in ingresso supera una certa soglia $S$ allora l'uscita sarà un valore "alto" di tensione, mentre se il segnale non la supera si avrà un valore di tensione basso.

Ne esiste una versione invertente e non, indicate dalle figure circuitali:

| Non invertente                       | Invertente |
| ------------------------------------ | ---------- |
| ![[Pasted image 20230109090417.png]] |      ![[Pasted image 20230109090435.png]]      |

Nel caso di **ingresso rumoroso** allora intorno alla soglia posso avere numerose oscillazione al di sopra e al di sotto della soglia, dunque è possibile avere numerose commutazioni indesiderate:

![[Pasted image 20230109090716.png]]

# Comparatore con isteresi
---
In questa versione abbiamo due soglie, che permettono al comparatore di comportarsi in funzione del suo stato attuale.
**L'isteresi** è definita come la differenza fra i due valori di soglia $S_{1}-S_{2}$.
In particolare per commutare quando lo stato è $Y=0$ (da 0 a 1) l'ingresso dovrà essere superiore alla soglia $S_{1}$ mentre per commutare con $Y=1$ (da 1 a 0) bisognerà scendere sotto $S_{2}$.

![[Pasted image 20230109091321.png]]

L'andamento della funzione caratteristiche è il seguente:

![[Pasted image 20230109091537.png]]

Le due soglie sono fondamentali in situazioni **di rumore**, in quanto non si hanno false commutazione, fino a che il rumore si minore del valore di isteresi.

# Comparatore basato su operazionale
---
Possiamo usare la dinamica di uscita di un operazionale come il comportamento di un comparatore di tensione non invertente.
$$
V_{out,min}=V_{0, \\log ico} \quad V_{out,max}=V_{1, \\log ico}
$$
## Non invertente, senza isteresi

Applichiamo ai morsetti la tensione in ingresso e la tensione di soglia.
Definiamo la tensione differenziale come $v_{d}=v_{in}-V_{S}$.
A seconda di $v_{d}$ positivo o negativo possiamo decidere se superiamo o no la soglia:
$$
v_{out}= \begin{cases}
& 0 & if   & v_{in}<V_{S} \\
& 1 & if & v_{in}>V_{S}
\end{cases}
$$
![[Pasted image 20230109092904.png]]

## Invertente, senza isteresi

Esattamente come prima ma dobbiamo invertire i morsetti dell'operazionale.

$$
v_{out}= \begin{cases}
& 1 & if   & v_{in}<V_{S} \\
& 0 & if & v_{in}>V_{S}
\end{cases}
$$

![[Pasted image 20230109093011.png]]

## Invertente, con isteresi

![[Pasted image 20230109093830.png]]

La retroazione postiva fa si che il nostro valore di soglia dipenda dallo stato, ovvero da $v_{out}$ e in particolare come valore di soglia prendiamo il valore al morsetto positivo.
$$
v^+=V_{S} \frac{R_{2}}{R_{1}+R_{2}} + v_{out} \frac{R_{1}}{R_{1}+R_{2}}
$$
E $v_{out}$ andrà sostituito con $V_{0, \\log ic}\space V_{1,\\log ic}$ per ottenere la soglia $V_{S_{1}},V_{S_{2}}$.
Per il caso non invertente basta scambiare i morsetti.

# Limitazioni dei comparatori
---
Iniziamo col dire che nei comporatori basati su operazionali è presente una retroazione sempre positiva o si usano ad anello aperto.
Inoltre si usano fuori linearità e raramente $v_{d}=0$ mentre è così nel momento in cui commuta.

### Dinamica del modo comune

Sicuramente il parametro più importante che limita l'ingresso dell'operazionale.
In uscita non ci sono vere proprie limitazioni ma i valori di tensione alto logico e basso logico, mentre la dinamica di corrente è come al solito sempre d'interesse.

### Offset

Altro parametro critico del comparatore, in quando cambia la consizione di commutazione: $$
v_{d}+V_{OFF}=0
$$
