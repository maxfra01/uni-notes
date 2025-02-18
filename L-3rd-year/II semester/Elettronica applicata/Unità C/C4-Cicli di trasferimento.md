# Cicli di trasferimento
---
L'obiettivo di un ciclo di trasferimento è quello di **garantire il corretto trasferimento dell'informazione**, in modo sincrono o asincrono.
Se a livello fisico garantivamo corretti livelli di tensione, bit ecc..., **a livello ciclo ci occupiamo della trasmissione di gruppi di bit**.

Concentriamoci ora su un'operazione di scrittura e modellizziamo il trasferimento come segue:

![[Pasted image 20230502161507.png]]

Oltre all'informazione notiamo la presenza di un segnale valid, per la corretta trasmissione.
E' importante che i tempi di setup e di hold siano rispettati!
Nel caso di scrittura c'è la stessa direzione di trasferimento per informazione e controllo.

### Temporizzazione del ciclo

Nel caso **sincrono** abbiamo **tempistiche ben note e fisse** e bisona garantire il funzionamento anche nel worst case scenario. 
Con un metodo **asincrono** ogni modulo, prima di procedere, attende un **segnale di acknowledge** e poi svolge il proprio compito: si parla di **temporizzazione adattiva**.

# Cicli di scrittura
---

### Ciclo di scrittura sincrono

![[Pasted image 20230502162014.png]]

L'informazione e lo **strobe** viaggiano con tempistiche ben fisse:
- $t_{A}$ è il tempo che intercorre **fra invio dell'operazione e l'invio dello strobe**
- $t_{B}$ è il tempo che intercorre fra l'attivazione dello strobe e il cambio dell'informazione

Nel tempo $t_{A}$ la sorgente deve aspettare un periodo di tempo prefissato determinato dal **tempo di setup** e dallo **skew** che è possibile avere fra i segnali (strobe compreso):
$$
t_{A} \geq t_{su}+t_{k}
$$
Rispettando questa condizione garantiamo (anche nel caso peggiore) il corretto trasferimento.
Il tempo $t_{B}$ dev'essere pari almeno al **tempo di hold** della memoria lato ricevitore più il tempo di **skew**:
$$
t_{B} \geq t_{h}+t_{k}
$$
I **Cicli di scrittura** intesi come **transazione** effettuata in modo atomico deve attendere ogni volta la somma del tempo A e il tempo B:

![[Pasted image 20230502162654.png]]

Osserviamo che la sorgente non deve conoscere i tempi massimi e minimi di scrittura ma solamente la loro differenza.
Notiamo come il segnale di **Strobe** è usato come CLK per il componente di memoria.
Il **Tempo di scrittura totale** è dunque pari a:
$$
t_{WR}=2t_{k}+t_{su}+t_{h}
$$
```ad-note
Per scritture broadcast (ovvero su più destinazioni contemporanee) dovremmo rispettare in ogni caso i vincoli **del componente di memoria più lento**.
```


### Ciclo di scrittura Asincrono

![[Pasted image 20230502163207.png]]

In questo caso esistono due direzioni dei segnali: la conferma realizza il meccanismo di handshaking.
Appena l'informazione è messa sul bus, si attende un tempo di skew $t_{k}$ per attivare lo strobe. Quando arriva alla logica di controllo P, si attende un tempo di setup e si manda il clock all'elemento di memoria.
Dopo un tempo di hold si avrà l'attesa di un tempo di hold e si manda l'ACK:
Quando ACK arriva al trasmettitore allora si può cambiare il dato.

In questo caso alla sorgente dev'essere noto **solamente il tempo di skew $t_{k}$**

![[Pasted image 20230502163703.png]]

Per completare una transazione (come un ciclo di scrittura) occorre far passare un tempo pari a:
$$
t_{WR}=t_{k}+t_{su}+t_{h}+4t_{TX,max}
$$
Il contributo del tempo di trasmissione moltiplicato per quattro dipende dal fatto che l'handshaking (in chiusura) si basa sullo scambio di due segnali e quindi 4 segnali di trasferire.

Quando un ciclo è **chiuso (completo)** ne può iniziare uno nuovo:
- se il precedente è terminato
- se i segnali di controllo (ACK e STB) sono allo stato basso

```ad-note
Si parla di cicli **cadenzati** quando i segnali di controllo si alzano in corrispondenza nei fronti del clock, ovviamente sempre rispettando le condizioni temporali. Il vantaggio sta nella semplicità del controllo, avendo a disposizione un clock locale.
```


# Cicli di Lettura
---

### Ciclo di lettura Sincrono

![[Pasted image 20230502164804.png]]

In questo caso il **master** (che vuole leggere) alza il segnale di strobe ed è quindi pronto a leggere (in questa fase fornisce anche indirizzi di memoria, qui è semplificato).
Trascorso un **$t_{TX,max}$** la sorgente la sorgente, dopo un tempo $t_{A}$ (tempo di accesso) mette sul BUS l'informazione richiesta, che dopo un tempo di trasmissione massimo arriva al master.
Quando arriva l'informazione si garantisce un tempo di setup e un tempo di hold per memorizza l'informazione; dopodichè si abbassa lo strobe.

La durata totale della transazione è quindi pari a:
$$
t_{RD}=t_{A}+t_{h}+t_{su}+ 4t_{TX,max}
$$

### Ciclo di lettura Asincrono

![[Pasted image 20230502165356.png]]

Quando il master alza lo strobe, dopo il tempo di trasmissione e dopo il tempo di accesso, si fornisce il dato e l'ack, perciò bisogna tenere conto dello skew.
Il tempo in verde è necessario per garantire il tempo di setup e di hold.
$$
t_{RD}=t_{k}+t_{A}+t_{su}+t_{h}+4 t_{TX,max}
$$

