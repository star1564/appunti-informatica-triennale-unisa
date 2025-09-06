---
aliases: 
tags:
  - corsi/informatica/programmazione_object_oriented
paragrafo: Interfacce Grafiche in Java
cssclasses:
  - 
---
I **Layout Managers** sono usati per posizionare (e ridimensionare) [[029 Componenti di GUI Java|Componenti di GUI]] in un modo particolare. Esistono diversi layout managers, tra cui **`BorderLayout`**, **`FlowLayout`** e **`GridLayout`**.

Come già detto in precedenza, i [[029 Componenti di GUI Java#PANNELLO (JPanel)|Pannelli]] possono usare i loro layout manager.

---
### BORDER LAYOUT
**`BorderLayout`** è il layout manager **di default** quando si crea un [[026 Finestra Semplice|Frame]], è usato per posizionare componenti in *cinque regioni*: 
- NORTH; 
- SOUTH; 
- EAST; 
- WEST; 
- CENTER.

![[Pasted image 20221209150544.png]]

Ogni regione può contenere **solo un componente**. 
Ci sono due [[Metodi]]: 
- *`.setLayout(new BorderLayout())`*: crea un layout senza spazi tra di loro;
- *`.setLayout(new BorderLayout(int hgap, int vgap))`*: crea un layout con gli spazi assegnati come parametri.

Si sceglie la regione attraverso il metodo `add`, passandogli un componente e la regione.

> [!example]- <font color="orange">Esempio</font>
>```Java
>import java.awt.BorderLayout;
>import javax.swing.JButton;
>import javax.swing.JFrame;
>
>public class Main {
>
>	public static void main(String[] args) {
>		JFrame frame = new JFrame();
>		frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
>		frame.setSize(500, 500);
>		frame.setLayout(new BorderLayout());  // BorderLayout è quello di default per i frame
>		// Aggiungere (new BorderLayout(10, 10)) per avere lo spazio tra componenti
>		
>		frame.add(new JButton("Nord"), BorderLayout.NORTH);
>		frame.add(new JButton("Sud"), BorderLayout.SOUTH);
>		frame.add(new JButton("Est"), BorderLayout.EAST);
>		frame.add(new JButton("Ovest"), BorderLayout.WEST);
>		frame.add(new JButton("Centro"), BorderLayout.CENTER);
>		
>		frame.setVisible(true);
>	}
>}
>```
>![[Immagine 2022-12-09 091234 1.png|Con spazi (sinistra); Senza spazi (destra)|500]]

> [!example]- <font color="orange">Esempio con JPanel</font>
>```Java
>import java.awt.BorderLayout;
>import java.awt.Color;
>import java.awt.Dimension;
>
>import javax.swing.JFrame;
>import javax.swing.JPanel;
>
>public class Main {
>
>	public static void main(String[] args) {
>		JPanel panel1 = new JPanel();
>		JPanel panel2 = new JPanel();
>		JPanel panel3 = new JPanel();
>		JPanel panel4 = new JPanel();
>		JPanel panel5 = new JPanel();
>		
>		panel1.setBackground(Color.red);
>		panel2.setBackground(Color.green);
>		panel3.setBackground(Color.yellow);
>		panel4.setBackground(Color.magenta);
>		panel5.setBackground(Color.blue);
>		
>		JFrame frame = new JFrame();
>		frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
>		frame.setSize(750, 750);
>		frame.setLayout(new BorderLayout(10, 10));
>		frame.setVisible(true);
>		
>		panel1.setPreferredSize(new Dimension(100, 100));
>		panel2.setPreferredSize(new Dimension(100, 100));
>		panel3.setPreferredSize(new Dimension(100, 100));
>		panel4.setPreferredSize(new Dimension(100, 100));
>		panel5.setPreferredSize(new Dimension(100, 100));
>		
>		frame.add(panel1, BorderLayout.NORTH);
>		frame.add(panel2, BorderLayout.WEST);
>		frame.add(panel3, BorderLayout.EAST);
>		frame.add(panel4, BorderLayout.SOUTH);
>		frame.add(panel5, BorderLayout.CENTER);
>		
>		
>		// -------------- sub panels -----------------
>		
>		JPanel panel6 = new JPanel();
>		JPanel panel7 = new JPanel();
>		JPanel panel8 = new JPanel();
>		JPanel panel9 = new JPanel();
>		JPanel panel10 = new JPanel();
>		
>		panel6.setBackground(Color.black);
>		panel7.setBackground(Color.darkGray);
>		panel8.setBackground(Color.gray);
>		panel9.setBackground(Color.lightGray);
>		panel10.setBackground(Color.white);
>		
>		panel5.setLayout(new BorderLayout());
>		
>		panel6.setPreferredSize(new Dimension(100, 100));
>		panel7.setPreferredSize(new Dimension(100, 100));
>		panel8.setPreferredSize(new Dimension(100, 100));
>		panel9.setPreferredSize(new Dimension(100, 100));
>		panel10.setPreferredSize(new Dimension(100, 100));
>		
>		
>		// Il pannello 5 è stato completamente usato
>		panel5.add(panel6, BorderLayout.NORTH);
>		panel5.add(panel7, BorderLayout.WEST);
>		panel5.add(panel8, BorderLayout.EAST);
>		panel5.add(panel9, BorderLayout.SOUTH);
>		panel5.add(panel10, BorderLayout.CENTER);
>	}
>}
>```

---
### FLOW LAYOUT
Attraverso il **`FlowLayout`** è possibile posizionare componenti *in una sola riga*, alla loro dimensione preferita. Se la finestra è troppo piccola per far vedere tutti i componenti su una sola riga, il layout manager utilizzerà le prossime righe necessarie.

![[Pasted image 20221209160346.png]]

Ci sono tre Metodi: 
- *`.setLayout(new FlowLayout())`*: crea un flow layout normale;
- *`.setLayout(new FlowLayout(FlowLayout.CENTER))`*: crea un flow layout centrato (`LEADING` lo organizza a sinistra, `TRAILING` verso destra);
- *`.setLayout(new FlowLayout(FlowLayout.CENTER, spzVer, spzHor))`*: crea un flow layout con spaziatura tra i componenti specificati.

> [!example]- <font color="orange">Esempio</font>
>```Java
>import java.awt.FlowLayout;
>import javax.swing.JButton;
>import javax.swing.JFrame;
>
>public class Main {
>
>	public static void main(String[] args) {
>		JFrame frame = new JFrame();
>		frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
>		frame.setSize(500, 500);
>		frame.setLayout(new FlowLayout(FlowLayout.LEADING, 10, 10));
>		
>		frame.add(new JButton("1"));
>		frame.add(new JButton("2"));
>		frame.add(new JButton("3"));
>		frame.add(new JButton("4"));
>		frame.add(new JButton("5"));
>		frame.add(new JButton("6"));
>		frame.add(new JButton("7"));
>		frame.add(new JButton("8"));
>		frame.add(new JButton("9"));
>		
>		frame.setVisible(true);
>	}
>}
>```

---
### GRID LAYOUT
Attraverso il **`GridLayout`** è possibile posizionare componenti *in una griglia*. Tutti i componenti prendono tutto lo spazio possibile all'interno di una cella, mentre tutte le celle hanno dimensioni uguali.

![[Pasted image 20221209162521.png]]



Ci sono tre Metodi:
- *`.setLayout(new GridLayout())`*: crea un grid layout normale, con celle dalle dimensioni automatiche;
- *`.setLayout(new GridLayout(righe, colonne))`*: crea un grid layout con un numero specificato di righe e colonne;
- _`.setLayout(new GridLayout(righe, colonne, spzHor, spzVer))`_: crea un grid layout con spaziatura tra i componenti specificati.

> [!warning]- Bilanciamento righe e colonne
> Il grid layout manager cercherà di bilanciare il contenuto della finestra (o del pannello) se il loro numero non è "giusto".
> Qui si vede il bilanciamento in un grid 3x3 nel quale ci sono 10 elementi.
> ![[Pasted image 20221209162448.png|400]]

> [!example]- <font color="orange">Esempio</font>
>```Java
>import java.awt.GridLayout;
>import javax.swing.JButton;
>import javax.swing.JFrame;
>
>public class Main {
>
>	public static void main(String[] args) {
>		JFrame frame = new JFrame();
>		frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
>		frame.setSize(500, 500);
>		frame.setLayout(new GridLayout(3,3, 10,10));
>		
>		frame.add(new JButton("1"));
>		frame.add(new JButton("2"));
>		frame.add(new JButton("3"));
>		frame.add(new JButton("4"));
>		frame.add(new JButton("5"));
>		frame.add(new JButton("6"));
>		frame.add(new JButton("7"));
>		frame.add(new JButton("8"));
>		frame.add(new JButton("9"));
>		//frame.add(new JButton("10"));  // Elemento sbilanciato
>		
>		frame.setVisible(true);
>	}
>}
>```

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
- [Tutti i Layout Managers](https://docs.oracle.com/javase/tutorial/uiswing/layout/visual.html)
- [BorderLayout - Bro Code (Video)](https://www.youtube.com/watch?v=PD6pd6AMoOI)
- [FlowLayout - Bro Code (Video)](https://www.youtube.com/watch?v=pDqjHozkMBs)
- [GridLayout - Bro Code (Video)](https://www.youtube.com/watch?v=ohNqQagkDDY)
