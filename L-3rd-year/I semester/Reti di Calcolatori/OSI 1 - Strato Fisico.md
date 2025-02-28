# Mezzi trasmissivi
---
Il mezzo fisico ottimale è caratterizzato da:
- Resistenza, capacità trasmissiva e impedenze basse
- Resistenza alla trazione
- Flessibilità
- Facilità dei collegaementi

## Doppino
Classimo mezzo della telefonia, composto da due fili di rame ritorti (per ridurre le interferenze elettromagnetiche).
Hanno costi ridotti e l'installazione è semplice

Il connettore del doppino è il RJ-45.

![[Pasted image 20221019134040.png]]

Esistono classificazione dei doppini in base alla schermatura:
- UTP-Unshielded Twisted Pair
- FTP-Foiled Twisted Pair
- STP-Shielded Twisted Pair
Inoltre distinguiamo i doppini in base alla categoria (**Cat.**)

## Cavo Coassiale
Mezzo fisico realizzato con un connettore centrale e una o più calze di schermo.
Grazie a quest'ultimo (principio della gabbia di Faraday) otteniamo molte meno interferenze.
Costi elevati e difficoltà di installazione.
Spesso usati in connessioni verso antenne radio.

![[Pasted image 20221019134515.png]]

## Fibra Ottica
Realizzato con un piccolo e flessibile filo di vetro costituito da due parti, **Core e cladding** con indici di rifrazione diversi.
Principio di funzionamento: un raggio luminoso che entra con un certo angolo di accettazione rimane confinato nel core (Legge di Snell).

![[Pasted image 20221019134735.png]]

| Vantaggi                             | Svantaggi                                              |
| ------------------------------------ | ------------------------------------------------------ |
| Immanità da disturbi elettromagntici | Adatte solo a collegamenti punto-punto                 |
| Alte velocità                        | Difficili le interconnesioni dei cavi e dei connettori |
| Bassa attenuazione                   | Raggio di curvatura del cavo fisico deve essere basso  |
| Dimensioni ridotte, costi contenuti  | Soffre di vibrazioni                                                       |

L'attenuazione e perdita di segnale al km è sicuramente il fattore più rilevante: per evitare il problema individuiamo tre "finestre" di lavoro centrate alle lunghezze d'onda di $0.8 \mu m$ , $1.3 \mu m$ , $1.55 \mu m$ .

![[Pasted image 20221019135603.png]]

## Canale Radio
Detto anche Etere, le informazioni si propagano mediante l'uso di antenne.
Se TX o RX sono in movimento si parla di *radiomobile*.
La qualità del segnale dipende dal rapporto tra potenza ricevuta e potenza trasmessa.
$$
\frac{P_{R}}{P_{T}}=G_{T}G_{R} \frac{\lambda^2}{(4\pi D)^2}
$$
(Eq. di Friis, propagazione nello spazio libero)
La potenza in ricezione è influenzata da:
- Fenomeni atmosferici
- Interferenze di altre sorgenti
- Osticali
In particolare la presenza di ostacoli determina **riflessioni e copie** del segnale trasmesso.
- **Fading**: variazione veloce del segnale dovuto alle altre copie ricevute
- **Shadowing**: variazione lenta dell'ampieza del segnale

![[Pasted image 20221019140408.png]]



# Trasmissione sul mezzo fisico
---
A seconda del mezzo trasmissivo, per ricostruire l'informazione legata al flusso di bit si utilizzano tecniche di:
- Codifiche di linea (mezzi elettrici e ottivi, cavi)
- Modulazioni digitali (mezzi radio)

## Codifiche di linea
Un primo tipo di codifica **unipolare** molto semplice consiste nell'associare una tensione nulla al valore 0 e una positiva al valore 1.
Problemi associati a questa tecnica sono la perdita di sincronismo e il sovraccarico della sorgente in caso si lunghe sequenze di 1.

Le codifiche **polari** associano due diversi livelli di tensione:
- NRZ (non return to zero), nelle transizioni alto basso non c'è significato
- RZ, dove la transizione è su tensione nulla fra due bit consecutivi
- Bifase, dove ogni bit è rappresentato da due livelli di tensione di polarità inversa.

![[Pasted image 20221024160649.png]]

Le codifiche bipolari associano la tensione nulla allo $0$ mentre l'$1$ corrisponde a due polarità inverse.
Sono dette **AMI, alternate mark inversion**, e sono rappresentabili su 3 bit.

![[Pasted image 20221024160941.png]]

Le **Codifiche nBmB** permettono di rappresentare n bit su bit diversi generalmente con $n<m$, e sono poplari perchè richiedono meno banda, limita la componente continua e introduce i caratteri speciali.


## Multiplazioni digitali
Le informazioni sono contenute nel segnale sinusoidale **portante** che varia in ampiezza, frequenza, fase e le loro combinazioni.

![[Pasted image 20221024161246.png]]


# Reti di accesso e di trasporto
---
Le **Reti di accesso** comprendono apparati e mezzi trasmissivi che collegano l'utente con il nodo di accesso del fornitore sel servizio.
Le **Reti di trasporto** sono destinati al transito fra due o più nodi e sono proprietà di diversi gestori.

## Reti di accesso
Realizzate mediante 3 principali tecnologie, xDSL, PON, Reti cellulare.

### ADSL
L'**ADSL** è la più antica della famiglia della tecnologia **Digital subscriber line** e la A sta per asimmetrica in quanto ha velocità maggiori in downstream che in upstream.
L'accesso è attraverso il modem (modulatore e demodulatore) che si occupa di convertire il segnale da analogico a digitale e viceversa per renderlo trasportabile sull'infrastruttura del telefono fisso (componenti fondamentali sono il filtro splitter **A** che separa voce e dati).

![[Pasted image 20221024162748.png]]

Miglioramento della ADSL è la VDSL ma ha il problema che al crescere delle distanze le prestazioni calano.

![[Pasted image 20221024162931.png]]

Le **Passive optical Network (PON)** sono un complemento all'ultimo miglio della fibra ottica; in particolare sopra la classica cabina sulla strada, troviamo la **ONU Optical Network unit** che raccoglie le terminazioni di tutti i cavi in fibra ottica per portare i dati sull'anello (DSLAM).

![[Pasted image 20221025165915.png]]


## Reti di trasporto
Rete che crea la connessione fra le reti di accesso, come nodi ha i **router** connessi in una topologia a maglia gerarchica.
La trasmissioni su queste reti è interamente digitale e si basa su multiplazione gerarchica a divisione di tempo.

![[Pasted image 20221025153248.png]]

L'attuale infrastruttra delle reti di trasporto è basata sulle tecnologie:
- SONET
- SDH
- STS
Sovente la topologia è ad anelli bidirezionali per motivi di affidabilità.

In SONET/SDH si trasmette una sequenza continua di bit a una certa velocità, con tecnica della multiplazione di tempo; ogni trama temporale contiene **informazioni di sincronizzazione, canali vocali di servizio, gestione guasti ed errori**.



strato successivo: [[OSI 2 - Strato Collegamento]]