---
aliases:
    - Linguaggio riconosciuto da una MdT
tags:
    - corsi/informatica/elementi_teoria_computazione
paragrafo: Macchine di Turing
cssclasses:
    - 
---

> Sia $M = (Q,\Sigma, \Gamma,\delta,q_0,q_{accept}, q_{reject})$ una [[018 Macchine di Turing|Macchina di Turing]]. Il linguaggio **riconosciuto** (o _accettato_) da $M$ è il seguente
> $$\mathcal{L}(M)=\{w\in\Sigma^*\ |\ \exists u,v\in\Gamma^*,\ \textcolor{#61AFEF}{q_0\ w \to^*u\ q_{accept}\ v}\}=\{w\in\Sigma^*\ |\ M \text{ accetta } w\}$$
> Ovvero, è l'_insieme di stringhe $w$_ dove la [[021 Configurazione di una Macchina di Turing#Computazione su un input|computazione]] di $M$ su input $w$:
>
> -   Comincia da una [[021 Configurazione di una Macchina di Turing#^f0bd9d|Configurazione Iniziale]]
> -   Termina in una [[021 Configurazione di una Macchina di Turing#^f0bd9d|Configurazione di Accettazione]]
>
> Quindi $M$ [[022 Parole accettate e rifiutate da una MdT#Accettazione|accetta]] $w$.

^940e45

![[019 Funzione di Transizione delle Macchine di Turing#^d05899]]

> [!quote] Linguaggio Turing-Riconoscibile
> Un linguaggio $L$ si dice **Turing-Riconoscibile** se e solo se esiste una Macchina di Turing $M$ che lo _riconosce_. $$\mathcal{L}(M)=L$$

^5b96ec

---

[[000 Indice ETC|↖ Ritorna all'indice ↖]]

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

-   [[Terminologie Linguaggi MdT.canvas|Canvas dei linguaggi delle MdT]]
