# Stadi comuni
---
Ora che abbiamo esaminato le caratteristiche dei [[Transistor]] possiamo costruire [[Amplificatori]] a partire da transistor MOS o BJT.

A seconda di come si dispone il transistore e di come si preleva l'uscita e l'ingresso otteniamo tre stadi comuni:

| Source comune                        | Drain comune                         | Gate comune |
| ------------------------------------ | ------------------------------------ | ----------- |
| ![[Pasted image 20221108085656.png]] | ![[Pasted image 20221108085714.png]] | ![[Pasted image 20221108085735.png]]            |

## Stadio Source comune

![[Pasted image 20221108093821.png]]

Dall'analisi di piccolo segnale si ottiene il circutio a destra, da cui deduciamo che:
$$
R_{in} \to \infty \quad R_{out}=R|| r_{0}
$$
$$
A_{v}=-g_{m}(R ||r_{0})
$$
Dalla funzione di trasferimento $A_{v}$ notiamo che la precedente configurazione crea un **amplificatore di tensione invertente** con modulo anche elevato.

## Drain comune

![[Pasted image 20221108094640.png]]

Dall'analisi circuitale ricavo che:
$$
R_{in} \to \infty \quad R_{out}=\frac{1}{g_{m}} || (R ||r_{0})
$$
$$
A_{v}=\frac{g_{m}R'}{1+g_{m}R'}
$$
Dove $R'=R ||r_{0}$ 
Otteniamo un **amplificatore di tensione con amplificazione positiva (non invertente) e minore di 1**.

## Gate comune

![[Pasted image 20221108095113.png]]

Dall'analisi di piccolo circuito otteniamo:
$$
R_{in}=\frac{R+r_{0}}{g_{m}r_{0}+1} \approx \frac{1}{g_{m}} \quad R_{out}=R'=R ||r_{0}
$$
$$
A_{v}=g_{m}R' + \frac{R'}{r_{0}} \approx g_{m}R'
$$
Abbiamo ottenuto un **amplificatore di tensione non invertente con modulo elevato**

| Tipo          | $A_{v}$                       | $R_{in}$                  | $R_{out}$                 | commenti                 |
| ------------- | ----------------------------- | ------------------------- | ------------------------- | ------------------------ |
| Source comune | $-g_{m}R'$                    | $\to \infty$              | $R'$                      | amplificatore invertente |
| Drain comune  | $\frac{g_{m}R'}{1+g_{m}R'}<1$ | $\to \infty$              | $\approx \frac{1}{g_{,}}$ |             amplificatore non invertente con mudulo $<1$            |
| Gate comune   | $\approx g_{m}(R')$           | $\approx \frac{1}{g_{m}}$ | $R'$                      |amplificatore non invertente con modulo elevato                          |


# Amplificatore a più stadi
---
Nessuno degli stadi comuni visti ha le caratteristiche che cerchiamo, ovvero **buona amplificazione e resistenza in uscita molto bassa**.
Un esempio di stadio con queste caratteristiche è l'**inseguitore di tensione**, caratterizzato da:
$$
R_{in} \to \infty \quad R_{out}=0 \quad A_{v}=1
$$
Per ottenere questo significativo risultato occorre collegare gli stadi visti sopra **in cascata**.
Proviamo a mettere in cascata un source comune e un drain comune:

## CS-DS

Collegando in cascata questi due stadi e ricavando il circuito equivalente otteniamo:

![[Pasted image 20221115091703.png]]

Possiamo ricavare la tensione in uscita del primo stadio pari a:
$$
v_{out_{1}}=-g_{m_{1}} R||r_{0_{1}} \cdot v_{gs_{1}} =-g_{m_{1}}R||r_{0_{1}} \cdot v_{s}
$$
Ma possiamo notare che questa quantità corrisponde all'ingresso del secondo stadio e quindi continuando a fare i calcoli otteniamo l'espressione di $v_{out_{2}}$.
Possiamo per rifarci ai modelli a blocchi con caratteristiche già note, ricavando:
$$
A_{v}=A_{v,cs} \cdot A_{v,cd}=-g_{m_{1}}(R_{1} \mid\mid r_{0}) \frac{g_{m}(R_{2} \mid\mid r_{0})}{1+g_{m}(R_{2}\mid\mid r_{0})}
$$
$$
R_{in} \to \infty
$$
$$
R_{out}=\frac{1}{g_{m}}\mid\mid R_{2}\mid\mid r_{0}
$$
Abbiamo perciò ottenuto un buon amplificatore con ottime caratteristiche!

## Collegamenti fra stadi

Quando effettuaimo dei collegamenti fra stadi non possiamo farlo semplicemente cortocircuitando l'uscita con l'entrata, perchè così facendo trasferisco sia informazioni sul segnale che la tensione di polarizzazione
Siamo dunque interessati a non far passare questa **tensione di polarizzazione**.

Per effettuare ciò inseriamo un **condensatore di accoppiamento** che in continua (cioè per la tensione di polarizzazione) si comporta come un **circuito aperto** mentre per il segnale in banda si comporta come un cortocircuito.
Quindi la capacità dev'essere abbastanza elevata per una conseguente ammettenza elevata.

![[Pasted image 20221115094922.png]]

Per capacità grande intendiamo quantità dell'ordine di $\mu F$.

I condensatori di accoppiamento servono anche in ingresso e uscita per non far passare corrente sul carico e dunque andare a risparmiare energia.


# Coppia differenziale
---
E' possibile combinare due stadi **NON in cascata**, per ottenere configurazioni notevoli come la **coppia differenziale**.

![[Pasted image 20221121085143.png]]

I due transistori sono identici fra di loro e data la loro configurazione condividono la stessa tensione in ingresso $v_{GS}$.
Inoltre nel nodo A (terminale di Source) noto che la corrente si divide esattamente a metà, ne consegue che anche le correnti di drain sono uguali.

Il condensatore presente è un condensatore di accoppiamento in quanto la tensione in ingresso sarà erogata da un generatore e ci interessa solamente la componente alternata!

Possiamo dunque passare alla rappresentazione del circuito equivalente...

![[Pasted image 20221121092827.png]]

# Coppia Differenziale 2
---
![[Pasted image 20221121095234.png]]

In questo caso applichiamo due ingressi alle porte gate source di entrambi i transistori.
Essendo il circuito qui sopra **lineare** è possibile calcolare l'uscita come la somma degli effetti dovuti a entrambe le tensioni in ingresso:
$$
v_{out}=v_{out}^{(1)} + v_{out}^{(2)}=\frac{R_{3}g_{m}}{2}(v_{in_{1}}-v_{in_{2}})
$$
Quest'uscita è interpretabile in due termini:
$$
\textrm{Tensione differenziale}: \space v_{d}= v_{in_{1}}-v_{in_{2}}
$$
$$
\textrm{Amplificazione Differenziale}: \space A_{d}=\frac{g_{m}R_{3}}{2}
$$
E' importante notare che è solo la tensione differneziale ad essere amplificata, non teniamo conto del valore medio delle tensioni. 

In particolare questo circuito verrà approfondito sulla parte degli Amplificatori [[Operazionali]].