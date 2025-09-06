---
aliases: 
tags:
  - corsi/matematica/analisi
paragrafo: Funzioni Trigonometriche
cssclasses:
  - 
---
Considerare un angolo $\alpha$ sulla [[018 Circonferenza Goniometrica|circonferenza goniometrica]].

![[Pasted image 20220321153828.png|400]]

Si indica con $P$ il punto intercettato dal secondo lato dell'angolo sulla circonferenza goniometrica. Tale punto si dice *punto associato all'angolo $\alpha$*. 
Siano poi $x_p$ ed $y_p$ l'ascissa e l'ordinata del punto $P$.
![[Pasted image 20220321154022.png|400]]



## Seno
>Dato un angolo $\alpha$ sulla circonferenza goniometrica, si dice **Seno dell'Angolo $\alpha$** l'*ordinata del punto $P$* associato ad $\alpha$, ovvero
>$$\sin(\alpha)=y_p$$

Sia $Q$ la proiezione del punto $P$ sull'asse $y$. Si viene così a formare un triangolo rettangolo $OPQ$, la cui ipotenusa $OP$, essendo un raggio della circonferenza goniometrica, misura $1$.

![[Pasted image 20220321155226.png|400]]

Si ha infatti che:
$$\sin(\alpha)=\frac{OQ}{OP}=\frac{OQ}{1}=OQ=y_p$$

## Coseno
>Dato un angolo $\alpha$ sulla circonferenza goniometrica, si dice **Coseno dell'Angolo $\alpha$** l'*ascissa del punto $P$* associato ad $\alpha$, ovvero
>$$\cos(\alpha)=x_p$$

Sia $R$ la proiezione del punto $P$ sull'asse $x$. Si viene così a formare un triangolo rettangolo $OPR$, la cui ipotenusa $OP$, essendo un raggio della circonferenza goniometrica, misura $1$.

![[Pasted image 20220321155449.png|400]]

Si ha infatti che:
$$cos(\alpha)=\frac{OR}{OP}=\frac{OR}{1}=OR=x_p$$

## Formula Seno e Coseno
È importante la seguente formula:
$$\color{#CC241D}1=\sin^2(x)+\cos^2(x)$$
Da cui si derivano:
$$\sin^2(x)=1-\cos^2(x)$$
$$\cos^2(x)=1-\sin^2(x)$$ ^37a9af

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
- [YouMath](https://www.youmath.it/lezioni/analisi-matematica/le-funzioni-elementari-e-le-loro-proprieta/144-seno-e-coseno-di-un-angolo.html)