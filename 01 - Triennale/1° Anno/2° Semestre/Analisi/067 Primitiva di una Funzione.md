---
aliases: 
tags:
  - corsi/matematica/analisi
paragrafo: Integrali
cssclasses:
  - 
---
>Sia $f:[a,b]\to\mathbb{R}$ una [[011 Funzioni|funzione]]. Si dice che $\color{#CC241D}F(x)$ è una **Primitiva di $\color{#CC241D}f(x)$** se la funzione è [[046 Derivata di una Funzione#Funzione Derivabile|derivabile]] e se $$\color{#61AFEF}F'(x)=f(x),\forall x\in(a,b)$$
>Ovvero *se $f(x)$ è la derivata di $F(x)$*.

Se una funzione $f:[a,b]\to\mathbb{R}$ è [[035 Funzione Continua|continua]], allora questa *Ammette delle Primitive*.

> [!example]+ <font color="orange">Esempio</font>
> $F(x)=\frac{x^2}{2}$ è una primitiva di $f(x)=x$.
>
> Infatti:
> $$F'(x)=\frac{d}{dx}\left[ \frac{x^2}{2}\right]=\frac{2x}{2}=x$$


> [!note] La primitiva non è mai unica
> Se $F(x)$ è una primitiva, allora lo è anche $F(x)+c$, con $c\in\mathbb{R}$ costante di integrazione. 
> 
> Infatti, a tutte le primitive $F(x)$, si può aggiungere la quantità $+c$, perché [[047 Derivate Fondamentali#F01 Funzione Lineare e Costante 759638 Funzione Costante|la derivata di una costante è zero]].
>
>> [!example]- <font color="orange">Esempio</font>
>> $F(x)=\frac{x^2}{2}+1$ è una primitiva di $f(x)=x$.
>>
>> Infatti:
>> $$F'(x)=\frac{d}{dx}\left[ \frac{x^2}{2}+1\right]=\frac{2x}{2}+0=x$$


Inoltre se $F_1(x)$ e $F_2(x)$ sono due primitive di $f(x)$ in uno stesso intervallo, allora $F_1(x)-F_2(x)$ è una [[F01 Funzione Lineare e Costante#^759638|funzione costante]].

## Come calcolare un Integrale Definito

![[072 Teorema Fondamentale del Calcolo Integrale#Secondo Teorema Fondamentale (Torricelli-Barrow)]]




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
- [YouMath (Primitive)](https://www.youmath.it/lezioni/analisi-matematica/integrali/597-primitive-e-integrale-indefinito.html)
- [Elia Bombardelli](https://youtu.be/4hfhVhnzuUw)
