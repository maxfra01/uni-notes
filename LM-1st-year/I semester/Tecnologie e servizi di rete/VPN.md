# Virtual Private Network

Una **VPN** è un insieme di tecnologie per permettere a diverse sottoreti distinte di **comunicare come se fossero un'unica rete privata**.
Si cerca quindi di far comunicare due sottoreti mantenendo le stesse politiche di sicurezza, affidabilità e QoS.

```ad-important
title: VPN e sicurezza
L'utilizzo di una VPN non rende automaticamente la connessione **sicura**.

```

I due elementi base che compongono la VPN sono:
- **Tunnel**: Tramite il tunnel è possibile **incapsulare** in modo sicuro il traffico in transito sulla rete condivisa (non è presente in alcune soluzioni)
- **VPN gateway**: E' il componente che **apre e termina i tunnel**, questi gateway devono supportare diversi protocolli specifici per il tunnelling.

I motivi per cui le VPN sono ampiamente utilizzate sono principalmente due: il primo riguarda l'abbattimento dei costi per predisporre effettivamente delle reti private. Il secondo riguarda la possibilità di accesso flessibile ai servizi di un azienda (impiegati da remoto accedono ai servizi tramite VPN).

# Definizioni e scenari 

- **End point authentication**: si verifica che il destinatario o il mittente siano effettivamente chi dicono di essere
- **Data integrity**: ci si assicura che i dati non cambino
- **Data confidentiality**: ci si assicura che i dati non siano acceduti/letti da qualcun altro oltre alla destinazione
- **Data separation**: è il tunneling

### Tunneling

Processo per il quale si instaura una connessione fra 2 VPN gateway: i pacchetti che entreranno nel tunneling saranno complementati da un **nuovo header** che ha come sorgente e destinazione i VPN gateways e non gli indirizzi degli host stessi.

Esistono diversi situazioni e scenari differenti:
- **Site to Site**: situazione classica, le VPN sono nate proprio per connettere due site di aziende diverse![[Pasted image 20231208110342.png]]
- **End to End**: in questo caso il compito del VPN gateway è svolto direttamente dall'host, senza un sistema dedicato ![[Pasted image 20231208110520.png]]
- **Remote VPN**: scenario usato da dipendenti che lavorano da remoto ![[Pasted image 20231208110700.png]]

### Intranet ed extranet

- Nel caso di **Intranet** prendiamo in esame un azienda dotata di una rete privata, dove tutti i dipendenti si collegano per lavorare ![[Pasted image 20231208110931.png]]

- Nel caso di **extranet** potrebbero esserci altri enti che accedono alla rete aziendale, come i clienti o aziende partner ![[Pasted image 20231208110945.png]]

L'unica effettiva differenza sta nella **sicurezza** dei vari servizi: vado a coprire con **firewall** anche settori della rete dove accedono i dipendenti.
Un altro problema di una extranet riguarda la **sovrapposizione degli indirizzi**, in quanto porzioni diverse di rete sono configurate da persone diverse e potrebbero esserci incompatibilità.

### Accesso ad Internet

In un primo approccio **centralizzato**, voglio usare la VPN per accedere genericamente ad internet:

![[Pasted image 20231208111447.png]]

Quello che notiamo è che il pacchetto, dalla casa, viene comunque mandato al VPN gateways che poi esce su internet.
E' un approccio che garantisce l'uso del tunnel nel primo tratto ma è molto costoso in termini di tempo, perchè la strada è più lunga.

Con un approccio **distribuito** è possibile uscire su internet direttamente dall'host:

![[Pasted image 20231208111615.png]]

Così facendo però vado a **perdere il mio tunnel** e quindi il traffico non è più sicuro e protetto: perdo i servizi di una VPN in sostanza, è comodo solo per le **performance**.

### Virtual VPN topologies

- **Hub and Spoke**: ogni site crea un tunnel verso un HUB centrale. Per comunicare prima si contatta HUB che a sua volta contatta un host
- **Mesh**: Sistema di più tunnel, configurazione più complessa. Per comunicare si contatta direttamente l'host 

# Deployment Models, Provisioning, Models & VPN Protocol Layers

![[Pasted image 20231208112728.png]]

### Deployment mode

Nel primo caso di parla di **Overlay**: la rete pubblica (ISP) **non partecipa alla realizzazione della VPN**, di fatto non sa dove sono i gateways ma fornisce solo connettività.

Nella versione **Peer** il VPN gateways interagisce con un **router pubblico**: a questo punto l'ISP sa che esiste una VPN.

### Provisioning model

Nel caso di **Customer provider** è l'azienda stessa a implementare la VPN, mentre il network provider non sa nulla.

![[Pasted image 20231208113940.png]]

Nel **Provider provisioned** anche ISP partecipa alla creazione della VPN.

![[Pasted image 20231208113959.png]]

Customer Provisioned:
- Remote host has 2 addresses
- ISP assigned and corporate
- Remote host terminates VPN tunnel
- Remote host must activate tunnel
- If tunnel is not activated, client can operate without VPN
- Can be used from any Internet connection (ISP)

Provider provisioned:
- Remote host has 1 address (corporate)
- NAS terminates VPN tunnel
- Remote host is always on VPN
- Internet access is only centralized
- Requires access to specific ISP

### VPN layers

- **Layer 2**
- **IP in IP**: ![[Pasted image 20231208114757.png]]

- **Tunneling level 3**: ![[Pasted image 20231208114827.png]]

- **Layer 4**: ![[Pasted image 20231208114851.png]]

# Generic Routing Encapsulation

Algoritmo per **incapsulare i pacchetti IP** all'interno di altri pacchetti (già visto in [[IPv6]]).
Il formato del pacchetto GRE è il seguente:

![[Pasted image 20231207144459.png]]

All'interno del campo **Protocol** ci sarà un codice che identifica il protocollo corrispondente al pacchetto incapsulato.
GRE permette inoltre di fare **source routing** (la sorgente sceglie il percorso da seguire).
Oltre a questa versione "base" esiste una versione **enhanced**, molto simile al protocollo TCP, infatti implementa i campi **ACK e Sequence number**:

![[Pasted image 20231207144717.png]]

Inoltre ci sono altre features di questo GRE come ad esempio il **flow control**, gestione dei pacchetti **fuori ordine**, controllo di **congestione**.

# Incapsulamento del livello 2

Due soluzioni:
- **L2TP**: Layer 2 Tunneling protocol, provider provisioner, protocollo molto usato 

- **PPTP**: Point to Point Tunneling Protocol, customer provisioner, proposto e sviluppato da Apple, Microsoft ecc..., ad oggi deprecato per basso livello di sicurezza

### L2PT

Per una soluzione Site 2 Site, l'host si collega al **LAC** (L2TP Access Concentrator). Mentre al alto ricevitore c'è **LNS** (L2TP Network Server). Si instaura un tunnelling fra i due dispositivi e all'interno di questo tratto, il pacchetto avrà un header L2TP.

![[Pasted image 20231207145640.png]]

Di fatto è possibile che la funzione del LAC sia svolta direttamente dal host.
All'interno di un tunnel possono essere presenti più **sessioni**: per questo esiste una sessione dedicata del tunnel, detto **CC** (control connection) per scambiare informazione di controllo fra LAC e LNS.

Procedura per la connessione:
1. Scambiare informazioni di controllo fra LAC e LNS, tramite control connection
2. Instaurare una o più sessioni

A questo punti i pacchetti sono caratterizzati da questo **header L2TP**:

![[Pasted image 20231207150205.png]]

Notiamo che è presente un **Tunnel ID, Session ID** e **flags** per capire se si tratta di messaggi di controllo o dati.

### Sicurezza di L2TP

C'è **Tunnel endpoint authentication**, ciò significa che andiamo ad autenticare il creatore del tunnel, ma non tutti i pacchetti che vengono inviati sulla rete.
Con altri protocolli, come **IPsec** è possibile fornire altri parametri di sicurezza, come **Encryption, Authentication (E2E) e integrity**.

L'idea generale è quindi quella di usare L2TP per creare i tunnel e successivamente tramite IPsec lo rendiamo sicuro.

### PPTP

![[Pasted image 20231207150706.png]]

In questo scenario un utente si collega a un **PNS** (PPTP Network server): il tunnel è creato direttamente dal host verso la rete obiettivo (è lo scenario delle VPN usate in abbonamento).
Ad oggi è deprecato per il problema della sicurezza.

I pacchetti hanno header differenti per dati e controllo:

![[Pasted image 20231207150903.png]]

# IPsec VPN

E' un **protocollo** IP pensato per aggiungere un maggiore livello di sicurezza: una prima aggiunga è un **Authentication header (AH)** che ci permette di fare **source authentication** e **data integrity**.

![[Pasted image 20231207151253.png]]

Per aggiungere **confidenzialità** al pacchetto IP dobbiamo usare un header aggiuntivo **ESP (Encapsulating security payload)**: garantisce allo stesso tempo confidenzialità, autenticazione e integrità. Lo svantaggio sta in un header decisamente più complesso.

![[Pasted image 20231207151528.png]]

In certe versione è possibile **combinare AH e ESP**, e infatti i VPN gateway possono aggiungere ai pacchetti questi header:

![[Pasted image 20231207151700.png]]

I VPN gateway possono procedere in due modi:
- **Transport mode**
- **Tunnel mode**: Al pacchetto standard viene aggiunto un header IPsec, successivamente questo nuovo pacchetto appena ottenuto è nuovamente imbustato in IP, il quale sarà quello effettivamente mandato ![[Pasted image 20231207152006.png]]
