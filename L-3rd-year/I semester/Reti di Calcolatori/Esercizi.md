# Esercizio 1
---
![[Pasted image 20221212151349.png]]

Si effettua un ping da A verso B, dunque (dato che siamo in MS) ci ricordiamo che ci basiamo su ICMP request e response.
Innanzitutto A si chiede se B sia sulla stessa rete, lo è dunque effettua una ARP request per scoprire il MAC

| Source MAC | Dest MAC | Type of Message | IP Source | IP Dest |
| ---------- | -------- | --------------- | --------- | ------- |
| A          | 1        | ARP request     | -         | -       |
| B          | A        | ARP reply       | -         | -        |

Ora A conosce il MAC di B, passa alla echo request

| Source MAC | Dest MAC | Type of Message | IP Source | IP Dest |
| ---------- | -------- | --------------- | --------- | ------- |
| A          | B        | Echo Request    | IP di a   | IP di B |
| B          | A        | Echo Reply      | IP di b   | IP di B        |
x4

Essendo su Windows si fanno 4 echo request e 4 reply.
Dunque in totale sono 10, come specificato dall'esercizio.


# Esercizio 2
---
![[Pasted image 20221212151949.png]]

In questo caso ho bisogno del DNS, in rete locale che conosce l'IP di polito.

| SOURCE MAC | DEST MAC | Type         | IP SOURCE | IP DEST |
| ---------- | -------- | ------------ | --------- | ------- |
| mac di a   | 1        | arp request  | -         | -       |
| mac di d   | mac di a | arp reply    | -         | -       |
| mac di a   | mac di d | dns query    | ip di a   | ip di d |
| mac di d   | mac di a | dns response | ip di d   | ip di a |
| mac di a   | 1        | arp request  | -         | -       |
| mac di b   | mac di a | arp reply    | -         | -       |
| mac di a   | mac di b | echo requesr | ip di a   | ip di b |
| mac di b   | mac di a | echo reply   | ip di b   | ip di a        |
Di cui gli ultimi due pacchetti x4, siamo sempre su windows.


# Esercizio 3
---
![[Pasted image 20221212152633.png]]

Qui per A pasa solo un pacchetto dell'arp request in broadcast, poi gli altri passano da B a C.
Questo perchè la rete è regolata da switch!

| Source MAC | Dest MAC | Type        | IP source | IP dest |
| ---------- | -------- | ----------- | --------- | ------- |
| mac di b   | 1        | arp request | -         | -        |


# Esercizio 13
---
![[Pasted image 20221212152835.png]]

Per prima cosa dobbiamo risolvere l'indirizzo url sul browser, per farlo andando su internet serve il default gateway.
Il default gateway si occupa di fare query al dns

| Source MAC | Dest MAC | Type          | IP Source    | IP Dest   | Note                                                             |
| ---------- | -------- | ------------- | ------------ | --------- | ---------------------------------------------------------------- |
| mac di A   | 1        | ARP request   | -            | -         | dovendo andare su internet serve l'indirizzo del default gateway |
| mac di E   | mac di A | ARP reply     | -            | -         |                                                                  |
| mac di A   | mac di E | DNS query     | ip di A      | ip di E   | A chiede al DNS di risolvere l'indirizzo url                     |
| mac di E   | 1        | ARP Request   | -            | -         | Il DG chiede chi ha l'ip del DNS                                 |
| mac di D   | mac di E | ARP reply     | -            | -         | Il DNS risponde                                                  |
| mac di E   | mac di D | DSN Request   | ip di E      | ip di D   | IL DG chiede l'ip di google.it                                   |
| mac di D   | mac di R | DNS reply     | ip di E      | ip di D   | Il DNS risponde                                                  |
| mac di E   | mac di A | DNS reply     | ip di E      | ip di A   | Il DG informa l'host A dell'indirizzo IP di google.com           |
| mac di A   | mac di E | TCP SYN       | ip di A      | ip google |                                                                  |
| mac di E   | mac di A | TCP SYN-ACK   | ip google    | ip di A   |                                                                  |
| mac di A   | mac di E | HTTP GET      | ip A         | ip google |                                                                  |
| mac di E   | mac di A | HTTP Response | ip di google | ip di A   |                                                                  |
| mac di A   | mac di E | TCP-FIN       | ip di A      | ip google          |                                                                  |


