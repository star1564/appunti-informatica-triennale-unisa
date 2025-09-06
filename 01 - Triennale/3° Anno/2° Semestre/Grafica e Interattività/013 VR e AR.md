---
aliases: 
tags:
  - corsi/informatica/grafica_interattivita
paragrafo: Teoria Grafica
cssclasses:
  - 
---
## Realtà Virtuale
>La **Realtà Virtuale** (Virtual Reality, *VR*) è il termine che indica una *realtà simulata* attraverso l'utilizzo di computer e interfacce dedicate. Permette all'utente di interagire in maniera immersiva con l'ambiente 3D ed i suoi oggetti.

Questa crea un ambiente *completamente virtuale*, quindi *non integra* aspetti dell'ambiente reale attorno all'utente.

![[Pasted image 20240601120402.png|600]]

> [!info] Unity
> Molti giochi in VR utilizzano [[U_001 Unity|Unity]] come [[009 Motore Grafico|Gaming Engine]], in quanto dispone di SDK accessibili per lo sviluppo VR.
> 


## Realtà Aumentata
>La **Realtà Aumentata** (Augmented Reality, *AR*) è una tecnologia che *arricchisce la percezione del mondo reale* con la *sovrapposizione di informazioni digitali* generate al computer. 

![[Pasted image 20240601120900.png|700]]

A differenza della realtà virtuale che crea un ambiente completamente artificiale, la AR *integra elementi virtuali nell'ambiente reale* che circonda l'utente.

![[Pasted image 20240601121008.png|600]]

## Tecniche di visualizzazione per l'AR
Le tecniche di visualizzazione per l'AR si possono classificare in tre categorie.

### Visori video see-through
Sono i più vicini alla realtà virtuale, in quanto usano due telecamere (una per ciascun occhio), con le quali acquisiscono l'immagine reale che viene poi fusa con quella di sintesi per essere in seguito inviate agli occhi tramite due display.

![[Pasted image 20240601121533.png]]

> [!success] Vantaggi
> - Migliori risultati in termini di complessità degli oggetti virtuali sovrapposti al mondo reale.

> [!failure] Svantaggi
> - Richiedono sistemi di messa a fuoco automatica al fine di garantire la nitidezza del punto osservato. 
> - In generale i tempi di risposta, ovviamente inferiori a quelli dell'occhio umano, rendono l'esperienza di utilizzo poco confortevole.


### Visori optical see-through
Rispetto ai primi, questi lasciano vedere all'utente il mondo reale così come è, mentre i contenuti aumentati vengono aggiunti alla scena reale attraverso la loro impressione su particolari lenti trasparenti.

![[Pasted image 20240601121544.png]]

> [!success] Vantaggi
> - Non richiedono sistemi di messa a fuoco automatica

> [!failure] Svantaggi
> - La qualità degli oggetti virtuali aumentati non è paragonabile con i risultati ottenuti con i visori video see-through per via della trasparenza delle lenti su cui sono impressi e fusi con la realtà.


### Display di proiezione
Si avvale di sistemi a proiezione per illuminare gli oggetti reali con apposite immagini generate dal computer.

![[Pasted image 20240601121556.png]]

> [!failure] Svantaggi
> I display di proiezione risultano troppo sensibili alla riflettività e alle caratteristiche fisiche degli oggetti illuminati. Ad esempio, corpi opachi e scuri limitano il tipo di informazione che può essere visualizzata oltre al fatto che l'allineamento dei proiettori con la superficie di proiezione è un fattore critico per la generazione di un effetto credibile.


___
[[000 Indice Grafica|↖ Ritorna all'indice ↖]]

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