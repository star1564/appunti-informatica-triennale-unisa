---
aliases: 
tags:
  - corsi/informatica/programmazione_object_oriented
paragrafo: Interfacce Grafiche in Java
cssclasses:
  - 
---
### EVENTI
Ogni volta che un utente esegue un'*azione* su degli elementi dell'[[029 Componenti di GUI Java|Interfaccia Grafica]] viene generato un **Evento**.

Ci sono molti tipi di eventi:
- il click del mouse; 
- la pressione di un tasto della tastiera; 
- l'interazione dell'utente con un elemento dell'interfaccia grafica (come un pulsante o un menu);
- ecc...

Un evento proviene da una *Sorgente (source)*, ovvero una componente di un'interfaccia grafica che può generare gli eventi.

Gli eventi generati vengono elaborati dai *Ricevitori (listener)*, che reagiscono nel modo descritto dal programmatore.

---
### ACTION LISTENERS
Un **Action Listener** è un ricevitore di eventi ed esiste una interfaccia `Listener` per ciascun tipo di evento.

Questi definiscono i [[Metodi]] che devono essere implementati da un ricevitore di eventi.

Un esempio è l'interfaccia **`ActionListener`**:
```Java
public interface ActionListener{
	void actionPerformed(ActionEvent event);
}
```

Per collegare una sorgente di eventi ad un ricevitore, occorre aggiungere il ricevitore alla sorgente di eventi.

Però è molto più semplice implementare un action listener con una [[021 Espressioni Lambda|espressione lambda]]:
```Java
componente.addActionListener(e -> {
	//Codice da eseguire
});
```

> [!example]+ <font color="orange">Esempio</font>
>```Java
>import java.awt.event.ActionEvent;
>import java.awt.event.ActionListener;
>import javax.swing.*;
>
>/**
> * Un action listener che stampa un messaggio sul terminale
>*/
>class ClickListener implements ActionListener {
>	public void actionPerformed(ActionEvent event){
>		System.out.println("I was clicked.");
>	}
>}
>
>public class Main{
>	private static final int FRAME_WIDTH = 100;
>	private static final int FRAME_HEIGHT = 60;
>
>	public static void main(String[] argrs){
>		JFrame frame = new JFrame();
>
>		// Creazione ed aggiunta di un pulsante
>		JButton button = new JButton("Click me!");
>		frame.add(button);
>
>		// Creazione e collegamento del listener al bottone
>		ActionListener listener = new ClickListener();
>		button.addActionListener(listener);
>
>		// Settaggio del frame
>		frame.setSize(FRAME_WIDTH, FRAME_HEIGHT);
>		frame.setLayout(null);
>		frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
>		frame.setVisible(true);
>	}
>}
>```
> Usando le espressioni lambda è più facile.
>```Java
>import javax.swing.*;
>
>public class Main{
>	private static final int FRAME_WIDTH = 100;
>	private static final int FRAME_HEIGHT = 60;
>
>	public static void main(String[] argrs){
>		JFrame frame = new JFrame();
>
>		// Creazione ed aggiunta di un pulsante
>		JButton button = new JButton("Click me!");
>		frame.add(button);
>
>		// Creazione e collegamento del listener al bottone
>		button.addActionListener(e -> {
>			System.out.println("I was clicked.");
>		});
>
>		// Settaggio del frame
>		frame.setSize(FRAME_WIDTH, FRAME_HEIGHT);
>		frame.setLayout(null);
>		frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
>		frame.setVisible(true);
>	}
>}
>```

---
### MOUSE LISTENERS
Un **Mouse Listener** è un creatore di eventi che si basa sull'osservazione del mouse. Può creare un evento quando:
- Il mouse clicca un certo elemento;
- Il mouse tiene premuto o rilascia un certo elemento;
- Il mouse entra/esce nel/dal contenuto di un certo elemento.

Questo definisce i metodi che devono essere implementati:
```Java
public class MyFrame implements MouseListener{
	public MyFrame(){
		// Costruzione del frame
		// e dei componenti

		componente.addMouseListener(this); // Aggiunta di un mouse listener ad un componente
	}
	
	@Override
	public void mouseClicked(MouseEvent e) {
		// Codice
	}
	@Override
	public void mousePressed(MouseEvent e) {
		// Codice
	}
	@Override
	public void mouseReleased(MouseEvent e) {
		// Codice
	}
	@Override
	public void mouseEntered(MouseEvent e) {
		// Codice
	}
	@Override
	public void mouseExited(MouseEvent e) {
		// Codice
	}
}
```

> [!info] Purtroppo non ci sono espressioni lambda per questo tipo di listener. Inoltre bisogna creare una classe apposita per il frame.


Per collegare una sorgente di eventi ad un ricevitore, occorre aggiungere il ricevitore alla sorgente di eventi.


___
[[000 Indice POO|↖ Ritorna all'indice ↖]]

```dataviewjs
/*
function extractUpperCaseLetters(inputString) {
	// Use a regular expression to match uppercase letters (A-Z)
	const uppercaseLetters = inputString.match(/[A-Z]/g);
	
	// Check if uppercaseLetters is not null, and join the matched letters into a string
	if (uppercaseLetters !== null) {
		return uppercaseLetters.join('');
	} else {
	    // If no uppercase letters were found, return an empty string
	    return '';
	}
}
*/

function extractNumberFromString(inputString) {
	const match = inputString.match(/^\d{3}/);
	
	if (match) {
		return match[0];
	} else {
		return null; // Return null if no match is found
	}
}

function startsWithNumber(inputString, targetNumber) {
  // Use a regular expression to check if the string starts with the target number
  const pattern = new RegExp(`^${targetNumber}`);
  return pattern.test(inputString);
}

function sort2Array(array){
	let res = []
	
	if (array[0] == undefined)
		res.push(array[1], array[1])
	else if (array[1] == undefined)
		res.push(array[0], array[0])
	else if (parseInt(extractNumberFromString(array[0])) > parseInt(extractNumberFromString(array[1])))
		res.push(array[1], array[0])
	else
		res.push(array[0], array[1])
	
	return res
}

let toDisplay = []
function searchPrevAndNext(tag, currentNumber) {
	const prevNumber = (parseInt(currentNumber) - 1).toString().padStart(3, "0");
	const nextNumber = (parseInt(currentNumber) + 1).toString().padStart(3, "0");
	
	let prevAndNext = [(dv.pages(tag).where(p => startsWithNumber(p.file.name, prevNumber) || startsWithNumber(p.file.name, nextNumber)).file.name)]
	
	// Prev = [0]; Next = [1]
	let sortedPrevAndNext = sort2Array(prevAndNext[0])
	
	if (currentNumber == "001"){ 
		// Se è la prima pagina aggiungi solo il prossimo
		let nextLink = "[[" + sortedPrevAndNext[1] + "|" + "Nota Successiva →" + "]]"
		toDisplay.push(nextLink)
	} else if (prevAndNext[0][1] == undefined){
		// Se è l'ultima pagina aggiungi solo il precedente
		let prevLink = "[[" + sortedPrevAndNext[0] + "|" + "← Nota Precedente" + "]]"
		toDisplay.push(prevLink)
	} else {
		let nextLink = "[[" + sortedPrevAndNext[1] + "|" + "Nota Successiva →" + "]]"
		toDisplay.push(nextLink)
		
		let prevLink = "[[" + sortedPrevAndNext[0] + "|" + "← Nota Precedente" + "]]"
		toDisplay.push(prevLink)
	}
	
	
}

if (dv.current().tags[0] == null || dv.current().tags[0] == undefined){
	dv.header(1, "Errore - Inserire il tag nelle proprietà del file")
} else {
	let tag = "#" + dv.current().tags[0]

	// Purtroppo obsidian non riesce a leggere i link e a creare connessioni nel grafo,
	// quindi ho disattivato il link all'indice automatico e la funzione extractUpperCaseLetters
	//let indexName = "000 Indice " + extractUpperCaseLetters(tag)
	//let index = dv.page(indexName).file
	//let indexLink = "[[" + index.name + "|" + "↖ Ritorna all'indice ↖" + "]]"
	//toDisplay.push(indexLink)
	
	let currentPage = dv.current().file
	let currentPageNumber = extractNumberFromString(currentPage.name)
	
	searchPrevAndNext(tag, currentPageNumber)
	
	dv.list(toDisplay)
}
```

Altri collegamenti: 