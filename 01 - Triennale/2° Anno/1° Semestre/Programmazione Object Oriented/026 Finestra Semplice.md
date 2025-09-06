---
aliases:
  - Frame
tags:
  - corsi/informatica/programmazione_object_oriented
paragrafo: Interfacce Grafiche in Java
cssclasses:
  - 
---
Un programma che utilizza le varie librerie di **grafica** in Java visualizza informazioni all'interno di una finestra dotata di barra di titolo e cornice (*frame*).

La JMV esegue ogni frame su un [[023 Thread|Thread]] separato.

### SEMPLICE FINESTRA VUOTA
Questo è il codice utilizzato ogni volta per creare una finestra qualsiasi, in questo caso vuota, con un titolo ed un pulsante per terminare il programma.

```Java
import javax.swing.*;
//...
JFrame frame = new JFrame();
frame.setSize(300, 400);
frame.setTitle("An Empty Frame");
frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
frame.setVisible(true); // Da mettere alla fine
```

1. Dichiarazione e costruzione del frame;
2. Impostazione della grandezza del frame misurata in pixels (`int`: `larghezza`, `altezza`);
3. Impostazione del titolo della barra del frame;
	- Lo si può fare anche dando al [[Costruttore di Oggetti|Costruttore]] la stringa del nome.
4. Impostazione dell'operazione di chiusura del programma al click sulla X; 
	- Se non è impostata, la finestra non si chiuderà.
5. Rende visibile l'intera finestra. 
	- **È importante mettere questa opzione alla fine**, perché quando si aggiungono componenti dopo questo metodo, questi non vengono visualizzati.

![[Pasted image 20221124170852.png|300]]


> [!note]+ Altri Metodi
>```Java
>frame.setResizable(false/true); // Non Permette / Permette il ridimensionamento della finestra
>frame.setLocationRelativeTo(null); // Mette la finestra al centro dello schermo
>frame.dispose(); // Chiude la finestra (utilie per i pulsanti personalizzati di chiusura)
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
- https://www.youtube.com/watch?v=KcEvHq8Pqs0