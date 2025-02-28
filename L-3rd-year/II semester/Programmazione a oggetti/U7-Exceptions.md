# Tecniche di gestione di errore
---
Tecniche per gestire errori nel codice sono:
- **Program abort** (solo per errori fatali gravi)
- **Usare valori speciali** (non sempre utilizzabili)
- **Usare una variabile globale di stato** (come in C)
- **Usare Eccezioni**

# Exceptions
---
### Dichiarazione di eccezioni

In java `Exception` è una classe e se vogliamo dichiarare eccezioni personalizzate proseguo nel seguente modo:

```java
public class HydraulicExceptionNoSource extends Exception {

	private static final long serialVersionUID = 1L;

	public HydraulicsExceptionNoSource(String msg){
		super(msg);
	}
}
```

Le nostre eccezioni **devono estendere la classe Exception**, inoltre dato che queste devono essere scritte su disco occorre che abbiamo un **identificatore seriale**.
Il SerialNumber non è obbligatorio ma elimina un warning dal compilatore.

A questo punto per lanciare l'eccezione possiamo usare la keyword `throw`:

```java
if (!sourceFound){
	throw new HydraulicExceptionNoSource("Manca il source");
}
```

Ogni volta che un metodo **può generare un eccezione** allora occorre segnalarlo nell'intestazione tramite la keyword `throws`:

```java
public String aMethod() throws HydraulicsExceptionNoSource{
	//do stuff
	if (!sourceFound){
		throw new HydraulicExceptionNoSource("Manca il source");
	}
	//do stuff
}
```

In questo modo però l'eccezione rimane **unhandled** cioè non è gestita e il compilatore da un errore quando si prova a usare il metodo.

### Gestione delle eccezioni

Un primo modo fa uso del costrutto `try, catch` che permette di catturare e gestire l'eccezione:

```java
try{
	String a = aMethod();
}
catch ( HydraulicsExceptionNoSource e ){
	e.printStackTrace();
}
catch ( AnotherException e){
	e.printStackTrace();
}
```

I blocchi catch sono eseguiti in sequenza, e ciò significa che se metto prima un eccezione più generica e poi una più specifica non serve a nulla, perchè se il primo blocco catch cattura l'eccezione, il secondo non verrà mai eseguito.
E' buona norma usare quest'ordine delle eccezioni:

![[Pasted image 20230420140200.png]]


Un secondo modo consiste nel **non gestire direttamente** l'eccezione nel blocco try,catch, ma bensì posso mettere "in cascata" un `throws Exception` così delego sempre il chiamante di gestire l'eccezione.
Prima o poi ci troveremo in un blocco try catch e il programma potrà gestire l'eccezione.


### Tipi di eccezioni

Se proviamo ad eseguire questo codice:

```java
String s= null;
s.charAt(0);
```

il compilatore non ci obbliga a gestire l'eccezione NullPointerException, e questo perchè esistono diverse famiglie di eccezioni:

![[Pasted image 20230420133837.png]]

Se proviamo a lanciare una unchecked exception il compilatore non ci dirà che dobbiamo gestirla, mentre nel caso di una checked (in cui rientrano tutte le eccezioni che dichiaramo a mano e che estendono Excpetion) il compilatore ci obbligherà a gestirla.

```ad-info
Il framework JUnit usa proprio eccezioni unchecked per verificare gli errori che si sono verificati, e inoltre fornisce sempre lo StackTrace.
```

### Keyword Finally

E' possibile inserire un ulteriore blocco nel costrutto try catch che **viene eseguito indipendentemente dal fatto che si abbia avuto un'eccezione**:

```java
try{
	//do stuff
}
catch (IOexception e){
	//do other suff
}
finally{
	f.close();
}
```

Questo metodo può essere usato per assicurarsi di aver chiuso un file, o fare una pulizia delle risorse usate.

