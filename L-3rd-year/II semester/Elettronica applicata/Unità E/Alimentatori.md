# Schema a blocchi PDU
---
![[Pasted image 20230530160935.png]]

Il trasformatore abbassa la tensione di rete (220 in italia) all'ordine di alcune decine di volt.
I diodi raddrizzatori trasformano la corrente alternata, ottenendo una componente continua.
Il regolatore di tensione mantiene il più possibile la tensione costante priva di oscillazioni.
E' importante che il regolatore mantenga la stabilità.

# Raddrizzatori
---
### Raddrizzatore a singola semionda

![[Pasted image 20230530161347.png]]

Con questo circuito abbiamo che le sole semionde positive tendono a passare e ad attraversare il carico.
Questo perchè sulle semionde positive il diodo si comporta come un circuito aperto.
Notiamo che l'andamento in uscita della tensione è determinata dallo scaricamento nel tempo del condensatore.

Il condensatore va scelto in modo da non far scendere la tensione al di sotto di un valore utile.

### Raddrizzatore a onda intera

![[Pasted image 20230530161536.png]]

Per il circuito a singola semionda è necessario un condensatore di grande capacità, se vogliamo però ridurre questo vincolo usiamo un condensatore ad onda intera.
Esso fa passare anche le semionde negative invertite di segno grazie a un circuito a 4 diodi.

La corrente che circola nei diodi lo fa in modo molto veloce, ovvero in un **angolo di conduzione $\alpha_{c}$** molto piccolo:

![[Pasted image 20230530163005.png]]

Dal punto di vista del contenuto spettrale si avranno grandi contributi quindi per non avere troppi disturbi conviene inserire dei filtri.

Il condensatore deve essere dimensionato correttamente:
In un tempo pari a metà del periodo esso deve scaricarsi al massimo in modo da non uscire da un certo intervallo:

![[Pasted image 20230530163456.png]]

In particolare chiamiamo **tensione di ripple** la variazione massima che può avere la tensione in metà periodo dal massimo al minimo.
Possiamo calcolare $V_{RI}$ a partire dal condensatore come:
$$
V_{RI}=\frac{I_{O}}{2fC}
$$
Perciò possiamo ricavare la capacità per garantire il ripple richiesto.
In questo caso (usando due diodi per il circuito a onda intera) abbiamo che:
$$
V_{DC}=V_{picco} -2V_{diodo} -\frac{V_{RI}}{2}
$$
**Queste tecniche viste finora vanno bene per potenze minori di 10 W**


# Regolatore in uscita
---

![[Pasted image 20230530164012.png]]

Cerchiamo di diminuire il piu possibile le due quantità:
$$
S_{L}=\frac{\Delta V_{0}}{\Delta I_{L}} \quad S_{I}=\frac{\Delta V_{0}}{\Delta V_{I}}
$$
Ovvero la variazione della tensione in uscita quando varia la tensione sul carico e la tensione in ingresso.

### Regolatore parallelo

![[Pasted image 20230530164243.png]]

Possiamo andare a modificare la resistenza parallelo in modo da mantenere $V_{0}$ costante, per variazioni di $V_{I},L$
La resistenza regolabile è realizzata con diodo zener, in particolare:

![[Pasted image 20230530164439.png]]



### Regolatore serie

![[Pasted image 20230530164359.png]]

Stesso principio in linea di massima.

Le resisten

