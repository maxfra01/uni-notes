# Combinazioni di sistemi LTI
---
E' possibile combinari sistemi LTI insieme per ottenere ancora un [[Sistemi lineari]] in cui la funzione di trasferimento si ricava con semplici operazioni.
Sono descritti tramite **diagrammi a blocchi**.

### Parallelo di LTI

![[Pasted image 20221021120506.png]]

$$
Y(f)=H_{1}(f)X(f) + H_{2}(f)X(f)=X(f)(H_{1}(f)+H_{2}(f))
$$
Dunque nel caso di parallelo otteniamo che la funzione di trasferimento del sistema è la somma delle funzioni di tasferimento dei sottosistemi.
$$
H_{tot}(f)=H_{1}(f)+H_{2}(f)
$$

### Serie di LTI

![[Pasted image 20221021120729.png]]

$$
Y(f)=(H_{1}(f)X(f))H_{2}(f)=X(f)[H_{1}(f)H_{2}(f)]
$$
Dunque nel caso di serie abbiamo che la funzione di trasferimento è il prodotto delle funzioni di trasferimento dei sotto-sistemi.
$$
H_{tot}(f)=H_{1}(f) \cdot H_{2}(f)
$$
### Retroazione

![[Pasted image 20221021121102.png]]

...calcoli....


# Amplificatori e ritardatori
---
Consideriamo la risposta all'impulso e la funzione di trasferimento dei due sistemi:

| Ritardatore          | Amplificatore            |
| -------------------- | ------------------------ |
| $h(t)=\delta(t-T)$   | $h(t)=A \cdot \delta(t)$ |
| $H(f)=e^{-2j\pi fT}$ | $H(f)=A$                         |

Nel caso del ritardatore avremo un output del tipo $x(t) \implies x(t-T)$

Nel caso dell'amplificatore $x(t) \implies A \cdot x(t)$

### Reti di amplificatori e ritardatori
Combinando opportunamente questi due elementi possiamo ottenere le strutture base per:
- **FIR, Finite Impulse Response**, con funzione di trasferimento pari a (senza retroazione):$$
H_{FIR}(f)=\sum_{i}\alpha_{i}e^{-2j\pi f\tau_{i}}
$$
- **IIR, Infinite Impulse Response**, con funzione di trasferimento pari a: $$
H_{IIR}(f)=\frac{\sum_{i} \alpha_{i}\exp(-2j\pi f\tau_{i})}{\sum_{j}\beta_{i}\exp(-2j\pi f \tau_{j})}
$$

# Banda
---

La **larghezza di banda** è definita come l'intervallo di frequenza occupato da un segnale o da una funzione di trasferimento.

![[Pasted image 20221022100039.png]]

Le definizioni di banda sono molteplici, ma nel più classico dei modi si definisce come l'intervallo di frequenze al di fuori del quale lo spettro è nullo.

### Banda unilatera e bilatera
I segnali reali hanno una trasformata di Fourier con modulo pari, ma solitamente si è interessati solamente alle frequenze negative; in tal caso si parla di banda **unilatera**, se considero anche frequenze negative avrò la bilatera.
Normalmente useremo la unilatera.

### Banda a -3dB
E' la frequenza alla quale il modulo quadro della funzione di trasferimento diminusice del 50% rispetto al picco massimo.
$$
|H(f)|^2_{dB}=10 \log_{10}(|H(f)|^2)
$$

![[Pasted image 20221022100738.png]]

### Banda equivalente di rumore
Definita come:
$$
B_{eq}=\frac{\int_{-\infty}^{\infty} |H(f)|^2 \, df }{2 \cdot max(|H(f)|^2)}
$$

### Banda contenente una data % di energia
E' definita come la larghezza dell'intervallo $[a,b]$ di frequenze che contengono una data x% di energia del filtro stesso.
$$
B_{x\%}=|a-b| : \int _{a}^b |H(f)|^2\, df=\frac{x}{100}E(H) 
$$

![[Pasted image 20221022101412.png]]

Esiste questa definizione in versione monolatera.


# Distorsione lineare
---
Per distorsione di un sistema generico si intende che l'uscita non ha le stesse caratteristiche del segnale in ingresso. (la "forma è modificata").
Nei sistemi lineari si parla di **distorsione lineare**.

Considero un'uscita del tipo:
$$
y(t)=A\cdot x(t-t_{D})
$$
Dato che un'**amplificazione o un ritardo non modificano la forma del segnale**, allora la condizione di non distorsione richiede modulo costante e fase lineare **all'interno della banda del segnale in ingresso (condizione sufficiente)**
$$
H(f)=j \cdot e^{-2j\pi t_{D}f}
$$

![[Pasted image 20221022102512.png]]
