---
aliases:
  - numero complesso
  - Unità Immaginaria
tags:
  - corsi/matematica/analisi
paragrafo: Numeri Complessi
cssclasses:
  - 
---
>I **Numeri Complessi** costituiscono un insieme che *estende* l'insieme dei [[003 Insieme dei numeri Reali|numeri reali]] ed in cui, a partire dalla definizione di *unità immaginaria*, è possibile: 
>- Estrarre le [[MB - 14 Radicali|radici]] ad indice pari di *numeri negativi* 
>- Risolvere le [[MB - 08 Equazioni di Secondo Grado|equazioni di secondo grado]] con *discriminante negativo*

## Campo dei Complessi
>Si definisce l'insieme dei numeri complessi $\color{#CC241D}\mathbb{C}$ come l'insieme ottenuto dal [[012 Prodotto Cartesiano tra Insiemi|prodotto cartesiano]] di $\mathbb{R}$ con se stesso. Ovvero:
>$$\mathbb{C}:=\mathbb{R}\times\mathbb{R}=\mathbb{R}^2=\{(a,b)\mid a,b\in\mathbb{R}\}$$
>
>Ne segue allora che *ogni numero complesso è una coppia ordinata di numeri reali*, ossia:
>$$z\in\mathbb{C}\iff z=(a,b),\quad \text{con}\ a,b\in\mathbb{R}$$

I numeri complessi della forma $(a,0)$ coincidono con i *numeri reali*, ovvero che $$(a,0)= a\in\mathbb{R}$$
Mentre i numeri del tipo $(0, b)$ sono gli *immaginari puri*. Inoltre possono [[024 Operazioni e Proprietà dei Numeri Complessi|godere di proprietà e si possono eseguire delle operazioni su di loro]].

### Unità Immaginaria
>L'**Unità Immaginaria $\color{#CC241D}i$** è il numero complesso *immaginario puro* che si identifica con la coppia ordinata $$\color{#CC241D}i:=(0, 1)$$

Dato che questa si usa per risolvere equazioni impossibili in $\mathbb{R}$, l'unità immaginaria è definita anche come $$\begin{align}
&\color{#61AFEF}i:=\sqrt{-1} \\
&\color{#61AFEF}i^2:= -1
\end{align}$$

> [!example]+ <font color="orange">Esempio</font>
>$$x^2+1=0\iff x^2=-1\iff x=\pm\sqrt{-1}\iff x=\pm i$$
>$$\sqrt{-16}=\sqrt{-1\cdot16}=\sqrt{-1}\cdot\sqrt{16}=i\cdot4=4i$$

Si trovano due tipi di *forme* dei numeri complessi:
- La [[023 Forma Algebrica di un Numero Complesso|Forma Algebrica]]
- La [[025 Forma Trigonometrica di un Numero Complesso|Forma Trigonometrica]]


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
- [YouMath](https://www.youmath.it/lezioni/analisi-matematica/numeri-complessi.html)