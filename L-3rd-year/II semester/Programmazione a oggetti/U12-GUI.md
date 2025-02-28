# Swing
---
E' un toolkit usato per scrivere interfacce grafiche.
Si basa sul pattern **MVC Model View Controller**, ovvero suddividiamo l'app in 3 parti:

![[Pasted image 20230531161958.png]]

Il **Model** è la componente che ha il compito di mantenere i dati. Esso comprende i dati stessi e le operazioni che posso fare su di essi (volendo semplificare un model è un database).

La **View** è la componente che permette la visualizzazione dei dati contenuti nel modello.

Il **Controller** è al componente che gestisce qualunque interazione con l'utente. Questa componente ha il compito di captare le interazioni e modificare opportunamente dati e vista se necessario.

La principale classe per gestire le GUI è **Jframe**, che rappresenta la finestra.

```java
import javax.swing.JFrame

JFrame finestra = new JFrame();
finestra.setDefaultCloseOperation(JFrame.DISPOSE_ON_CLOSE);
finestra.setVisibile(true);
```

Il codice qui sopra mostra una finestra vuota, con lo stile del sistema operativo. Possiamo personalizzare la finestra con i vari metodi predefiniti.

```java
finestra.setTitle("Prova del titolo!");
finestra.setSize(400,500);
```

### Traduttore

La classe model conterrà i dati su cui lavoriamo, mentre la view:

```java
import javax.JFrame.*;

public class View extends JFrame{
	private Model modello;
	JLabel l1;
	JLabel l2;
	JButton pulsante;
	JTextField parola;
	
	public View(Modello m){
		modello =m;
	//creo la label con un testo predefinito
		l1 = new JLabel("Parola da tradurre:");
	//creo la label che conterrà la parola tradotta
		l2 = new JLabel("-");
	//Creo la text area con al massimo 15 caratteri inseribili
		parola = new JTextField(15);
	//pulsante con scritta interna
		pulsante = new JButton("Traduce!");
	//Stile e comportamento
		setTitle("Dizionario");
		setSize(500,400);
		setDefaultCloseOperation(DISPOSE_ON_CLOSE);
		setVisible(true);
	}
}
```

Avviando il programma così com'è non si visualizza nulla, serve perciò un **layout manager** per sapere come disporre gli elementi all'interno della finestra.

```java
import java.awt;
public class View{
	...
	public View(Modello m){
		...
		JPanel p = new JPanel();
		p.setLayout(new FlowLayout());
	//Aggiungiamo p alla finestra
		add(p);
	//Poi aggiungiamo al pannello gli elementi
		p.add(l1);
		p.add(pulsante);
		p.add(parola);
		p.add(l2);
		...
	}
}
```

Per gestire gli eventi sul pulsante dobbiamo creare la classe del controllore:

```java
public class Controller{

//Il costruttore ha come argomenti passati la vista e il modello dei dati perchè deve modificare entrambi
	public Controller(Model m, View v){
		v.pulsante.addActionListener(
		evt -> {
			String parola = v.parola.getText();
			m.setParola(parola);
			//L'update della view è lasciato a un metodo update alla view stessa
			v.update();	
		})
	}

}
```

Inseriamo il metodo aggiorna all'interno della view:

```java
 public update(){
	 l2.setText(modello.getTraduzione());
 }
```


