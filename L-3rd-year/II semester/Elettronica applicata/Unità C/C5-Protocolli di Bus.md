# Protocolli di transazione
---
Utilizzando sempre configurazioni master slave allora, **in sistemi multi punto**, è necessario gestire chi utilizza il bus. Ciò avviene in 3 fasi:
- Nella fase di **Allocazione** si decide chi è il master che opera sul bus
- Nella fase di **Indirizzamento** si indiviuda lo slave
- Infine avviene il vero e proprio **trasferimento**

In un contesto di **master unico e multislave** la fase di allocazione è ovviamente inutile, in quanto già individuato: è presente solo la fase di indirizzamento e di trasferimento:

![[Pasted image 20230502171225.png]]

### Unità slave

Sono dotate di meccanismi di:
- **Decodifica di indirizzo**, tramite decoder, comparatori e porte logiche.
- **Temporizzazione e controllo**
- **Core** dello slave, ovvero le memorie vere e proprie, unità operative, registri
- **Buffer dati**, ovvero i driver e i receviver che si interfacciano al bus

![[Pasted image 20230502171442.png]]

# Protocolli di arbitraggio
---
Tipici protocolli di arbitraggio:
- Token passing
- FCFS
- Priority (può portare a starvation)
- Collision detection
- Arbitraggio esplicito (con logica di controllo)


# Prestazioni
---
Definiamo il **throughput** del bus come la sua larghezza per la velocità (in cicli al secondo):
$$
T=W \cdot S
$$
Per migliorare le prestazioni del bus è possibile usare il **Double Data Rate** dove si sfruttano sia la transizione alto basso che la basso alto:

![[Pasted image 20230502173450.png]]

E' inoltre possibile usare il **trasferimento in burst** per trasferire blocchi successivi senza ripetere l'indirizzamento.