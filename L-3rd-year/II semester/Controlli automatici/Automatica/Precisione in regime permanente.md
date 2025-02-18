# Precisione in regime permanente
---
Consideriamo il seguente schema:

![[Pasted image 20230503093112.png]]

Con le seguenti fdt:
$$
G_{a}(s)=C(s) \cdot F(s) \quad \quad W(s)=\frac{y(s)}{r(s)} \quad \quad W_{y}(s)=\frac{y(s)}{y_{des}(s)}
$$
Poichè abbiamo **asintotica stabilità** sappiamo che esiste la condizione di **regime permanente**. La **precisione** con la quale il regime permanente insegue il riferimento è un parametro fondamentale, e la sua metrica è l'errore:
$$
e=y_{des}-y
$$
### Segnali canonici

Per studiare i sistemi ci serviamo spesso di segnali **polinomiali e sinusoidali**:
$$
r(t)=\frac{t^k}{k!} \implies r(s)=\frac{1}{s^{k+1}}\,,k=0,1,2\dots
$$
$$
r(t)=\sin(\omega_{0}t) \implies r(s)=\frac{\omega_{0}}{s^2+\omega_{0}^2}
$$
Sono in queste specifiche forme per manipolarle meglio nel dominio di laplace.
A seconda del valore di $k$ per il segnale polinomiale, è possibile simulare i comportamenti degli ingressi (moltiplicati per $K_{T}$):
| Valore di $k$ | Significato                                                                                   | Funzione         |
| ------------- | --------------------------------------------------------------------------------------------- | ---------------- |
| $k=0$         | ---                                                                                           | Gradino unitario |
| $k=1$         | Per un uscita in posizione, corrisponde a imporre velocità desiderata costante pari a $K_{T}$ | Rampa            |
| $k=2$         | Corrisponde a imporre accelerazione desiderata costante pari a $K_{T}$                        | Arco di parabola                 |

### Esempio

Profilo di posizione:

![[Pasted image 20230503094341.png]]

Ora nel primo tratto noto che (arco di parabola) ho accelerazione costante $k=2$, poi ho un tratto lineare (velocità costante, $k=1$), nel tratto finale c'è accelerazione negativa.

Per il profilo di velocità, lo si può ricavare derivando questo profilo:

![[Pasted image 20230503094450.png]]

Coerente con quello detto prima; derivando ulteriormente troviamo il profilo di accelerazione:

![[Pasted image 20230503094525.png]]



# Inseguimento di segnali polinomiali
---
Per analizzare la **precisione** dell'inseguimento è possibile far uso **solamente** dell'analisi di: 
- **tipo di sistema** 
- **guadagno stazionario**
- **fdt d'errore**.

### Tipo di sistema

Il **Tipo di sistema** è determinata dal numero di poli nell'origine della fdt dell'anello: In generale un sistema è **di tipo $h$** solamente se la sua funzione di trasferimento ad anello ha un polo di molteplicità $h$ in $s=0$.

### Guadagno stazionario

Il **Guadagno stazionario** è definito come:
$$
K_{G}=\lim_{ s \to 0 }\{s^h \cdot G(s)\} 
$$
Perciò dipende dal tipo di sistema (essendo $h$ la molteplicità dei poli dell'origine):
| Tipo $h$ | Guadagno unitario   | Nome                  |
| -------- | ------------------- | --------------------- |
| $k=0$    | $K_{Ga}=G_{a}(0)$   | Guadagno di posizione |
| $k=1$    | $K_{Ga}=G_{a}'(0)$  | Guadagno di velocità  |
| $k=2$    | $K_{Ga}=G_{a}''(0)$ |Guadagno di accelerazione                       | 

Inoltre sappiamo che il guadagno stazionario rappresenta il **guadagno della fdt a Bassa Frequenza (BF)**.

### Fdt d'errore

La **Fdt d'errore** ci consente di andare ad analizzare gli errori, senza doverli calcolare esplicitamente. Considerando il seguente blocco:

![[Pasted image 20230505084607.png]]

Andiamo ad esprimere l'errore $e$ in funzione di $y_{des}$ perciò con le regole dell'algebra dei blocchi otteniamo:
$$
W_{e,y}(s)=\frac{e(s)}{y_{des}(s)}=\frac{1}{1+G_{a}(s)}
$$
Per poi ottenere l'errore in funzione del riferimento andiamo a moltiplicare per $K_{r}$, ottenendo:
$$
W_{e}=\frac{e(s)}{r(s)}
$$
Essendo in condizione di asintotitica stabilità possiamo affermare che **vale il teorema del valore finale** perciò possiamo calcolare l'errore per tempi lunghissimi:
$$
e_{\infty}=\lim_{ t \to \infty } e(t)=\lim_{ s \to 0 } s \cdot e(s)
$$

![[Pasted image 20230505085122.png]]

Possiamo distinguere 3 casi a seconda del grado del riferimento e del tipo di sistema:
| Caso                                                        | Errore                                                                                                      |
| ----------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| Grado $h$ del riferimento **pari** al tipo $h$ del sistema  | L'errore è **finito e non nullo**, inversamente proporziale ad $K_{Ga}$ e viene detto **errore intrinseco** |
| Grado $h$ del riferimento **minore** del tipo $h$           | L'errore è **nullo** qualsiasi sia il guadagno $K_{Ga}$                                                     |
| Grado $h$ del riferimento maggiore del tipo $h$ del sistema | In questo caso l'errore **diverge** e quindi il sistema non è in grado di inseguire il riferimento                                                                                                            |


# Implicazioni sul progetto del controllore
---
I vincoli che si andranno a generare saranno sul tipo di sistema e sul guadagno stazionario minimo:

![[Pasted image 20230505092311.png]]

Ricordando che il valore della fdt ad anello dipende da quella del controllore e da quella del sistema, allora, dato che $F(s)$ è noto imporremo dei vincoli su $C(s)$.

Chiamimao $n_{0,F}$ il numero di poli del sistema da controllare $F(s)$, mentre $n_{0,C}$ è il numero di poli del controllore $C(s)$.

Per **garantire l'inseguimento del riferimento di grado $k$** dovremmo avere che:
$$
\textrm{se} \space n_{0,F}<k \implies n_{0,C}= (k-n_{0,F})   
$$
Nel caso invece in cui $n_{0,F}\geq k$ allora non serve aggiungere poli con $C(s)$ in quanto l'errore risulterà essere:
- Finito, nel caso si $n_{0,F}=k$
- Nullo, solo se $n_{0,F}> k$

Invece per **garantire l'inseguimento con errore nullo** dobbiamo assicurarci che $n_{0,F}\geq k+1$ e quindi abbiamo che:
$$
\textrm{se} \space n_{0,F}<k+1 \implies n_{0,C}= (k+1-n_{0,F})  
$$

Se imponiamo un limite massimo all'errore in regime permanente allora si va a generare un vincolo sul guadagno stazionario minimo della fdt ad anello, e quindi conseguentemente sulla fdt del controllore:
$$
|e_{\infty}(K_{Ga})|\leq e_{max} \implies |K_{Ga}|\geq K_{Ga,min} \implies |K_{C}|\geq K_{C,min}
$$


