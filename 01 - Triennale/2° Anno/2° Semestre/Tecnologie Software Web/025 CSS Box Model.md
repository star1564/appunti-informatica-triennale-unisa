---
aliases: 
tags:
  - corsi/informatica/tecnologie_software_web
paragrafo: CSS
cssclasses:
  - 
---
>Il **Box Model** è essenzialmente una scatola che *avvolge* ogni [[007 Elementi e Attributi HTML#ELEMENTO|elemento di HTML]].

![[Pasted image 20230504153720.png|500]]

Tutto quello che riguarda il [[024 CSS|CSS]] è influenzato dal box model.

> [!tip]+ Debug dello stile CSS su un browser
> Per vedere come si comporta il box model in una pagina, o per vedere altre informazioni utili come la console:
> - Aprire l'analisi della pagina (Firefox: `F12`, Chrome: `Ctrl` + `Shift` + `I`);
> - Selezionare l'elemento con mouse attraverso l'opportuna funzione in alto a sinistra.

---
### Parti del Box Model
Una *Box* è composta da diverse parti:
- **Content** (area del contenuto): *ogni elemento HTML* ha un suo contenuto (sia esso testo, immagini, video, ecc.). Di questo è possibile definirne l'*Altezza (`Height`)* e la *Larghezza (`Width`)*. ![[Pasted image 20230504154045.png|400]]
- **Padding** (cuscinetto): è uno spazio vuoto *tra il contenuto ed il bordo dell'elemento*. ![[Pasted image 20230504154220.png|400]]
- **Border** (bordo): è lo spazio riservato per il *bordo*, di cui possiamo definirne il colore, stile e spessore. ![[Pasted image 20230504154317.png|400]]
- **Margin** (margine): è lo spazio *tra l'elemento e gli altri elementi adiacenti*.
	-  In caso due box siano allineate *orizzontalmente*, la loro distanza è la somma dei due margini;
	- Se sono allineate *verticalmente*, la loro distanza è il massimo fra il margine inferiore del primo e il margine superiore del secondo (*margin collapsing*).

![[Immagine 2023-05-04 160330.png|400]]

___
[[000 Indice TSW|↖ Ritorna all'indice ↖]]

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
- [Fireship](https://www.youtube.com/watch?v=Qhaz36TZG5Y)

