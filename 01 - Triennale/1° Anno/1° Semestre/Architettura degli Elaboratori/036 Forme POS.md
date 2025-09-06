---
aliases: 
tags:
  - corsi/informatica/architettura_degli_elaboratori
paragrafo: Componenti Logici
cssclasses:
  - 
---
>Per **POS** (Product Of Sums) si intendono i prodotti delle somme.

Per individuare le forme canoniche POS, si possono eseguire due metodi:
- Con la tavola di verità;
- Negando la funzione ed applicare [[025 Funzioni Booleane (AdE)#Leggi di De Morgan per l'Algebra Booleana|Leggi di De Morgan (AdE)|De Morgan]].

Una formula booleana è in forma canonica POS se è un AND di [[025 Funzioni Booleane (AdE)#Clausola|Clausole]], cioè, un AND di OR di [[025 Funzioni Booleane (AdE)#Letterale|letterali]].

> [!example]- <font color="orange">Esempio</font>
>$$(\overline{x_1}\lor x_2\lor x_3\lor x_4)\land(x_3\lor \overline{x_6})\land (x_3\lor \overline{x_5}\lor x_5)$$

### TAVOLA DI VERITÀ → FORMA CANONICA POS
Se la funzione logica è un [[Maxtermine|maxtermine]], la sua tavola di verità *ha un solo 0* e viceversa.

Se invece la tavola di verità ha più *corrispondenze di 0*, troviamo i maxtermini corrispondenti e li moltiplichiamo.

Gli 0 vanno presi normalmente, gli 1 devono essere negati.

> [!example]- <font color="orange">Esempio 1</font>
>Dalla seguente tabella di verità ricavare la funzione in forma POS.
>
| $x_2$ | $x_1$ | **$f$** |
| ----- | ----- | ------- |
| *0*   | *0*   | *0*     |
| 0     | 1     | 1       |
| *1*   | *0*   | *0*     |
| 1     | 1     | 1       |
Allora la forma canonica POS è:
$f_{POS}=(x_2+x_1)\cdot (\overline{x_2}+x_1)$

> [!example]- <font color="orange">Esempio 2</font>
Dalla seguente tabella di verità ricavare la funzione negata, trovare la sua forma SOP e convertirla in forma POS.
>
| $x_2$ | $x_1$ | $f$ | **$\overline{f}$** |
| ----- | ----- | --- | ------------------ |
| *0*   | *0*   | *0* | *1*                  |
| 0     | 1     | 1   | 0                  |
| *1*   | *0*   | *0* | *1*                  |
| 1     | 1     | 1   | 0                   |
$\overline{f_{SOP}}=(\overline{x_2}\cdot \overline{x_1})+(x_2\cdot \overline{x_1}) =$
$=\overline{(\overline{x_2}\cdot \overline{x_1})+(x_2\cdot \overline{x_1})} = \overline{(\overline{x_2}\cdot \overline{x_1})}\cdot\overline{(x_2\cdot \overline{x_1})}$
$= (x_2+x_1)\cdot (\overline{x_2}+x_1) = f_{POS}$
Circuito OR-to-AND
![[Pasted image 20211029111830.png|500]]

___
[[000 Indice AdE|↖ Ritorna all'indice ↖]]

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