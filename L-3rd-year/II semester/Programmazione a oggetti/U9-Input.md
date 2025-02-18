# I/O Stream
---
Tutte le operazioni di I/O si basano sullo stream, e in particolare per prendere in input byte o char si usa la libreria `java.io`.
Sono presenti due classi: reader e writer
Nel caso dei byte abbiamo InputStream e OutputStream.

L'eccezione principe è la IOException

![[Pasted image 20230507104201.png]]

Nel solo caso di testo, allora prima di essere memorizzati vengono codificati.
Questo perchè non c'è corrispondenza univoca fra byte e caratteri.

# Lettura di caratteri
---
Tutte le operazioni sono fatte dalla classe padre Reader:

![[Pasted image 20230507104422.png]]

I metodi principali sono:

```java
void close();

//legge un singolo carattere, -1 al EOF
int read();

//legge i dati in un buf
int read(char[] buf);

//legge i dati in una porzione del buf
int read(char[] cbuf, int off, int len);

//dice se lo stream è pronto ad essere letto
boolean ready();

//resetta lo stream
void reset();

//skippa n caratteri
long skip(long n);
```

La `read` ritorna un intero che indica in unicode il carattere. E' bene perciò andare a convertire il numero in un carattere.

```java
int ch = r.read();
char unicode = (char) ch;
System.out.print(unicode);
r.close();
```

Per leggere invece una riga mi avvalgo della classe StringBuffer:

![[Pasted image 20230507104951.png]]

Per leggere da un file possiamo usare la classe `FileReader`:

```java
Reader r = new FileReader("file.txt");
int ch = r.read();
System.out.println( (char) ch);
r.close();
```

Il `path` del file parte alla ricerca dalla **current Working Directory**, che nel caso di un progetto eclipse è appunto la cartella stessa del progetto.

Per gestire `IOException` possiamo:
1. propagarla al chiamante
2. usare un blocco try catch finally
3. usare un blocco try-with-resource

```java
try(Reader r = new FileReader("file.txt")){
	//do stuff
	//automatically close the reader
}
```

Per leggere un Buffer possiamo usare la classe `BufferReader`:

```java
try(BufferReader rd = new BufferReader( new FileReader("file.txt"))){
	String linea;
	while (linea = rd.readLine() != null){
		System.out.println(linea);
	}
}
```

# Scrittura di caratteri
---
![[Pasted image 20230507105019.png]]

I principali metodi sono:

```java
void write(int c);

void write(char[] buf);

void write(char[] buf, int off, int len);

void write(String str);

close();

abstract void flush();
```

Per scrivere un file c'è la classe duale `FileWriter` e useremo una struttura del tipo:

```java
try(Writer w = new FileWriter("newfile.txt")){
	//stuff
	w.write();
	w.flush();
}
```

Occorre usare il metodo flush per non riempire il buffer di IO, altrimenti non si stampa nulla.


# I/O orientato ai byte
---
Posso usare due interfacce `InputStream` e `OutputStream` per manipolare esattamente allo stesso modo i byte, come facevo per i caratteri.

Usando l'interfaccia InputStream posso usare la classe `System.in` per leggere da standard input da tastiera:

```java
//1.Leggo i bute e li convergo in caratteri
Reader in = new InputStreamReader(System.in);

//2.Costruisco un buffer reader
BufferedReader br = new BufferedReader(in);

//3.Uso il metodo readline
String s = br.readLine(); 
```

Ora immaginiamo di voler leggere un file come un `.csv`: per fare:

```java
//1. Capire il contenuto di file, capire se usare un byte oriented o char oriented, poi procedo con buffered reader
try{Reader r = new FileReader("file.csv")
	BufferedReader br = new BufferedReader(r);
	String header = br.readLine();
	
	//2a. usare il metodo split
	String[] intestazioni = header.split(",");
	//2b. usare StringTokenizer
	StringTokenizer st = new StringTokenizer(header, ",");
	while (st.hasMoreTokens()){
		System.out.println(st.nextToken());
	}

	
}
```

```ad-note
Nei file `.csv` per preservare le stringhe così come sono le metto fra doppi apici, mentre per includere dei doppi apici in un campo stesso le raddoppio.
```