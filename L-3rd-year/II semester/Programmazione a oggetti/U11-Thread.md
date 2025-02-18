# Thread in Java
---

### Creazione dei thread in Java

Quando lanciamo un programma in Java, la JVM istanzia un thread: il thread "main" che viene istanziato di default.
Per **istanziare manualmente un thread** dobbiamo usare la classe `Thread` dando al costruttore un oggetto che implementa `Runner`, che in sostanza significa dare al thread il codice da eseguire.

```java
//Ciò che fa la JVM all'avvio è istanziare un thread nel seguente modo
Thread t = new Thread(
	()->Threads.main(null);
)
```

Notiamo che il costruttore del thread non lo fa partire: per startarlo è necessario (guardando l'esempio di prima) usare il metodo `start()`:

```java
Thread t = new Thread(
	()->{
		for (int i = 0; i < 10; i++){
			Thread.sleep(500);
			System.out.println(i);
		}
	}
)
t.start();
```

Con il metodo `join()` possiamo **attendere che un thread finisca di essere eseguito**.
Con il metodo `interrupt()` si può interrompere un thread in esecuzione, e lo fa tramite un eccezione:

```java
Thread t = new Thread(
	()->{
		for (int i = 0; i < 10; i++){
			
			try{
				Thread.sleep(500);
			}
			catch (InterruptException e){
				System.out.println("Thread interrotto...");
			}
			if (Thread.interrupted()){}
			System.out.println(i);
		}
	}
)
t.start();
Thread.sleep(1000);
t.interrupt();
```

Per dichiarare un oggetto `Runnable` che possiamo passare al thread possiamo usare il seguente formato:

```java
import java.util.Runnable
static Runnable contatore (int start, int end){
	return ()-> {
		for (int i = start; i< end;i++){
			//do stuff
		}
	}
}
```

Ovviamente l'ordinamento non è mai garantito implicitamente quando lanciamo più thread.
Per essere più comodi nel lanciare thread usiamo degli **Executor service**.

### ExecutorService

E' una classe di java che mette a disposizione dei metodi per terminare, rilasciare e sincronizzare i thread.

![[Pasted image 20230519204025.png]]

I tasks che possono essere eseguiti dagli executor service sono sia di tipo `Runnable` ma anche di tipo `Callable`:
Runnable ha solo il metodo : `void run()`
Callable invece ritorna un futuro oggetto di tipo `Future<T>`: con il metodo `isDone()` verifico se il Callable è terminato e `get()` mi restituisce il risultato entro un certo timeout.

```java
ExecutorService e = new Executors.newCachedThreadPool();
for (int i =0; i< 5 ;i++){
	e.submit(
		()-> {
		for (int i = start; i< end;i++){
			//do stuff
		}
	)
}

```

### Race conditions

Se abbiamo due thread che possono accedere contemporaneamente a due thread posso usare la parola chiave `synchronized` per permettere ai thread di accedere uno alla volta.

