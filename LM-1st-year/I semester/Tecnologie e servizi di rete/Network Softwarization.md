# Network Softwarization

Il più grande ambito di ricerca sulla reti al giorno d'oggi si basa sul **rivalutare gli approcci tradizionali** dell'architettura di rete, per favorire nuove tecnologie:
- Cloud computing
- Big data
- Mobile Traffic
- IoT
Il problema principale riguarda i **pattern del traffico**.
L'architettura tradizionale è basato su TCP/IP, con il metodo client server, dove il server rimaneva la macchina più potente e il client no: ora non è più così, **si cerca un approccio più distribuito**.

4 Limitazioni dell'architettura tradizionale (dettata da ONF) sono:
- Architettura statica e complessa
- Policies inconsistenti
- Non scalabilità
- Dipendenze da un venditore

# Software-Defined Networking SDN

Un network device è partizionato fra **planes**: control, management e data plane (si occupa del forwarding).
Originariamente ogni dispositivo aveva scritti i 3 piani.

![[Pasted image 20231129180610.png]]

Il paradigma SDN **separano il Data plane e control Plane**: il piano di controllo diventa **unico e centralizzato** (diventa un SDN controller).
Un unico dispositivo (dove scrivo policy ed algoritmi) comanda uno o più dispositivi (ad esempio degli switch): 

![[Pasted image 20240115154842.png]]

SDN non è un nuovo layer ma una **riorganizzazione** dell'architettura di rete muovendo alcune funzioni in altri posti.

I pilastri di SDN sono 4:

- **Separazione di data e control plane**

Per il routing, ora gli switch si occupano solamente del packet forwarding. La logica di routing è tutta all'interno del SDN controller, che è una macchina programmabile (tramite Python, Java, ...  quindi alla portata di tutti). La comunicazione fra SDN controller e switch è detta **southbound interface** (usa il protocollo **OpenFlow**)

- **Data plane semplice, "plumbing"**

I dispositivi switch diventano molto più semplici, in quanto le uniche azioni che fanno sono il forwarding o il drop di un pacchetto, a seconda di semplici confronti/match.

- **Centralized control**

A differenza di routing con distance vector o link state, in questo contesto un SDN controller ha la **visione dell'intera rete**, e quindi posso immaginare una porzione di rete gestita in SDN come un **big switch**: posso addirittura definire dei comandi ad alto livello per configurare la mia rete in un certo modo.

Il controllo dev'essere **centralizzato solo logicamente**:

![[Pasted image 20240115160953.png]]

Avere più controller fisici contribuisce ad avere una rete più robusta e scalabile, a fronte di una maggiore complessità.

- **Context-based forwarding**

Non originariamente presente in SDN, ma nella sua prima implementazione OpenFlow molto utile: il piano di controllo prevede che si possano prendere decisione runtime e quindi modificare il percorso di un pacchetto in base al contesto e al tipo di traffico.

![[Pasted image 20240115161527.png]]
# OpenFlow

E' un protocollo che rappresenta la prima implementazione di SDN, supportando tutti e 4 i pilastri di quest'ultimo.
- Imposta le regole di forwarding e le inoltra agli switch
- Invia al controller i pacchetti che non corrispondono a nessuna regola
- Può fare query allo switch per ottenere statistiche

Per il forwarding ci basiamo su un meccanismo di **match/action**: se un pacchetto corrisponde (match) con una certa regola, allora si procede ad eseguire l'azione corrispondente
Le forwarding table in OpenFlow hanno una struttura simile:

![[Pasted image 20240115161715.png]]

Se un pacchetto non segue nessuna regola viene solitamente inviato al controller, che a sua volta può modificarlo, e rimandarlo indietro con le corrette informazioni per il forwarding (una nuova regola).

OpenFlow supporta sia:
- **Reactive configuration**: quando si manda un pacchetto al controller c'è un piccolo delay per l'applicazione della nuova regola
- **Proactive**: le regole sono preparate prima, quindi non c'è delay

Le regole hanno il seguente formato:

![[Pasted image 20240115174907.png]]

# Network function virtualization (NFV)

Sull'onda dell'innovazione fornita da SDN, possiamo vedere tutti i dispositivi che un pacchetto attraversa, come una macchina virtuale che fornisce certe funzionalità. Andiamo quindi a **virtualizzare le funzioni di vari dispositivi** (vado così ad evitare l'acquisto di dispositivi extra):

La sequenza di dispositivi (in NFV sono software in pratica) che un pacchetto attraversa è detto **Service Function Chain (SFC)**:

![[Pasted image 20240115175750.png]]

Per collegare fra loro i servizi della SFC è possibile usare uno switch OpenFlow (uso quindi SDN) per fare "plumbing" tra i componenti:

![[Pasted image 20240115180023.png]]

Da NFV ottengo diversi benefici:
- **Virtualization**
- **Orchestration**
- **Programmable**
- Scaling
- Automation
- Visibility
- Performance
- Multi-tenancy
- Service integration
- Openness
