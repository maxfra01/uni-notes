# WDM: Wavelength Division Multiplexing

E' possibile fare **multiplexing**, ovvero mandare **più segnali all'interno della stessa fibra**: ogni segnale avrà una diversa lunghezza d'onda.
Esistono due soluzioni: la prima è detta **dense** (DWDM) dove ci sono molte lunghezze d'onda (più costoso), mentre la seconda è **coarse** (CWDM) dove ci sono poche lunghezze d'onda (più economico).

![[Pasted image 20231207152749.png]]

L'idea generale è quella di trasmettere più bit in parallelo, andando ad **aumentare la capacità della fibra** stessa. Dal punto di visto degli operatori si aumenta il ROI.

E' successivamente possibile realizzare sistemi per cambiare la lunghezza d'onda (**wavelength switching**) dei segnali, per poi combinare insieme di segnali differenti:

![[Pasted image 20231207153325.png]]

La rete dell'operatore avrà un **backbone** composto da collegamenti che possono gestire la fibra, mentre i collegamenti ai vari router sono tradizionali.
All'interno del backbone ci sono gli **switch ottici** che sono in grado di gestire canali in fibra ottica.
I router si velocizzano in quanto la rete interna è più veloce.

```ad-note
title: Optical Switch
In questo scenario gli switch ottici trasportano **segnali**, non pacchetti. In altre parole non c'è nulla di legato ad IP, ma i pacchetti vengono instradati in questo backbone una volta convertiti in segnali ottici. 

```


![[Pasted image 20231207153548.png]]

# Optical switching

#recuperare ultimi 10 minuti