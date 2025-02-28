# Ripassino IPv4

Standard di rete con **indirizzi univoci** per ogni dispositivo connesso alla rete su **32 bit**: composto da **network e host part**.
Una **rete IP** è composta da dispositivi connessi tramite lo stesso livello collegamento e quindi condividono la stessa network part.

I soliti indirizzi speciali riservati:

![[Pasted image 20231011164131.png]]

Poi è possibile distinguere gli indirizzi per classe, ovvero tramite **approccio classful**:
- Classe A, iniziano con 0 e hanno pochi networks ma molti devices
- Classe B, iniziano con 10 
- Classe C, iniziano con 110, moltissimi network ma pochi host ( 254 )

L'approccio **classless** è detto **CIDR, classless interdomain routing** e si basa sulla solita rappresentazione su 32 bit più una **maschera** che specifica quanti bit sono riservati per il network.
Un esempio è 200.23.16.0/23 , dove ci sono 23 bit (più significativi) che identificano la network part e i restanti 9 bit sono per gli host.

Il **Routing** funziona genericamente nel seguente modo: un device IP cerca nella sua **routing table** per un match, e se lo trovo allora instrada il pacchetto. Inoltre nel caso di diversi match si sceglie la destinazione più specifica (**longest prefix matching**)

```ad-important
title: Assegnare gli indirizzi
Quando devo progettare gli indirizzi di una rete, devo sempre ricordare che per $n$ host considerati, devo aggiungere il network id e il broadcast (ad esempio per gestire 2 host ho bisogno di un /30, in quando necessito di 4 indirizzi). Parto ad assegnare le reti da un address specifico, e dove finisce quel network inzio l'altro (facendo attenzione alle maschere)

```

```ad-important
title: Scrivere le routing table
Per prima cosa occorre identificare le reti direttamente connesse al ruoter interessato (fare attenzione se viene dato l'indirizzo delle interfacce).
- Scrivo la destinazione (network) e affianco il next hop (interfaccia)

Per quanto riguarda gli indirizzi statici: se non c'è internet e dobbiamo raggiungere indirizzi tramite altri router conviene usare la default route.

```

### IPv4 Multicast

Il multicast è un concetto che sta nel mezzo tra una comunicazione unicast (1 a 1) e broadcast (1 a tutti). I pacchetti vengono indirizzati da una sorgente verso multiple destinazioni, quindi verso solo alcuni host. Sono dunque presenti dei gruppi a cui degli host possono entrare o uscire. E’ vantaggioso in quanto l’alternativa sarebbe mandare pacchetti uno ad uno in modo molto più lento. Nel multicast viene inviato un solo pacchetto, che viene poi instradato correttamente dal router ai destinatari utilizzando meno traffico (nel broadcast è sempre un pacchetto, ma viene poi mandato anche ai non
interessati).
Il gruppo di host è identificato da un indirizzo di classe D.
Gli host possono entrare ed uscire da gruppi multicast liberamente.

Lavorando a livello collegamento è necessario mappare il gruppo multicast in un indirizzo MAC multicast nel seguente modo:

![[Pasted image 20231016143110.png]]

I primi 25 bit sono fissi, mentre gli ultimi 23 sono gli stessi dell'indirizzo IP multicast.
Questo nuovo indirizzo MAC è l'indirizzo destinatario per i contenuti che voglio trasmettere in multicast: per quanto riguarda gli host destinatari vanno configurati in modo da essere compatibili con quell'indirizzo.

Il protocollo **IGMP** (Internet Group Management Protocol) permette agli switch di configurare le tabelle di inoltro sulla base di dove si trovano gli interessati al multicast.

```ad-note
title: MAC
L'ottavo bit dell'indirizzo MAC dice se quell'indirizzo fisico è unicast o multicast: se è come sopra (1 in rosso) allora è multicast.

```


# Esercizi

![[Pasted image 20231016131650.png]]

Supponendo di fare packet sniffing su host A determinare tutti i pacchetti rilevati.
La stazione B manda un ping sulla rete (che in realtà è C): deve conoscere il MAC di C per effettuare una comunicazione diretta. B manderà in broadcast la ARP request e sarà catturata anche da A, mentre la ARP reply e le ICMP echo request e reply non saranno catturate da A.

![[Pasted image 20231016132248.png]]

Qui host A deve fare una comunicazione indiretta e tramite DNS: per prima cosa occorre tradurre URL in un indirizzo IP:
A invierà una ARP request e otterrà una ARP reply dal DNS.
La terza trama è la DNS query da parte di A: dato che il DNS locale è vuoto dovrà gerarchicamente risalire all'indirizzo (dato che non sappiamo se il DNS opera in modo ricorsivo o iterativo non possiamo sapere quanti pacchetti saranno scambiati fra DNS e default gateway).
La DNS response è il quarto (non contando gli N in mezzo) pacchetto .
Quinto e sesto sono ARP request e reply del default gateway.
Il settimo è la ICMP echo request al server di google.
L'ottavo sarà la ICMP echo reply. x4

![[Pasted image 20231016134124.png]]

Stavolta la cache del DNS è piena e sa mappare google.com, ma su questa rete si usano due reti logiche sulla stessa rete fisica.
La prima cosa che devo fare è tradurre URL, ma per farlo devo conoscere l'indirizzo del DNS (che è sulla seconda rete logica, quindi si passerà dal Default Gateway).
I primi due pacchetti sono una ARP request e reply del DG.
Il terzo pacchetto è una DNS query verso il DG.
Quarto e quinto pacchetto sono ARP per conoscere il MAC del DNS.
Il sesto è l'inoltre della DNS query al DNS.
Settimo e ottavo sono la DNS response sulle due reti logiche.
Poi gli altri 8 pacchetti sono ICMP request e reply.