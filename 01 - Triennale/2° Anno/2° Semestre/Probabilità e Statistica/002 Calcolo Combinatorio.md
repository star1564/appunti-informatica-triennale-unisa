---
aliases:
  - Principio Fondamentale del Calcolo Combinatorio
tags:
  - corsi/matematica/probabilità_statistica
paragrafo: Calcolo Combinatorio
inglese:
  - Counting
  - Multiplication Rule
cssclasses:
  - 
---
>Il **Calcolo Combinatorio**, è la parte del Calcolo della Probabilità che si occupa dello studio dei metodi per *raggruppare un numero finito di elementi*, e che si pone l'obiettivo di *contare il numero di possibili raggruppamenti* degli elementi per ciascun metodo.

> [!note] Principio Fondamentale del Calcolo Combinatorio
> Si realizzino $N$ scelte. Si supponga che la prima scelta abbia $n_1$ esiti possibili, la seconda scelta abbia $n_2$ esiti possibili e così via fino all'$N$-esima scelta che può avere $n_N$ esiti possibili. Allora gli $N$ esperimenti producono in tutto $\color{#CC241D}n_1\cdot n_2\cdot \dotsc \cdot n_N$ esiti possibili.
> 
> Questo va a produrre un [[013 Alberi#Albero Radicato|grafo ad albero]] ([[028 Alberi di Decisione|albero decisionale]]).
> 

^5e55f1

> [!example]- <font color="orange">Esempi</font>
>Supponiamo di voler organizzare una vacanza e di poter scegliere:
>- la meta tra: <font color="#00C575">Parigi, Londra e Monaco</font>;
>- il mezzo di trasporto tra: <font color="#FF6611">Treno, Aereo e Auto</font>;
>- il periodo tra vacanze di: <font color="#61AFEF">Natale e Pasqua</font>.
>
>In quanti modi diversi possiamo organizzare la vacanza?
>
>Possiamo rappresentare la situazione con un Grafo ad Albero:
>
>![[Pasted image 20230304143059.png|800]]
>
>Poiché ogni scelta termina nel livello più basso dell'albero, per sapere quante vacanze diverse possiamo organizzare, basta contare le foglie dell'albero, che sono $18$.
>Questo numero lo si può calcolare, come viene specificato nel principio fondamentale del calcolo combinatorio, con $\textcolor{#00C575}{3}\cdot\textcolor{#FF6611}{3}\cdot\textcolor{#61AFEF}{2} = 18$.
>---
>Si lanciano a caso due dadi da sei facce. Quanti sono i risultati possibili?
>Il lancio del primo dado dà esito del primo esperimento (<font color="#CC241D">6</font> possibilità diverse), e il lancio del secondo dado dà esito del secondo esperimento (<font color="#61AFEF">6</font> possibilità diverse).
>Per il principio fondamentale del calcolo combinatorio vi sono in tutto $\textcolor{#CC241D}{6}\cdot\textcolor{#61AFEF}{6}=36$ risultati possibili.
>
>![[Immagine 2023-03-04 152240.jpg]]
>
>---
>Vi sono 10 partite, ed ognuna può dare 3 tipi di risultati. Quanti sono i risultati possibili?
>
>I possibili risultati sono $3^{10}=\underbracket{3\cdot3\cdot\dotsc\cdot3}_{\text{10 volte}}=59\ 049$.


___
[[000 Indice PS|↖ Ritorna all'indice ↖]]
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
- [Studocu](https://www.studocu.com/it/document/best-notes-for-high-school-it/matematica/411-calcolo-combinatorio/15202041)
- [YouMath](https://www.youmath.it/lezioni/probabilita/calcolo-combinatorio.html)
