---
aliases: 
tags:
  - corsi/informatica/programmazione_object_oriented
paragrafo: Interfacce Grafiche in Java
cssclasses:
  - 
---
Per disegnare forme geometriche semplici dobbiamo:
1. Creare una nuova classe, facendola [[015 Ereditarietà|Estendere]] a `JFrame`;
2. Impostare nel [[Costruttore di Oggetti|Costruttore]] il [[026 Finestra Semplice|Frame]];
3. Utilizzare il [[Metodi|Metodo]] `paint`.

```Java
import java.awt.*;
import javax.swing.*;

public class MyFrame extends JFrame {
	private final int larghezza = 500;
	private final int altezza = 500;
	
	/**
	 * Costruttore che costruisce un frame.
	 */
	MyFrame(){
		this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		this.setSize(larghezza, altezza);
		this.setLocationRelativeTo(null); // Mette la finestra al centro dello schermo
		this.setVisible(true);
	}
	
	/**
	 * Utilizziamo il seguente metodo per disegnare oggetti in una finestra.
	 */
	public void paint(Graphics g) {
		Graphics2D g2D = (Graphics2D) g;
		
		g2D./* INSERIRE QUI I METODI DELLE FORME DA DISEGNARE */;
	}
}
```

---
### COLORE E STROKE
Impostiamo il *colore* dei disegni in questo modo: **`g2D.setPaint(Color.blue)`** (si può scegliere un qualsiasi colore).
Impostiamo la *dimensione (stroke)* dei disegni in questo modo: **`g2D.setStroke(new BasicStroke(x))`**.

---
### LINEA
Disegna una linea, usando il colore corrente, tra i punti `(x1, y1)` e `(x2, y2)` nel sistema di coordinate del frame.

```Java
public void paint(Graphics g) {
		Graphics2D g2D = (Graphics2D) g;
		
		g2D.drawLine(0, 0, 500, 500); 
		// Disegna una linea che va dal punto più in alto a sinistra a quello più in basso a destra
}
```

> [!warning]- ATTENZIONE ALLA BARRA SUPERIORE
> La dimensione del frame include anche la parte superiore (che mostra il titolo ed i pulsanti della finestra), quindi se si esegue il codice qui sopra si avrà il seguente:
> ![[Pasted image 20221208171504.png|350]]
> Questo vale per tutti i metodi di disegno.
> La soluzione è quella di usare *JPanel*.
> 
> **File MyFrame.java**
>```Java
>import javax.swing.*;
>
>public class MyFrame extends JFrame {
>	MyPanel panel;
>	
>	/**
>	 * Costruttore che costruisce un frame.
>	 */
>	MyFrame(){
>		panel = new MyPanel();
>		
>		this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
>
>		this.add(panel); // Aggiunge il pannello alla finestra
>		this.pack(); // Imposta automaticamente la dimensione della finestra in base ai suoi componenti
>		
>		this.setLocationRelativeTo(null); // Apre la finestra al centro dello schermo
>		this.setVisible(true);
>	}	
>}
>```
> **File MyPanel.java**
>```Java
>import java.awt.*;
>import javax.swing.JPanel;
>
>public class MyPanel extends JPanel {
>	private final int larghezza = 500;
>	private final int altezza = 500;
>	
>	MyPanel() {
>		this.setPreferredSize(new Dimension(larghezza, altezza));
>	}
>	
>	/**
>	 * Utilizziamo il seguente metodo per disegnare oggetti in un pannello.
>	 */
>	public void paint(Graphics g) {
>		Graphics2D g2D = (Graphics2D) g;
>		
>		g2D.drawLine(0, 0, 500, 500);
>	}
>}
>
>```

---
### MESSAGGIO (STRINGA)
Per scrivere una stringa in una finestra si usa `.drawString(str, x, y)`. 
Gli `x` ed `y` sono le coordinate del punto base.

![[Pasted image 20221208175003.png|600]]

```Java
public void paint(Graphics g) {
	Graphics2D g2D = (Graphics2D) g;
	
	g2D.setColor(Color.blue);
	// Imposta il colore
	
	g2D.setFont(new Font("Comic Sans MS", Font.PLAIN, 50));
	// Imposta la famiglia del font, il tipo di font (BOLD, ITALIC, PLAIN), la dimensione del font
	
	g2D.drawString("Message", 50, 100); 
}
```

> [!warning]- Il messaggio può essere di una sola riga
> Per avere più righe bisogna usare più volte il metodo.
>
>```Java
>public void paint(Graphics g) {
>	Graphics2D g2D = (Graphics2D) g;
>	
>	g2D.setColor(Color.black);
>	g2D.setFont(new Font("Comic Sans MS", Font.ITALIC, 50));
>	
>	g2D.drawString("graphic design is", 50, 100);
>	g2D.drawString("my passion.", 50, 160);
>}
>```
>![[Pasted image 20221208180210.png|350]]

---
### ALTRE FORME
- **Rettangoli**;
- **Ellipse (Cerchio)**;
- ecc.

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