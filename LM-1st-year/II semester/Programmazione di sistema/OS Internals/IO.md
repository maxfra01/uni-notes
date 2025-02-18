# Struttura BUS

![[Pasted image 20240617155910.png]]

# Polling

Meccanica basata sul **busy waiting**: per ogni byte o dato che vogliamo leggere o scrivere aspettiamo che il dispositivo di I/O sia pronto (o che il dato sia disponibile per essere scritto ).
1. Si deve leggere uno **status register**  in cui si aspetta che il busy bit vada a 0.
2. La CPU setta un bit per indicare se bisogna leggere o scrivere da I/O, ovvero decidere il flusso
3. La CPU setta il ready-bit a 1
4. Il Controller (device) setta il busy bit a 1 ed esegue il trasferimento dati
5. Quando ha finito il controller resetta i suoi registri e mette il busy bit a 0

# Interrupt

Opzione duale al polling, si parla di IO sincrono. Ad un certo punto la CPU può ricevere un interrupt per IO ed attiva un servizio di gestione dell'interrupt.
C'è l'interrupt vector che mappa gli interrupt alle azioni da compiere (IVC): ci sarà dunque un context switch, una gestione della priorità.

![[Pasted image 20240617215155.png]]

# Direct Memory Access

Se dobbiamo trasferire grosse moli di dati, possiamo evitare di fare IO programmato (tramite polling o interrupt): se è disponibile un **DMA controller** esso prende il controllo del bus senza chiedere alla cpu (la cpu dice solo la source, destination e size).
Il DMA gestirà il bus tramite il cycle stealing (ovvero mentre la cpu non usa il bus è il DMA controller che lo usa).

![[Pasted image 20240617215752.png]]

# Interfaccia all'applicazione

Le applicazioni fanno IO tramite syscall in genere :

![[Pasted image 20240617220009.png]]

Gli IO devices sono molteplici:
- Quelli che fanno IO a blocchi
- Gli stream di caratteri
- Memory mapped file access
- Sockets

IO **Bloccante**: il processo che esegue l'IO è fermo finchè non si completa (a)
IO **Non Bloccante**: il processo non si ferma, l'IO ritorna un risultato ma potrebbe non essere consistente
IO **Asincrono**: un processo va avanti nel mentre che un IO è in corso (b)

![[Pasted image 20240617220931.png]]

# Kernel IO subsystem

Primo problema è lo **scheduling**: se ho più richieste devo scegliere chi servire, posso dunque usare FIFO priorità ecc...

Con **Buffering** invece intendiamo il passaggio dei dati in memoria principali prima di essere trasferiti in un dispositivo (ad esempio da processo a dispositivo). Di fatto i dati fanno un passaggio in più, ma lo sa perchè i dispositivi hanno velocità di trasmissione diverse. Un altro motivo è se c'è un mismatch di dimensioni dei file da trasferire.

Il **Double buffering** è una tecnica  che fa uso di due buffer, in uno si crea l'informazioni riempiendolo, mentre nell'altro c'è l'informazione completa. A un certo punto si scambiano di ruolo.

Il **Caching** permette di fare una copia di un dato in un dispositivo veloce, se devo fare un trasferimento prima guardo nel dispositivo veloce, poi in quello lento.

Lo **Spooling** è una gestione di parcheggio di un dato e una coda FIFO (stampante)

# Syscall IO

La gestione dell'IO deve essere privilegiata ma si vogliono evitare comportamenti illeciti: dunque si ricorre alle **syscall**:

![[Pasted image 20240618100818.png]]

Supponiamo di fare una lettura di un file dal disco:
1. Determinare il device
2. Tradurre il nome del device in una locazione
3. Si leggono i dati dal disco
4. Si rendono disponibili i dati al processo
5. Si da il controllo al processo

![[Pasted image 20240618101502.png]]