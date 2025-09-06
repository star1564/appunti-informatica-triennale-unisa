---
aliases: 
tags:
  - corsi/informatica/sistemi_operativi
paragrafo: Memoria
cssclasses:
  - 
---
Il **Trashing** è una condizione nel quale si spende più tempo nella paginazione che nella esecuzione dei processi per colpa dell'alto numero di [[035 Paginazione su Richiesta#PAGE FAULT|page fault]].

Per evitare il thrashing occorrerebbe fornire a un processo tutti i frame di cui necessita. Per sapere quanti frame servano a un processo si impiegano diverse tecniche.

### MODELLO DI LOCALITÀ
Una località è un insieme di pagine che vengono accedute insieme e che sono contemporaneamente in uso attivo. Ogni località può sovrapporsi e ogni processo si muove da una località all'altra.

> [!question]- Perché si verifica il trashing
> Perché la dimensione della località è maggiore del numero di frame assegnati.

### MODELLO DEL WORKING-SET
In questo modello si ha:
- $\color{#CC241D}\Delta$: Si considera una finestra temporale $\Delta$ chiamata *finestra working-set*, fondamentalmente è un numero fisso di riferimenti di pagina;
- $\color{#CC241D}WSS_i$: Si individua il numero totale di pagine cui $P_i$ si riferisce nel più recente $\Delta$ (che può variare nel tempo).
	- Se $\Delta$ è troppo piccolo non comprenderà l'intera località;
	- Se $\Delta$ è troppo grande può sovrapporre parecchie località;
	- Se $\Delta$ è infinito, allora il working set è l'insieme delle pagine toccate durante l'esecuzione del processo.

Con $\textcolor{#CC241D}{D} = \sum WSS_i$  si riferisce alla richiesta globale dei frame.

Si va a confrontare $D$ con il numero reale di frame nel disco ($\color{#CC241D}m$ è il numero di frame).

Se $D>m$ allora si ha trashing, quindi bisogna sospendere uno dei processi.

Per individuare quali sono le pagine che sono state utilizzate nella finestra di tempo $\Delta$:

Man mano che la finestra scorre può aumentare o diminuire il numero di frame dati al processo.

Nell'esempio in t1+1 la pagina 2 scompare, quindi il frame ad essa allocato viene considerato libero (può eventualmente essere usato da un altro processo il cui working set invece cresce).

![[Pasted image 20221216142920.png]]

> [!example]- <font color="orange">Esempio</font>
![[Pasted image 20221217100552.png]]



___
[[000 Indice SO|↖ Ritorna all'indice ↖]]

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