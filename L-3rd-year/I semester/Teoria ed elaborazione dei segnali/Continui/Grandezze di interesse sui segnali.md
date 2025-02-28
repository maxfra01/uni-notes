# Grandezze dei segnali

Prima di inoltrarci nella rappresentazione vettoriale, servono alcune definizioni che useremo per tutta la durata del corso, Inoltre servono le basi sul campionamento dei segnali, spiegate in [[Conversione Analogico-Digitale]].

## Potenza di un segnale
Distinguiamo fra potenza media e potenza istantanea:
- La **potenza istantanea** coincide con il quadrato del segnale in quell'istante $$P_{ist}(t)=|x(t)|^2$$
- La **potenza media** non è altro che la media temporale della potenza istantanea$$P(x)=\lim_{a->\infty}\frac{1}{2a}\int_{-a}^{a}|x(t)|^2dt$$
## Energia di un segnale
Definiamo l'energia di un segnale come l'area sottesa dalla sua rappresentazione come funzione.
$$E(x)=\int_{-\infty}^{\infty}|x(t)|^2dt$$
Possiamo classificare i segnali in base all'energia e alla potenza:
- Segnali a **energia finita**: $E(x)<\infty$
- Segnali a **potenza (media) finita**: $P(x)<\infty$

Si noti che se un segnale è a energia finita allora è a potenza media nulla.

## Segnali periodici
Sono segnali che si ripetono regolarmente nel tempo ovvero:$$\exists T:x(t)=x(t+T) \quad \forall t $$
Si possono scrivere come la somma di tutti i periodi che lo compongono ovvero:
$$x(t)=\sum_{n=-\infty}^{\infty}x_T(t-nT)$$

Si dimostra che un **segnale periodico è sempre a energia infinita**.
Solitamente un segnale periodico è a potenza media finita e coincide con l'energia del segnale troncato al singolo periodo $$P(x)=\frac{E(x_T)}{T}$$
## Classificazione dei segnali

![[Pasted image 20220930164224.png]]
