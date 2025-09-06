---
aliases: 
tags:
  - corsi/informatica/programmazione_object_oriented
paragrafo: Interfacce Grafiche in Java
cssclasses:
  - 
---
## NUOVE FINESTRE
Per creare una nuova [[026 Finestra Semplice|finestra]] da un'altra, bisogna istanziare una nuova classe contenente un'altra finestra.

![[Pasted image 20221209170817.png]]

Per esempio:
- *File `LaunchPage.java`*
```Java
import javax.swing.JButton;
import javax.swing.JFrame;

public class LaunchPage {
	JFrame frame = new JFrame();
	JButton button = new JButton("New Window");
	
	public LaunchPage() {
		button.setBounds(100, 160, 200, 40);
		button.setFocusable(false);
		
		button.addActionListener(e -> {
			//frame.dispose(); // Usare questo per eliminare la finestra corrente
			NewWindow n = new NewWindow();
		});
		
		frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		frame.setSize(400, 400);
		frame.setLayout(null);
		frame.add(button);
		
		frame.setVisible(true);
	}
	
	public static void main(String[] args) {
		LaunchPage lp = new LaunchPage();
		
	}
}
```

- *File `NewWindow.java`*
```Java
import java.awt.Font;

import javax.swing.JFrame;
import javax.swing.JLabel;

public class NewWindow {
	JFrame frame = new JFrame();
	JLabel label = new JLabel("Hello");
	
	NewWindow(){
		frame.setSize(200, 100);
		frame.setLayout(null);
		
		label.setBounds(0, 0, 100, 50);
		label.setHorizontalAlignment(JLabel.CENTER);
		label.setFont(new Font("Comic Sans MS", Font.PLAIN, 25));
		
		frame.add(label);
		frame.setVisible(true);
	}
}
```

## FINESTRE POPUP (JOptionPane)
Le finestre **Popup** servono per comunicare qualcosa in un'altra finestra velocemente.

```Java
JOptionPane.showMessageDialog(null, "Messaggio", "Titolo", JOptionPane.PLAIN_MESSAGE);
```

![[Pasted image 20221209173759.png]]
<center>PLAIN_MESSAGE</center>

Ci sono diverse opzioni per l'ultimo parametro (cambia solo l'icona):
- `PLAIN_MESSAGE`;
- `INFORMATION_MESSAGE`;
- `QUESTION_MESSAGE`;
- `WARNING_MESSAGE`;
- `ERROR_MESSAGE`.

![[Pasted image 20221209173825.png]]
<center>INFORMATION_MESSAGE</center>

Il primo parametro riguarda il *Componente Genitore*.

## FINESTRE DI DIALOGO (JDialog)
Le finestre di **Dialogo** servono per creare nuove finestre che possono mandare informazioni alla finestra principale. Un esempio sarebbe quello di prendere le informazioni in input dall'utente.

```Java
JDialog dialog = new JDialog(frame, "Dialog Example", true);
dialog.setLayout(new FlowLayout());
JButton button = new JButton("OK");

// Se si preme il pulsante OK, l'intera finestra di dialogo scompare
dialog.addActionListener(ev -> {
	dialog.setVisible(false);
});

dialog.add(new JLabel("Click button to continue"));
dialog.add(b);
dialog.setSize(300, 300);
dialog.setLocationRelativeTo(null);
dialog.setVisible(true);
```

![[Pasted image 20230123140513.png]]

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
- [Finestra Nuova - Bro Code (Video)](https://www.youtube.com/watch?v=HgkBvwgciB4)
- [JOptionPane - Bro Code (Video)](https://www.youtube.com/watch?v=BuW7y21FcYI&t=307s)