# Concetti di Base
---
Durante una comunicazione in rete è fondamentale rispettare questi aspetti:
- **Autenticazione**: ad entrambi i lati (da ora A e B) bisogna essere certi della reciproca identità
- **Riservatezza**: Solo A e B devono essere in grado di comprendere il messaggio.
- **Sicurezza del sistema**: esso deve essere robusto rispetto ad attacchi verso di lui
- **Integrità dei messaggi**: ovvero dobbiamo essere sicuri che non ci siano state manipolazioni e che il messaggio ricevuto sia corrisponodente all'originale.

# Cifrature a chiave simmetrica
---
Nei sistemi a chiave simmetrica **solamente A e B conoscono la chiave segreto, che è identica fra i due**.
Un esempio di cifratura simmetrica è il "Cifrario di Cesare", oppure il "cifrario Polialfabetico", che usa più cifrari di cesare in un pattern preciso.

### Cifrario a blocchi

Il **Cifrario a blocchi** è uno dei più utilizzati. Per usarlo si partiziona il messaggio $m$ in blocchi da $k$ bit. Ogni blocco poi è cifrato in modo indipendente usando una tabella conosciuta da A e B
Così facendo abbiamo ben $2^k$ entry della tabella che sono molto, numeri tipici sono blocchi da 64 bit.

A volte per cifrare un messaggio si preferisce usare un ciclo di $n$ iterazioni diviso come segue:
1. si ottiene in input il blocco da $k$ bit 
2. si partiziona il blocco in **chunks** e ognuno viene cifrato in maniera indipendete con una sua tabella
3. si ricompone il blocco completo e si reitera

Algoritmi noti sono **DES (Dara Encryption Standard)**, blocchi da 64 e chiave da 56 bit, oppure **AES (Advanced Encryption Standard)** con blocchi da 128 e chiave da 128,192 o 256 bit.

### Cifrario a blocchi concatenato

Il problema del cifrario a blocchi è che si possono riconscere alcuni pattern. Per ovviare a ciò si usano i seguenti passaggi.
Il **Cipher Bloch Chaining (CBC)** funziona come segue:
Consideriamo un messaggio in chiaro da $k$ bit, $m(i)$.
1. Quando A trasmette, genera un numero causale $r(i)$ da $k$ bit e calcola $c(i)=K_{S}(m(i) \space XOR \space r(i))$ 
2. A invia la coppia $(c(i),r(i))$
3. B riceve la coppia e ottiene il plaintext tramite $m(i)=K_{S}(c(i) \space XOR \space r(i))$
Così facendo anche se due messaggi sono uguali in plaintext avranno una cifratura diversa eliminando i pattern.

Condividera ogni volta il numero $r(i)$ è costoso in banda, dunque al posto che questo meccanisco si condivide **in chiaro** all'inizio della comunicazione un vettore di $k$ bit chiamato **Initialization Vector, IV** che denotiamo con $c(0)$.
Procediamo a cifrare come segue:$$
c(i)=K_{S}(m(i)\space XOR \space c(i-1))
$$

# Cifratura a chiave pubblica
---
Supponiamo che A voglia comunicare con B, allora B mette a disposizione una chiave pubblica $K_{B}^+$ con cui si crittografa il messaggio, poi B userà la sua chiave privata per decrittografare $m(i)=K_{B}^-(K_{B}^+(m(i)))$
Ma come scegliere la chiave pubblica e privata?

### RSA, non quella dei vecchi

Per ottenere le due chiavi, si procede come segue:
1. Si scelgano due numeri $p,q$ **primi e molto grandi**, dopodichè si calcoli $$
n=pq \quad \quad z=(p-1)\cdot(q-1) 
$$
2. Trovare un numero $e$ che è minore di $n$ e non abbia fattori comuni con $z$ ($e,z$  coprimi)
3. Trovare un numero $d$ tale per cui $$ed \space \textrm{mod} \space z=1$$
Fatto ciò la chiave pubblica sarà la coppia $(n,e)$ mentre la chiave privata sarà $(n,d)$.
Il messaggio si crittografa facendo:
$$
c=m^e \space \textrm{mod} \space n  
$$
e si può invertire facendo:
$$
m=c^d \space \textrm{mod} \space n 
$$
Funziona perchè non esistono attualmente algoritmi noti per la fattorizzazione (problema NP).
Molto comodo e sicuro, ma richiede numeri grossi e computazionalmente molto costosi perciò spesso si usa RSA per scambiarsi una chiave di sessione e poi si usa DES o AES molto più rapidi.

# Funzioni Hash Crittografiche
---
Sono particolarmente importanti per l'**integrità del messaggio**.
Consederiamo sempre lo scenario in cui A comunica con B.
L'idea è la seguente:

1. A calcola $h=H(m)$ e invia a B (con l'algoritmo che preferisce) la coppia $(m,h)$
2. B riceve la coppia e calcola a sua volta l'Hash di $m$ per vedere se corrisponde a quello ricevuto.

Questo però non garantisce l'**autenticazione di A**.

Per ovviare a ciò occorre un segreto condiviso, ovvero la **Authentication Key $s$**.
Si procede così:

1. Alice crea $h=H(m+s)$ , noto come **MAC Message Authentication Code**, poi manda $(m, h)$
2. B riceve e calcola a sua volta il MAC, e poi confronta

Ad oggi lo standard è **HMAC**.

# Firme digitali
---
Meccanismo analogo alla firma vera su carta, non si può usare il MAC perchè implicherebbe che il destinatario conosco la chiave segreta, perciò non valida.
La soluzione la si trova nell'applicazione della RSA.
Un messaggio $m$ che deve essere firmato viene crittografato con la chiave **privata** di A e si invia la coppia $(m,h=K^-(m))$.
Per verificare la firma basta applicare la chiave pubblica e si ottiene $m$ allora la firma è confermata.
Inoltre ciò basta per confermare oltre che l'autenticazione, anche l'integrità del messagio stesso in quanto l'hash è univoco per messaggio.

RSA è lento, quindi si potrebbe preferire far firmare ad A non il messaggio, ma l'hash del messaggio, di lunghezza fissa e minore del messaggio stesso.

### Certificazione di chiave pubblica

Considero che B voglia comunicare con A e firmi l'hash.
Per essere sicuri che la firma sia valida occorre anche **verificare che la chiave pubblica che ho sia veramente quella di B**.

Esiste per questo motivo la **Certification Authority (CA)** che ha il compito di verificare l'identità e generare un **certificato** da allegare insieme alla chiave pubblica.
Tipicamente il certificato è un ID univoco globale, inoltre è firmato dalla CA.



