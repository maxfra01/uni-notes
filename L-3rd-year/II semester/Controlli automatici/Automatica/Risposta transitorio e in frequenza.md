# Relazione fra $W$ e $G_{a}$
---
Consideriamo una funzione di trasferimento dell'anello di un sistema nella forma:
$$
G_{a}(s)=\frac{K_{a} \prod_{j=1}^{m_{a}}(s-\xi_{j})}{\prod_{i=1}^{n_{a}}(s-\lambda_{i})}
$$
Mentre la **fdt a catena chiusa** è nella forma:
$$
W_{y}(s)=\frac{K_{W} \prod_{j=1}^{m_{W}}(s-\xi_{Wj})}{\prod_{i=1}^{n_{W}}(s-\lambda_{Wi})}
$$
Combinando lo schema a catena chiusa con l'espressione della funzione ad anello otteniamo:

![[Pasted image 20230516155047.png]]

Notiamo che: 
$$
W_{y}=\frac{G_{a}}{1+G_{a}} = \frac{N_{a}}{D_{a}+N_{a}}
$$

Andando a scrivere l'espressione della fdt a catena chiusa sulla base di questo schema otteniamo il seguente risultato:
$$
\begin{cases}
m_{W}=m_{a} \\
\xi_{Wj}=\xi_{aj} \space \forall j =0,1,\dots \\
n_{W}=n_{a}
\end{cases}
$$
Ovvero abbiamo ottenuto che:
- L'ordine della catena chiusa coincide con quello della catena aperta
- Il numeratore della catena chiusa coincide con quello della catena aperta
- La catena chiusa ha **gli stessi zeri** di quella aperta


### Alte e basse frequenza

Consideriamo una funzione di trasferimento dell'anello, detta $G_{a}(s)$: il DdB del modulo è nella seguente forma:

![[Pasted image 20230516153856.png]]

Chiamiamo **pulsazione di riferimento** al $\omega_{c}$ per il cui DdB attraverso gli 0 dB.
Allora definiamo la zona di **alta frequenza** e di **bassa frequenza** come:
$$
	\textrm{BF}: \omega \ll \omega_{c} \quad \textrm{AF}: \omega \gg \omega_{c} \quad    
$$
In generale vogliamo (perchè **richeiesta dalle specifiche**) che $|G_{a}(s)|_{BF} \gg 1$ e (**perchè il sistema è proprio**) che $|G_{a}(s)|_{AF}\ll 1$
Le relazioni in unità naturali viste sopra diventano in dB:
$$
|G_{a}|_{BF}> 20\,dB \quad |G_{a}|_{AF}<-20\, dB
$$
![[Pasted image 20230516154442.png]]


# Caratterizzazione di W con dinamica dominante del 2° ordine
---
Ricordiamo che per una generica $W$ valgono le seguenti relazioni:
$$
\frac{\omega_{B}}{\omega_{n}} \approx R = \sqrt{ 1- \zeta^2 + \sqrt{ 2-4\zeta^2+4\zeta^4 } }
$$
$$
M_{r} \approx \frac{1}{2 \zeta \sqrt{ 1-\zeta^2 } }
$$
$$
\hat{s}=e^{- \frac{\pi \zeta}{\sqrt{ 1-\zeta^2 }}}
$$
Inoltre abbiamo le seguenti approssimazioni, sempre per $W$ generiche:

![[Pasted image 20230517084121.png]]

