# Spettro di energia
---
Definiamo lo **spettro di energia** di un segnale come il modulo quadro della sua trasformata di Fourier:
$$
S_{x}(f)=|X(f)|^2
$$
Definizione molto usata perchè utile al calcolo dell'energia:
$$
E(x)=\int_{-\infty}^{\infty} S_{x}(f) \, df 
$$
Consideriamo ora un generico filtro con una data funzione di traferimento $H(f)$, allora possiamo dire che:
$$
Y(f)=H(f)X(f) \implies |Y(f)|^2=|H(f)|^2 |X(f)|^2=S_{y}(f)=|H(f)|^2 S_{x}(f)
$$
Si intuisce perciò che lo spettro in uscita di un filtro conta solamente il modulo quadro e non la fase!
Ovviamente si ricava immediatamente che l'energia del segnale in uscita è
$$
E(y)=\int_{-\infty}^{\infty} |H(f)|^2S_{x}(f) \, dx 
$$

### Significato fisico
Riflettiamo ora sul significato fisico dello spettro di energia, detto semplicemente spettro.
Considero un sistema con una funzione di trasferimento molto stretta intorno a $f_{0}$

![[Pasted image 20221101151940.png]]

Allora è ragionevole pensare che lo spettro d'uscita dipenda solamente dal valore del segnale nel puto $f_{0}$.
Dunque lo spettro dà informazione relativa alla **quantità di energia di un certo segnale attorno a ogni possibile frequenza**.
Dimensionalmente abbiamo quindi le dimensioni di energia su frequenza:
$$
S_{x}(f)=\left[  \frac{energia}{Hz} \right]
$$

# Funzione di autocorrelazione
---
Per segnali ad energia finita definiamo la seguente funzione di autocorrelazione:
$$
R_{x}(\tau)=\int_{-\infty}^{\infty} x(t+\tau) x^*(t) \, dt 
$$
O equivalentemente, ricordando la definizione di convoluzione:
$$
R_{x}(\tau)=x(\tau)*x^*(-\tau)
$$
Questa funzione si lega con lo spettro in quanto lo spettro è uguale alla trasformata di Fourier di quest'ultima.
$$
S_{x}(f)=\mathcal{F}(R_{x}(\tau))
$$
Casi particolari, quando $\tau=0$ allora abbiamo $R_{x}(0)=E(x)$

L’autocorrelazione è anche interpretabile come il prodotto scalare del segnale con se stesso traslato di $\tau$

![[Pasted image 20221101153012.png]]

Proprietà dell'autocorrelazione per un segnale $x(t)$ reale:
- simmetria Hermitiana
- pari
- massimo nell'origine
- il massimo corrisponde all'energia del segnale

```ad-note
Per testare se una funzione può rappresentare una autocorrelazione si deve testare:
- la sua parità
- il massimo nell'origine
- che il suo spettro di energia assuma solo valari positivi
```

![[Pasted image 20221101182544.png]]

Per un segnale "veloce" abbiamo autocorrelazione "stretta"
Per segnali "lenti" invece è "larga"

# Mutua correlazione
---
Possiamo definire la mutua correlazione fra due segnali $x$ e $y$ come:
$$
R_{xy}(\tau)=\int_{-\infty}^{\infty}  x(t+\tau)y^*(t)\, dt=R_{yx}^*(-\tau) 
$$
Di conseguenza lo spettro di mutua correlazione fra segnali risulta essere pari a:
$$
S_{xy}(f)=\mathcal{F}(R_{xy}(\tau))=S_{yx}^*(f)
$$
Applicazioni della cross-correlazione è quella di verificare quale fra un insieme di segnali sono vicini ad un segnale di riferimento, si cerca dunque quello con massima cross-correlazione.


# Spettro di potenza
---
Dato che l'energia dei segnali periodici è infinita non è possibile dare una definizione di spettro di energia, perciò cerchiamo una quantità $G_{x}(f)$ che rispetti (similmente allo spettro di energia) la seguente relazione:
$$
\int_{-\infty}^{\infty} G_{x}(f) \, df=P(x) 
$$
Visti i risultati visti in [[Segnali periodici]] allora possiamo definire lo **spettro di potenza** come:
$$
G_{x}(f)=\sum_{i}|\mu_{i}|^2 \space \delta\left( f-\frac{i}{T} \right)
$$
Per segnali periodici definiamo l'autocorrelazione come:
$$
R_{x}(\tau)=\frac{1}{T} \int_{-\frac{T}{2}}^{T/2}x(t+\tau) x^*(t) \, dt 
$$
E anche in questo caso possiamo definire la seguente relazione:
$$
G_{x}(f)=\mathcal{F}(R_{x}(\tau))
$$


Per segnali non periodici sempre a energia infinita ma potenza finita abbiamo le seguenti definizioni:
$$
G_{x}(f)=\lim_{ T \to \infty } \frac{1}{T} |X_{T}(f)|^2 
$$
