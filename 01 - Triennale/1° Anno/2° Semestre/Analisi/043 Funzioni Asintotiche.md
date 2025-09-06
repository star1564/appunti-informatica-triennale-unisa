---
aliases: 
tags:
  - corsi/matematica/analisi
paragrafo: Limiti di Funzioni
cssclasses:
  - 
---
## Equivalenze Asintotiche
>L'**Equivalenza Asintotica**, solitamente indicato con $\sim$ oppure $\simeq$, permette di confrontare localmente due funzioni al tendere di $x$ ad un determinato valore o all'infinito.

Due [[011 Funzioni|funzioni]] $f(x)$ e $g(x)$ si dicono *Asintoticamente Equivalenti* per $x\mapsto x_0$ se 
$$\lim\limits_{x \to x_0} \frac{f(x)}{g(x)}=1$$
e si scrive $f(x)\sim g(x)$ per $x\mapsto x_0$.


> [!example]+ <font color="orange">Esempio</font>
>$\lim\limits_{x \to 0}\frac{sen(x)}{x}=1$ si può riassumere in "$sen(x)$ si comporta come $x$ quando $x$ tende a $0$", ovvero $sen(x)\sim x$.

### Proprietà Equivalenze Asintotiche
Valgono le seguenti proprietà per le equivalenze asintotiche:
1. Se $f_1(x)\sim g_1(x)$ e $f_2(x)\sim g_2(x)$ per $x\mapsto x_0$, allora $$\color{#61AFEF} f_1(x)\cdot f_2(x)\sim g_1(x)\cdot g_1(x),\ x\mapsto x_0$$
Ed in particolare, se i limiti per $x\mapsto x_0$ dei due prodotti esistono, sono uguali.

2. Se $f_1(x)\sim g_1(x)$ e $f_2(x)\sim g_2(x)$ per $x\mapsto x_0$, allora $$\color{#61AFEF} \frac{f_1(x)}{f_2(x)}\sim\frac{g_1(x)}{g_2(x)},\ x\mapsto x_0$$
Ed in particolare, se i limiti per $x\mapsto x_0$ dei due rapporti esistono, sono uguali. 

3. Se $f(x)\sim g(x)$ per $x\mapsto x_0$, allora $$\color{#61AFEF}[f(x)]^\alpha\sim[g(x)]^\alpha,\ x\to x_0$$

___
[[000 Indice Analisi|↖ Ritorna all'indice ↖]]

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
