---
aliases: 
 
tags:
  - corsi/informatica/grafica_interattivita
paragrafo: Teoria Grafica
cssclasses:
  - 
---
## Visione

>La **Visione** (o visualizzazione) è il processo di *percezione degli stimoli [[004 Illuminazione|luminosi]]*, che permette di vedere gli *oggetti* le loro rappresentazioni grafiche.

Queste rappresentazioni sono dei *paradigmi di comunicazione di tipo visivo*, che permettono di comunicare superando le barriere imposte dall'uso di lingue differenti (diagrammi, mappe, grafici, rotte, ecc.).


## La Percezione e l'Occhio
>La **Percezione** è legata al concetto della visione e permette all'uomo di *elaborare gli stimoli visivi*

I **Fotorecettori** sono neuroni specializzati che si trovano sulla retina dell'occhio. La luce che arriva sul fondo dell'occhio viene "*tradotta*" in segnali bioelettrici che giungono al cervello attraverso il nervo ottico.

![[Pasted image 20240601100417.png|400]]

Ci sono due tipi di fotorecettori:
- I **coni** sono dedicati alla *visione dei colori* e ne esistono di tre tipi diversi, per il rosso, il verde ed il blu.
- I **bastoncelli** sono più sensibili al movimento e sono impiegati per la visione in *condizioni di bassa luminosità*.

![[Pasted image 20240601091841.png|400]]

La percezione coinvolge anche svariate attività del cervello umano.

### Percezione della profondità
I nostri occhi utilizzano due principali "*indizi fisiologici di profondità*" per percepire la **profondità**:
- L'**Accomodazione** si riferisce alla capacità dell'occhio di cambiare la sua lunghezza focale per *mettere a fuoco oggetti a distanze diverse*. Il cristallino dell'occhio è una struttura flessibile che può cambiare forma grazie all'azione dei muscoli ciliari. 
	- Quando mettiamo a fuoco un oggetto *vicino*, i muscoli ciliari si *contraggono* e la lente diventa più spessa. 
	- Al contrario, quando mettiamo a fuoco un oggetto *lontano*, i muscoli ciliari si *rilassano*, facendo sì che la lente diventi più sottile.
- La **Convergenza** si riferisce alla rotazione degli occhi verso l'interno per *mettere a fuoco gli oggetti vicini*. Quando guardiamo un oggetto vicino, i nostri occhi devono convergere verso l'interno in modo che le immagini dell'oggetto cadano su punti corrispondenti della retina di entrambi gli occhi. ^aa2cbb

![[Pasted image 20240601114456.png]]

Combinando le informazioni provenienti dall'accomodazione e dalla convergenza, il cervello umano è in grado di creare una percezione tridimensionale del mondo.


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