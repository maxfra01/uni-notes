# Errore della risposta in frequenza
---
Il riferimento canonico diventa: $r(t)=\sin(\omega_{0}t)$
Mentre per quanto riguarda la fdt d'errore è del tipo:
$$
W_{e}(s)=\frac{e(s)}{r(s)}=\frac{K_{r}}{1+G_{a}}
$$
L'errore è quindi del tipo sinusoidale con pulsazione corrispondente a quella del riferimento:
$$
e_{p}(t)=E \cdot \sin(\omega_{0}t+\phi_{e})
$$
Per limitare l'errore mi limito a limitare l'ampiezza della sinusoide ovvero minimizzo:
$$
E=|\frac{K_{r}}{1+G_{a}(j\omega_{0})}|
$$
Per fare ciò $G_{a}$ dev'essere molto grande, e questo avviene a **Basse frequenze**.

# Implicazioni sul progetto del controllore
---
Se vogliamo imporre che l'errore sia più piccolo di una quantità desiderata, allora questo si traduce in una condizione sul modulo:
$$
|e_{p}|\leq e_{max} \implies |G_{a}(j\omega)|\geq G_{min}
$$
Affinchè ciò sia garantito dobbiamo fare in modo che il modulo della funzione ad anello sia sufficientemente grande, e ciò avviene principalmente alle basse frequenze, quindi $\omega$ deve essere molto più piccola rispetto alla $\omega_{c}$ di crossover.
Abbiamo perciò un nuovo vincolo sulla $\omega_{c}$ e conseguentemente sulla banda passante.


# Effetti sull'uscita
---
L'effetto del disturbo sull'uscita è pari a :
$$
y_{p,\sin}(t)=Y_{d,p} \cdot \sin(\omega_{d}t+\phi_{d})
$$
Il valore massimo sarà pari a:
$$
Y_{d,p}=D_{s} |W_{d,\sin}(j\omega_{d})|
$$

### Disturbo sull'uscita

Consideriamo ora un sistema come il seguente:

![[Pasted image 20230519084555.png]]

Possiamo calcolare la fdt del disturbo coem:
$$
W_{d,\sin}(s)=\frac{1}{1+G_{a}(s)}
$$
Per gatantire un errore ridotto dobbiamo che $G_{a}(s)$ sia sufficientemente grande e perciò a BF sono **ben attenuati** i disturbi sinusoidali.

### Disturbo sul riferimento

In questo secondo caso abbiamo:

![[Pasted image 20230519084740.png]]

Perciò la fdt in catena chiusa:
$$
W_{d,\sin}=\frac{G_{a}(s)}{1+G_{a}(s)}
$$
In questo caso manterremo un'attenuazione solo quando $G_{a}(s)\ll 1$, questo avviene alle ALTE FREQUENZE dove il modulo è molto piccolo.

### Implicazioni sul progetto del controllore

Se ho un disturno sinusoidale sull'uscita allora $\omega_{c}$ deve essere sufficientemente elevata rispetto alla pulsazione del disturbo.
Se il disturbo è sulla retroazione o sul riferimento allora devo garantire che $\omega_{c}$ sia piccola rispetto alla pulsazione del disturbo.
